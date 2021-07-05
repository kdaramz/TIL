# `contentMode`란?

>   ## Reference
>
>   -   [Apple Developer Documentation - contentMode](https://developer.apple.com/documentation/uikit/uiview/1622619-contentmode/)
>   -   [Rhyno's DevLife Log - View의 ContentMode](https://jcsoohwancho.github.io/2019-10-05-View의-ContentMode/)

애플에서 `contentMode`는 어떻게 정의되어 있을까?

>   뷰 경계가 변경될 때, 뷰가 컨텐츠를 레이아웃하는 방법을 결정하는데 사용되는 플래그이다.

**뷰의 경계가 변경된다** 라는 것은 말 그대로 뷰의 크기가 변경된다는 뜻으로 받아들일 수 있다. 그래서 보통 사이즈 조절이 가능한 컨트롤(`UIImageView`, `UIVisualEffectView` 등)을 구현할 때 자주 사용한다고 한다. (`UIView`의 프로퍼티이기 때문에 아마 `UIView`를 상속받고 있는 컨트롤들은 모두 사용 가능하다.)

뷰는 내부적으로 자신이 나타낼 컨텐츠의 비트맵 데이터를 캐싱하고 있는데, 뷰의 `bounds`가 변했을 때(최초 컨텐츠를 그릴 때도 영향을 미침), 매번 새로 그리는 작업은 많은 비용을 요구하므로 캐싱하고 있던 비트맵 데이터를 활용해서 빠르게 반응하게 된다고 한다. 이 때, 뷰의 사이즈, 배치 등의 정책을 결정하는 것이 `contentMode`이다.

## 종류

해당 프로퍼티는 **컨텐츠 크기를 조정**하거나 **뷰의 특정 지점에 컨텐츠를 고정**할 수 있다. `contentMode` 에 할당할 수 있는 값으로는 열거형인 `UIView.ContentMode` 이 있는데, 이 열거형에 대해서 두 개의 부류로 나누어 알아보자.

### 컨텐츠 크기를 조정

```swift
case scaleToFill
case scaleAspectFit
case scaleAspectFill
case redraw
```

1.  `scaleToFill` : 컨텐츠 가로 및 세로 비율을 조정해서 정해진 뷰의 크기와 일치하도록 조정한다.
2.  `scaleAspectFit` : 컨텐츠의 가로 및 세로 비율을 유지하면서 뷰의 크기에 맞도록 조정한다. 뷰 경계의 나머지 부분은 투명하다.
3.  `scaleAspectFill` : 뷰 크기를 채우기 위해 비율을 유지한 채로 컨텐츠 크기를 조정한다. 뷰의 경계를 채우기 위해 컨텐츠 일부가 잘릴 수 있다.
4.  `redraw` : 해당 항목의 경우는 좀 특별한 케이스이다. 결과만 놓고 보자면, `scaleToFill`과 동일한 결과를 보여준다. 하지만 단어 뜻에서 알 수 있듯이, 비트맵 데이터를 활용하지 않고 **다시 그린다**. 그리고 매번 `setNeedsDisplay()`가 호출된다.

### 뷰의 특정 지점에 컨텐츠를 고정

```swift
case center
case top
case bottom
case left
case right
case topLeft
case topRight
case bottomLeft
case bottomRight
```

해당 속성들은 직관적인 위치를 나타내므로 자세한 설명은 생략한다.

