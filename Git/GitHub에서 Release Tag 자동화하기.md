## GitHub에서 Release Tag 자동화하기

<img width="641" alt="스크린샷 2023-10-21 오후 8 13 53" src="https://github.com/yaezzin/TIL/assets/97823928/ae036fdc-3dd5-4e7f-9005-e7a8bcb1214c">

* Main 브랜치는 출시 가능한 프로덕션 코드를 모아두는 브랜치이며, 배포된 각 버전을 Tag를 이용해 표시한다.
* Develop 브랜치는 다음 버전 개발을 위한 코드를 모아두는 브랜치로 개발이 완료되면 Main 브랜치로 merge된다.
* Feature 브랜치는 하나의 기능을 개발하기 위한 브랜치로, Develop 브랜치에서 분기되며, 기능이 개발 완료되면 다시 Develop 브랜치로 merge된다.
* Release 브랜치는 소프트웨어 배포를 준비하기 위한 브랜치 Develop 브랜치에서 배포될 준비가 됐다면 Release 브랜치로 merge된다.


## 자동화 하기

1. Dev barnch가 release로 merge될때 CI 작동
2. Release branch가 main으로 merge될때 CD 작동
3. merge commit 으로부터 버전 정보 추출하여 추출된 버전 정보를 통해 release/tag 생성
4. Helm chart의 버전 정보를 변경하여 ArgoCD에서 버전 정보를 탐지하도록

## CI.yaml

```yaml
name: CI

on:
  workflow_dispatch:
  pull_request:
    branches:
      - "release"
    types:
      - closed
      
env:
  IMAGE: ${{ secrets.NCP_CONTAINER_REGISTRY }}/app
  IMAGE_TAG: ${{ secrets.NCP_CONTAINER_REGISTRY }}/app:latest

jobs:
  lint:
    name: Check lint (black)
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup python
        uses: actions/setup-python@v4
        with:
          python-version: "3.11"

      - name: Install black
        run: pip install black

      - name: Check black
        run: black --check app

  build_push_to_ncp:
    needs: lint
    name: Build Image
    runs-on: ubuntu-latest
    outputs:
      NEW_TAG: ${{ steps.check_tag.outputs.NEW_TAG }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Docker buildx
        uses: docker/setup-buildx-action@v2

      - name: Login to NCR
        uses: docker/login-action@v2
        with:
          registry: ${{ secrets.NCP_CONTAINER_REGISTRY }}
          username: ${{ secrets.NCP_ACCESS_KEY }}
          password: ${{ secrets.NCP_SECRET_KEY }}

      - name: Bump version and push tag
        id: tag_version
        uses: mathieudutour/github-tag-action@v6.1
        with:
          github_token: ${{ secrets.DEL_APP }}
          default_bump: false
          release_branches: release
          # fix: - patch, feat: - minor, BREAKING CHANGE: - major
      
      - name: Check if new_tag exists
        id: check_tag
        run: |
          echo "NEW_TAG=${{ steps.tag_version.outputs.new_tag }}" >> $GITHUB_ENV

      - name: Create a GitHub release
        if: ${{ env.NEW_TAG != '' }}
        uses: ncipollo/release-action@v1
        with:
          token: ${{ secrets.DEL_APP }}
          tag: ${{ steps.tag_version.outputs.new_tag }}
          name: Release ${{ steps.tag_version.outputs.new_tag }}
          body: ${{ steps.tag_version.outputs.changelog }}

      - name: create TAG
        id: created_tag
        run: |
          if [[ "${{ env.NEW_TAG }}" != "" ]]; then
              echo "Using version tag: ${{ env.NEW_TAG }}"
              echo "TAG=${{ env.NEW_TAG }}" >> $GITHUB_ENV
          else
              TIMESTAMP=$(date '+%s')
              echo "Using timestamp tag: $TIMESTAMP"
              echo "TAG=$TIMESTAMP" >> $GITHUB_ENV
          fi
      
      - name: Build and Push
        if: ${{ env.NEW_TAG != '' }}
        uses: docker/build-push-action@v4
        with:
          context: app
          push: true
          tags: ${{ env.IMAGE_TAG }},"${{ env.IMAGE }}:${{ env.TAG }}"
          platforms: linux/amd64,linux/arm64
```

## CD.yaml

```yaml
name: CD

on:
  workflow_dispatch:
  pull_request:
    branches:
      - "main"
    types:
      - closed

env:
  IMAGE: ${{ secrets.NCP_CONTAINER_REGISTRY }}/app
  IMAGE_TAG: ${{ secrets.NCP_CONTAINER_REGISTRY }}/app:latest

jobs:
  deploy:
    name: deploy new version
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          ref: main
          token: ${{ secrets.DEL_APP }}
      
      - name: Get Latest Tag
        id: get-latest-tag
        run: |
          LATEST_TAG=$(curl -s -H "Authorization: token ${{ secrets.DEL_APP }}" https://api.github.com/repos/directory/tags | jq -r '.[0].name')
          echo "Latest Tag is $LATEST_TAG"
          echo "LATEST_TAG=$LATEST_TAG" >> $GITHUB_ENV
        
      - name: Update appVersion in Chart.yaml
        if: ${{ env.LATEST_TAG != ''}}
        run: |
          echo "Using version tag: ${{ env.LATEST_TAG }}"
          sed -ie 's/appVersion: ".*"/appVersion: "'${{ env.LATEST_TAG }}'"/g' helm-chart/Chart.yaml

      - name: Commit files
        if: ${{ env.LATEST_TAG != ''}}
        run: |
          git add .
          git config --local user.email "yaezzin@gmail.com"
          git config --local user.name "github-actions[bot]"
          git commit -a -m "update chart tag"

      - name: Push changes
        if: ${{ env.LATEST_TAG != ''}}
        uses: ad-m/github-push-action@master
        with:
          branch: main
          github_token: ${{ secrets.DEL_APP }}
```





