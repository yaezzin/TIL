# Autoscailing


## Launch configure 

```terraform
resource "aws_launch_configuration" "launchconfig" {
  name          = "launch-config"
  image_id      = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
  key_name      = aws_key_pair.mykeypair.key_name
  security_groups = [
      aws_security_group.allow_ssh.id
  ]
}
```

```aws_launch_configuration```
* Auto Scaling 그룹에서 사용할 EC2 인스턴스의 설정을 정의하는 데 사용

## AutoScailing Group

```terraform
resource "aws_autoscaling_group" "autoscailing-group" {
  name = "autoscaling group"
  
  vpc_zone_identifier = [
      # 서브넷 2개 지정
  ]

  launch_configuration = aws_launch_configuration.launchconfig.name
 
  min_size           = 1
  max_size           = 2 

  health_check_type = "EC2"
  health_check_grace_period = 300
  force_delete = true
}
```

```aws_autoscaling_group```
* AWS Auto Scaling 그룹을 정의

```vpc_zone_identifier```
* Auto Scaling 그룹이 사용할 VPC(Virtual Private Cloud)의 서브넷을 지정
* 2개 이상의 서브넷을 지정하는 것이 일반적
* 하나의 서브넷이 다운되거나 이용 불가능한 경우에도 Auto Scaling 그룹은 다른 서브넷에 인스턴스를 생성하여 서비스를 지속함

```launch_configuration```
* Auto Scaling 그룹의 구성 정보를 지정


## AutoScailing Policy

```terraform
resource "aws_autoscaling_policy" "policy" {
  name                   = "policy-test"
  scaling_adjustment     = 1
  adjustment_type        = "ChangeInCapacity"
  cooldown               = 300

  autoscaling_group_name = aws_autoscaling_group.autoscailing_group.name
  policy_type            = "SimpleScailing" 
}
```

```aws_autoscaling_policy```
* Auto Scaling 그룹에 대한 정책을 설정
* 예를 들어, 트래픽이 증가하면 인스턴스를 추가하고, 트래픽이 감소하면 인스턴스를 제거하는 정책을 설정할 수 있음

```scaling_adjustment```
* 이 정책이 적용될 때 조정할 인스턴스 수
* 여기서는 1로 설정되어 있으므로 Auto Scaling 그룹의 현재 크기에 1을 더하거나 1을 빼서 인스턴스 수를 조정함

## Cloud Watch

```
resource "aws_cloudwatch_metric_alarm" "cpu-alram" {
  alarm_name                = "cpu-alram"

  comparison_operator       = "GreaterThanOrEqualToThreshold" // 비교연산자 지정
  evaluation_periods        = 2
  metric_name               = "CPUUtilization"
  namespace                 = "AWS/EC2"
  period                    = 120
  statistic                 = "Average"
  threshold                 = 30 // 임계값 설정
  
  dimensions = {
      AutoScailingGroupName = aws_autoscaling_group.autoscailing-group.name
  }

  actions_enabled = true
  alarm_actions = [
       aws_autoscailing_policy.policy.cpu-alram.arn
  ]
}
```

```Cloud Watch```
* Auto Scaling 그룹의 대상 상태를 모니터링하고 조정하기 위해 CloudWatch 경보를 설정
* EC2 인스턴스의 CPUUtilization 지표를 모니터링하고, 특정 조건을 충족할 때 알림을 보내거나 Auto Scaling 작업을 트리거함

```metric_name```
* Alarm이 모니터링할 Metric의 이름을 설정 - CPUUtilization은 CPU 사용률

```alarm_actions```
* CloudWatch 알람이 발생할 때 Auto Scaling 정책이 실행되도록 함

## SNS

```terraform
resource "aws_sns_topic" "cpu-sns" {
  name = "cpu-topic"
}
```

```terraform
resource "aws_autoscaling_notification" "example_notifications" {
  group_names = [
    aws_autoscaling_group.bar.name,
    aws_autoscaling_group.foo.name,
  ]

  notifications = [
    "autoscaling:EC2_INSTANCE_LAUNCH",
    "autoscaling:EC2_INSTANCE_TERMINATE",
    "autoscaling:EC2_INSTANCE_LAUNCH_ERROR",
    "autoscaling:EC2_INSTANCE_TERMINATE_ERROR",
  ]

  topic_arn = aws_sns_topic.example.arn
}
```
* EC2 인스턴스가 시작되거나 종료될 때, 그리고 시작 또는 종료 중에 오류가 발생할 때 알림이 오도록 설정
