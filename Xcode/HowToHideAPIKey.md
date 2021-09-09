# API Key를 숨기는 방법

> ## Reference
>
> 1. NSHipster - [애플리케이션의 민감한 정보를 보호하는 방법](https://nshipster.co.kr/secrets/)
> 2. Hong3의 개발블로그 - [iOS - 환경변수 분리하기](https://hongdonghyun.github.io/2020/06/iOS-환경변수-분리하기/)

<br/>



## 경위

API를 통해 특정 데이터를 받아오는 앱을 개발하는 도중, GitGuardian에서 "개발하던 프로젝트에 토근이 노출되어 있는 거 같은데, 확인해 봐" 라는 내용의 메일이 왔다. 소스코드를 푸쉬한지 1분도 채 지나지 않은 시점이었다.

<img width="528" alt="GitGuardian에서 온 메일" src="https://user-images.githubusercontent.com/73573732/132616542-1df4c540-d44c-44c4-b780-263e0243626a.png">

<br/>



## 원인

말 그대로 API Key를 상수로 만들어서 `String` 타입으로 저장해 사용하고 있었기 떄문이다.

<br/>



## 문제

현재 나는 무료 API를 사용하고 있지만, 찾아보니 여러 사람들이 해당 문제에 대해 인지하지 못하고 AWS나 다른 API 키들을 올렸다가 금전적인 손해를 본 경우도 많다고 한다.

API Key를 코드에 문자열로 저장해서 사용한 후, 배포를 위해 아카이브 하면 당연히 바이너리화 되니까 문제가 없을 거라고 생각할 수 있다.

하지만 Radare2 같은 리버스 엔지니어링 도구가 있으면 컴파일된 파일도 읽을 수 있다고 한다.

코드에서 민감한 정보를 관리하는 것이 얼마나 중요한 것인지 인지시켜준 GitGuardian에게 무한한 감사를 보낸다.

<br/>



## 해결

1. 우선 .xcconfig 파일을 생성한다. 생성할 때, Target은 지정하지 않는다. Debug와 Release를 다르게 해서 두 개의 파일을 생성해도 과정은 동일하다. 하지만 편의상(배포용이 아니기도 하고) 하나로 Debug와 Release 를 지원했다.

    <img width="718" alt="xcconfig 생성" src="https://user-images.githubusercontent.com/73573732/132741257-d4e6e5aa-ac7d-48e7-bf7f-78dd9bfe3259.png">

2. .xcconfig 파일에 API Key를 저장할 환경 변수를 생성하고 값을 지정해줬다.

    <img width="708" alt="API Key 환경 변수 생성" src="https://user-images.githubusercontent.com/73573732/132742080-dd553d80-c239-4779-90a4-0ee0cc848e54.png">

3. 가장 중요한, .gitignore에 `*.xcconfig`를 추가해준다. 환경 변수로 만들어 준 것은 중요한 내용을 노출시키지 않기 위해서였다. 하지만 git에 .xcconfig가 올라가면 그대로 값이 노출되기 때문에 아무 의미가 없는 행위가 된다.

4. Info.plist에 아까 추가했던 환경 변수를 키와 값으로 분리해서 기입한다. 키는 변수명과 동일하게 작성하고, 값은 $(변수명)의 형식으로 넣어준다.

    <img width="691" alt="Info.plist" src="https://user-images.githubusercontent.com/73573732/132744253-e4da3234-62c4-4211-9b86-b03d96b3df48.png">

5. 프로젝트의 `Configuration`에 생성해준 .xcconfig 파일을 적용해준다. 위에서도 설명했듯이, 기본적으로 Debug와 Release 파일을 다르게 적용할 수 있다.

    <img width="812" alt="Project 설정에서 Configuration 적용" src="https://user-images.githubusercontent.com/73573732/132745567-3f7e41d7-1c7c-4b56-b3ff-641ac0e739a1.png">

6. 코드에 해당 키 값을 불러온다.

    ```swift
    Bundle.main.infoDictionary?["$(환경변수명)"]
    ```

    나는 아래와 같이 불러왔다.

    <img width="651" alt="코드 예시" src="https://user-images.githubusercontent.com/73573732/132747054-00b14f58-ec46-404e-a9bd-d59fe6a8cb7b.png">

> ### Note
>
> 만약 Cocoapods를 사용하고 있다면 환경 변수쪽에서 오류가 날 수 있다.
>
> 이럴 경우 다음과 Pod 전용 xcconfig가 생성되는데, 다음과 같이 추가하면 해결될 수 있다.
>
> ```markdown
> #includ "Pods/Target Supprot Files/프로젝트명/프로젝트명.debug.xcconfig"
> ```

<br/>



## 맺음

이렇게 API 키 값을 숨기는 방법을 적용해봤다.

하지만 클라이언트에서 완벽하게 민감한 정보를 숨기는 방법은 없으며, 

위에서 적용했던 방법도 사실 해커로부터 안전하지 않다고 한다.
(내부자이거나, 내부자의 컴퓨터 자체를 해킹한다면, 해커가 쉽게 정보를 볼 수 있다.)

다음 스텝으로는 민감한 정보를 GYB를 통해 Swift와 Python 코드를 조합하여 난독화하는 방법이 있다.
(단, Python에 대한 이해가 다소 필요하다.)

하지만 이 방법 또한 해킹에서부터 완전히 안전할 수 없다.

하지만 어쩔 수 없이 해야만 하는 상황이라면, 난독화를 하는 것이 그나마 안전하다고 한다.
(이론적으로 오류가 있지만, 실전에서는 효과적인 해결책이 될 수 있다고 한다.)

전문가 들이 말하길, 가장 안전한 방법은 민감한 정보를 **클라이언트에 포함시키지 않는 것**이라고 한다.

그들은 **민감한 정보를 관리하는 것을 해결해야할 문제가 아니라 피해야 할 안티 패턴으로 봐야한다**고 한다.

> 비밀을 지키는 단 하나의 방법은 비밀을 만들지 않는 것이다.
>
> *Julian Assange - NSHipster*
