# App Transport Security(ATS)

> ATS (App Transport Security)라는 네트워킹 기능은 모든 앱 및 앱 확장에 대한 개인 정보 보호 및 데이터 무결성을 향상시킵니다. - [NSAppTransportSecurity](https://developer.apple.com/documentation/bundleresources/information_property_list/nsapptransportsecurity/)

* URLSession 클래스를 사용하는 URL 로딩 시스템으로 이루어진 모든 HTTP 연결을 HTTPS로 강제화

* iOS 9.0, macOS 10.11 SDK 부터 기본적으로 동작하는데, iOS 10.0, macOS 10.12 이상부터 조금 바뀜
* 개발할 때 이전 SDK에 연결하면 앱이 실행되는 운영 체제 버전에 관계없이 ATS가 비활성화
* [Provide Justification for Exceptions](https://developer.apple.com/documentation/security/preventing_insecure_network_connections#3138036)에 제시된 대로 ATS 관련 키 값을 `YES`로 설정했을 때 앱 스토어에 보안에 대해 검증하지 않으면 리젝당할 수 있음
* `Network` 프레임워크 또는 `CFNetwork` 프레임워크 같은 하위 수준 네트워킹 인터페이스에 대한 앱 호출에는 미적용

## 모든 ATS 옵션 해제

1. `Info.plist`에서` App Transport Security Settings` 키를 추가
2. 하위에 `Allow Arbitrary Loads` 를 추가하고  `YES` 로 설정

## 특정 도메인만 옵션 해제

1. `Info.plist`에서` App Transport Security Settings` 키를 추가
2. 하위에 `Allow Arbitrary Loads` 를 추가하고  `NO` 로 설정
3. `Allow Arbitrary Loads` 와 동일한 레벨에 `NSExceptionDomains` 추가
4. 하위에 ATS 해제를 허용할 도메인 추가 후, 해당 도메인 하위에 `NSIncludesSubDomains` 와 `NSExceptionAllowsInSecureHTTPLoads` 값을 `YES` 로 설정하여 추가

<img width="611" alt="스크린샷 2021-05-18 오후 4 24 26" src="https://user-images.githubusercontent.com/73573732/118609259-891a3a00-b7f5-11eb-896e-4d9ecb537014.png">

## 종류

* `NSAllowsArbitraryLoads`
  * 모든 도메인에 대해 ATS 제한 비활성화
  * 다음 키들이 함께 존재하는 경우 해당 키의 값은 무시됨 (`NO`로 설정)
    * `NSAllowsArbitraryLoadsForMedia`
    * `NSAllowsArbitraryLoadsInWebContent`
    * `NSAllowsLocalNetworking` 
* `NSAllowsArbitraryLoadsInWebContent`
  * `URLSession` 연결에 영향을 주지 않고 ATS에서 웹 뷰에서 안전하지 않는 로드를 허용
  * 웹 뷰는 다음과 같음
    * `WKWebView`
    * ~~`UIWebView`~~ (Deprecated)
    * ~~`WebView`~~ (Deprecated)
* `NSAllowsArbitraryLoadsForMedia`
  * `URLSession`을 통한 연결에 영향을 주지 않고 `AVFoundation` 을 사용하여 만든 요청에 대해 모든 ATS 제한을 비활성화
  * 개인화된 정보가 포함되지 않은 암호화 된 미디어를 로드하는 곳에서만 사용
* `NSAllowsLocalNetworking`
  * iOS 9 및 macOS 10.11 에서의 ATS는 비정규 도메인, `.local` 도메인 및 IP 주소 연결을 비허용하므로, 해당 방법으로 접근하려면 `NSAllowsArbitraryLoads` 와 `NSAllowsLocalNetworking` 설정을 변경해줘야함
  * iOS 10 및 macOS 10.12 이상부터는 위 3가지 연결을 지원
  * 구 버전의 iOS를 지원하지 않더라도 해당 키의 값은 `YES`로 명시해놓는 것이 좋다고 애플에서 권장
* `NSExceptionDomains`
  * 특정 도메인 이름을 문자열로 지정하여 ATS에 대해 예외 처리
  * 도메인 이름은 문자열로 지정하며, 다음 규칙을 따르도록 작성
    * 소문자로 작성할 것
    * 포트 번호를 포함시키지 말 것
    * 숫자로 된 IP 주소를 사용하지 말 것
    * 의도한 경우가 아니라면 끝에 `.`을 붙이지 말 것
    * 와일드 카드(`*`)를 사용하지 말 것
