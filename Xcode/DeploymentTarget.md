# Deployment Target

> `Project Deployment Target` vs `Target Deployment Target`

<br/>



## Deployment Target?

* Deployment Target은 해당 앱이 지원하는 최소 버전
* 보통 기업에서는 최신 버전을 기준으로 `-2` 또는 `-3` 정도 지원한다고 함



## Project iOS Deployment Target vs Target iOS Deployment Target

* 하나의 프로젝트에 여러 개의 타겟이 존재할 수 있음
* 각 타겟은 독립적임
* 앞선 두 가지를 유추했을 때 타겟 설정이 프로젝트 설정을 덮어쓴다는 것을 알 수 있음
* 즉, 서로 버전이 다를 경우는 타겟의 Deployment Target이 최소 버전이 됨