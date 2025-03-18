## 프로젝트 설정 및 환경 변수 설정

#### 1. 프로젝트 디렉토리 생성

```
$ mkdir {project_name}
$ cd {project_name}
```

#### 2. 가상 환경 설정

```
$ python3 -m venv venv
$ source venv/bin/activate
```

#### 3. Django 설치

```
$ pip install django djangorestframework python-dotenv
```

#### 4. Django 프로젝트 생성

```
django-admin startproject {project_name} .
```
* 해당 폴더 안에 settings.py 등의 파일들이 담김


#### 5. 환경 변수 파일 생성

```
DJANGO_SECRET_KEY = "my-secret-key"
````
* 프로젝트 루트에 .env파일을 생성 후 안에 환경변수값들을 넣는다



#### 6. settings.py에서 환경 변수 로드


```python

import os
from pathlib import Path
from dotenv import load_dotenv

BASE_DIR = Path(__file__).resolve().parent.parent # manage.py가 있는 폴더

load_dotenv(BASE_DIR / '.env')  # .env 파일 로드

SECRET_KEY = os.getenv("DJANGO_SECRET_KEY")
```

#### 7. 서버 실행

```
$ python manage.py runserver
```


