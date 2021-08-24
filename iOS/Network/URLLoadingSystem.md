# URL Loading System

> ## Reference
>
> 1. Apple Developer Documentation - [URL Loading System](https://developer.apple.com/documentation/foundation/url_loading_system)
> 2. Apple Developer Documentation - [URLSessionConfiguration](https://developer.apple.com/documentation/foundation/urlsessionconfiguration)
> 3. Ray Wenderlich - [URLSession Tutorial: Getting Started](https://www.raywenderlich.com/3244963-urlsession-tutorial-getting-started)



표준 인터넷 프로토콜을 통해 URL과 상호작용 및 서버와 통신한다.

해당 작업은 비동기로 처리되기 때문에, 앱의 데이터, 응답 및 오류가 도착하는 즉시 처리할 수 있다.

`URLSession` 인스턴스를 생성하면, 데이터를 앱으로 가져와 반환, 파일 다운로드, 원격에 데이터 업로드 등 실질적인 작업을 수행하는 하나 이상의 `URLSessionTask`를 생성할 수 있다.

<img width="500" alt="URLSession 모식도" src="https://user-images.githubusercontent.com/73573732/130601707-c73a9ac1-3d8e-41b0-bf0e-321b83ee5691.png">

쿠키 및 캐시, 셀룰러 네트워크 상태에서의 연결 허용 여부 등 네트워크 및 세션 설정에 관한 것은 `URLSessionConfiguration` 객체를 통해 제어할 수 있다. `URLSessionConfiguration`의 유형은 다음과 같다.

- `default` : 기본 값. 영구 디스크 기반 캐시를 사용하고, 사용자의 키체인에 자격 증명을 저장하도록 설정한다.
- `ephemeral` : 비공개 세션. 세션 관련 데이터를 디스크에 저장하지 않고 RAM에 저장하도록 설정한다.
- `background` : 세션이 백그라운드에서 업로드 및 다운로드 작업을 수행할 수 있도록 설정한다.

하나의 세션으로도 여러 개의 작업을 수행할 수 있으며, 세션이 백그라운드에서 동작할 수 있도록 구성할 수 있다.

