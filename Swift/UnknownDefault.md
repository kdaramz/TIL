# `@unknown default`에 대한 고찰

<br/>



## `@frozen` 과 `@unknown`

Swift 공식 문서의 프로퍼티 래퍼 부분을 보면 문서의 정의에서 자주 보이는 `@frozen`, `@unknown` 부터, 코드를 작성하다가 종종 볼 수 있는 `@available` 들 까지 심상치 않은 친구들을 볼 수 있다.

여러 종류가 있지만, 이번에는 약간의 `@frozen`과 조금 더 얹은 `@unknown`에 대해 공부해보려고 한다.

이전에 `@frozen`에 대해 혼자 공부한 적이 있는데, 대강 내용은 이랬다.

>   #### `@frozen`
>
>   -   더 이상의 추가적인 케이스 또는 값이 추가되지 않음을 명시하는 프로퍼티 래퍼이다.
>   -   명시해줌으로써 퍼포먼스의 상승을 끌어올릴 수 있다.
>   -   하지만 일반 코드에서는 해당 래퍼는 무시되며, **Library Evolution Mode**에서만 동작한다. (그게 뭔지 모르겠다...)
>
>   *p.s. 내부적으로 `@frozen`으로 만들 것인지에 대한 리스트가 [여기](https://github.com/apple/swift-evolution/blob/master/proposals/0192-non-exhaustive-enums.md#effect-on-the-standard-library-and-overlays) 있으니 참고하면 좋을 듯 하다.*

대강 라이브러리 만들 때 사용하면 된다는 것 같아서, 대강의 의미만 이해하고 넘어갔었다.

그런데 이번에 Xcode 13으로 업데이트 후, 열거형에 대해 `switch - case` 구문을 자동으로 작성해주는 기능을 사용했더니 갑자기 `@unknown default` 라는 것이 생성되었다.

보통 열거형에 대해 `switch - case` 문을 작성할 때는 `default` 는 따로 안들어갔던 거 같은데... 싶어서 삭제하고 수동으로 다시 적어봤다.

그랬더니, 노란 줄이 뜨더니 내용은 이랬다.

>   ##### Warning
>
>   Switch covers known cases, but 'CLAuthorizationStatus' may have additional unknown values, possibly added in future versions.

즉, `CLAuthorizationStatus`가 미래 버전에서 추가적인 값을 가질 수도 있으니, 그에 대비하라는 것이다.

<br/>



## `default` 와 `@unknown default` 의 차이점

그럼 `default`와 `@unknown default`의 차이는 무엇일까?

예시 코드를 하나 보자.

```swift
let manager: CLLocationManager = .init()

switch manager.authorizationStatus {
    case .notDetermined:
    	print("not determined")
    case .denied, case .restricted:
    	fallthrough
    default:
    	print("default")
}

switch manager.authorizationStatus { // Warning: Switch must be exhaustive
    case .notDetermined:
    	print("not determined")
    case .denied, case .restricted:
    	fallthrough
    @unknonwn default:
    	print("default")
}
```

두 개의 동일한 스위치 구문이 있는데, 아래에만 경고가 뜬다.

우선 `authorizationStatus`가 가지고 있는 유형은 다음과 같다.

-   `denied`
-   `notDetermined`
-   `restricted`
-   `authorizedAlways`
-   `authorizedWhenInUse`

그럼 첫 번째 스위치 구문에서는 경고가 뜨지 않는 것이 맞다. 정의해주지 않은 `authorizedAlways`와 `authorizedWhenInUse`에 대한 처리는 이미 `default`가 해주고 있기 때문이다.

하지만 두 번째 스위치 구문에서는 `authorizedAlways`와 `authorizedWhenInUse`에 대해서 처리해주고 있지 않다.

`@unknown default`는 나머지 유형에 대한 처리가 아닌 미래에 추가될 값에 대한 처리이기 때문이다.

`@unknown default`로 처리한 값은 미래에 값이 추가되면 Xcode가 경고를 띄워줄 것이다.

<br/>



>   ## Reference
>
>   -   https://www.avanderlee.com/swift/unknown-default-enums-in-swift/
>   -   https://docs.swift.org/swift-book/ReferenceManual/Attributes.html

