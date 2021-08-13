# Relation between view and layer

>   ## Reference
>
>   1.   Raywenderlich - [CALayer Tutorial for iOS : Getting Started](https://www.raywenderlich.com/10317653-calayer-tutorial-for-ios-getting-started)
>   2.   Development Documentation - [UIView](https://developer.apple.com/documentation/uikit/uiview)



## `UIView`는 `CALayer`의 wrapper 역할을 한다.

`UIView`는 터치 및 레이아웃 관리 등 많은 일을 처리하지만, 드로잉 및 애니메이션을 직접 제어하지는 않는다. 해당 작업은 `CALayer`를 사용할 수 있는 Core Animation으로 위임한다. (`UIView`가 `CALayerDelegate`를 준수하고 있기 때문)

`UIView` 인스턴스는 여러 개의 서브 레이어를 포함할 수 있는 루트 레이어가 있다. `UIView` 하나에 루트 레이어는 한 개만 존재할 수 있고, 서브 레이어는 여러 개 존재할 수 있다.

<img width="300" alt="스크린샷 2021-08-13 13 49 17" src="https://user-images.githubusercontent.com/73573732/129306264-15d99583-5e72-4fc3-903b-5db5a18473a0.png">

예를 들어 다음 코드에서 2번 라인과 3번 라인의 코드는 같다. 

```swift
let redView: UIView = .init(frame: CGRect(origin: origin, size: size))
redView.backgroundColor = UIColor.systemRed
redView.layer.backgroundColor = UIColor.systemRed.cgColor
```

그 이유는 `UIView`는 `CALayer`의 래퍼 역할을 하기 때문이다.

하지만 `UIView`가 `CALayer`의 래퍼 역할을 한다고 하지만, 레이어와 동일한 것은 아니다. 뷰는 드로잉 작업 뿐만 아니라, 터치 이벤트 처리, 하위 뷰에 대한 레이아웃 관리 등을 포함하고 있는데, 레이어는 드로잉 및 애니메이션과 연관이 있기 때문에 터치 이벤트를 수행하지는 않는다.
