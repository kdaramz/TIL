# CGRect

>   ## Reference
>
>   -   [Documentation Archive - Coordinate system](https://developer.apple.com/library/archive/documentation/General/Devpedia-CocoaApp-MOSX/CoordinateSystem.html)
>
>   -   [Apple Development Documentation - CGRect](https://developer.apple.com/documentation/coregraphics/cgrect/)
>   -   [Apple Development Documentation - CGPoint](https://developer.apple.com/documentation/coregraphics/cgpoint/)
>   -   [Apple Development Documentation - CGSize](https://developer.apple.com/documentation/coregraphics/cgsize/)



## 정의

공식 문서에는 다음과 같이 표현되어있다.

>   A structure that contains the location and dimensions of a rectangle.
>
>   " 직사각형의 **위치**와 **치수**를 포함하는 구조체 "



## 특징

Core Graphics의 좌표 시스템은 UIKit의 좌표 시스템과 조금 다르다. UIKit의 좌표 시스템은 좌상단 모서리에서 (0, 0)으로 시작하여 우측으로 갈수록 x 값이 증가하고, 하단으로 내려갈수록 y값이 증가하는 구조이다. 반면 Core Graphics의 좌표 시스템은 우하단 모서리에서 (0, 0)으로 시작하여 우측으로 갈수록 x 값이 증가하고, 상단으로 올라갈수록 y값이 증가하는 구조이다. (AppKit도 동일)

<img width="600" alt="스크린샷 2021-08-10 03 48 27" src="https://user-images.githubusercontent.com/73573732/128758154-32c85896-50aa-4e4b-bfce-9b88ed83cc7c.png">

해당 이미지를 참고하면 특정한 점(원점)에서 좌표를 따라 확장되는 그림을 볼 수 있다. 즉, `CGRect`를 이용하여 직사각형을 그리는 방식도 **특정한 점(위치)**을 기준으로 **특정 크기(치수)**만큼 확장하는 방식이다.

`CGRect`의 이니셜라이저를 보면 다음과 같다.

```swift
init(origin: CGPoint, size: CGSize)
init(x: Double, y: Double, width: Double, height: Double)
init(x: Int, y: Int, width: Int, height: Int)
init(x: CGFloat, y: CGFloat, width: CGFloat, height: CGFloat)
```

뭔가 `origin`은 `x`와 `y`, `size`는 `width`와 `height` 값이 연관될 것 같고, 각각 위치와 치수에 관한 것이라 추측된다. 그럼 `CGPoint`와 `CGSize`에 대해 알아보자.



### CGPoint

문서에서는 `CGPoint`를 다음과 같이 정의하고있다.

>   A structure that contains a point in a two-dimensional coordinate system.
>
>   " 2차원 자표계의 한 점을 표함하는 구조체 "

즉, 하나의 점(Point)를 나타내는 구조체이다. 해당 구조체의 이니셜라이저를 보면 다음과 같다.

```swift
init(x: Double, y: Double)
init(x: Int, y: Int)
init(x: CGFloat, y: CGFloat)
```



### CGSize

문서에서는 `CGSize`를 다음과 같이 정의하고있다.

>   A structure that contains width and height values.
>
>   " 너비 및 높이 값을 포함하는 구조체 "

이니셜라이저는 다음과 같다.

```swift
init(width: Double, height: Double)
init(width: CGFloat, height: CGFloat)
init(width: Int, height: Int)
```



위에서 했던 추측이 검증되었다.  `CGRect`는 `CGPoint`와 `CGSize` 값을 통해 사각형을 그린다 라고 할 수 있겠다.