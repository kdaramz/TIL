# 알쓸신잡

> 본격 정리하기에는 방대하고, 지나치기엔 중요한, 알아두면 정말 좋은 잡다한(?) 지식들을 정리했습니다.
>
> 새로운 공부 주제가 될 수도 있겠네요! 😁



## 프로그래밍

* 콜백 함수: 다른 코드의 인수로서 넘겨주는 실행 가능한 코드를 말한다. 처음에는 Swift에서 제공하는 클로저 문법과 콜백이 비슷해서 무슨 차이일까 생각을 했었는데, Swift의 클로저에는 많은 의의가 있는데, 많은 용도 중에서 클로저를 콜백으로 사용하기도 한다라고 들었다. 즉, 프레임워크에서 지원해주는 여러가지 메서드 중, completionHandler 라는 인자 레이블을 볼 수 있는데, 이 부분은 콜백으로 사용하는 부분이라고 생각하면 된다.
* Thread Safe: 여러 스레드에서 하나의 값을 접근 또는 변경해도 값이 보장된다는 것을 의미하며, 프로그램 실행의 문제가 없음을 뜻한다.
* i18n: internationalization의 첫과 끝을 나타내고 사이에 i와 n을 제외한 글자 수를 표기해 나타낸 것이다. 즉, 국제화를 나타내는 단어이다.
* l10n: localization을 뜻하며, 현지화를 나타내는 단어이다. 국제화가 단순히 문자열 또는 문자를 치환해서 다른 언어권에서 읽을 수 있도록 하는 조치라면, 현지화는 그 나라 언어의 특성, 숫자, 날짜 및 시간 형식 등 문화가 조금 더 접목된 개념이라고 볼 수 있다.



## iOS

* `UILabel` 폰트 설정하는 방법 : `UILabel` 의 `font` 속성은 `UIFont` 타입이다. 그 것에 유의해서 다음을 적용시킬 수 있다.

  ```swift
  // System Font의 Size만 변경하고자 할 때
  label.font = UIFont.systemFont(ofSize: 10)
  // Font 까지 변경하고자 할 때
  label.font = UIFont(name: "폰트 이름", size: 10)
  ```

* UIColor 를 Hex Value로 사용하기

  ```swift
  // How to set
  extension UIColor {
      convenience init(hex: Int, alpha: CGFloat = 1.0) {
          self.init(
          	red: CGFloat((hex & 0xFF0000) >> 16) / 255.0,
              green: CGFloat((hex & 0x00FF00) >> 8) / 255.0,
              blue: CGFloat((hex & 0x0000FF) > 0) / 255.0,
              alpha: alpha
          )
      }
  }
  
  // How to use
  let green: UIColor = UIColor(hex: 0x1faf46)
  let red: UIColor = UIColor(hex: 0xfe5960)
  let blue: UIColor = UIColor(hex: 0x0079d5)
  ```

* `print` 자체의 로직으로 인해 앱 배포 시 포함되어 있다면 성능에 좋지 않은 영향을 준다. print를 지워주거나 `#if` 와 `#endif` preprocessor를 사용하는 것이 좋다고 한다.

  ```swift
  func aFunction() {
      // Code...
      #if DEBUG // 디버그 시에만 사용된다.
      print(message)
      #endif
  }
  ```

* iOS에는 Project Deployment Target과 Target Deployment Target이 있는데, 보통 Project Deployment Target의 설정으로 끝낼 수 있지만, Target Deployment Target을 설정했을 경우, Project Deployment Target 설정을 오버라이딩한다. 즉, Target Deployment Target이 최소 지원 버전이 된다.

* `isEmpty` 가 `count == 0` 보다 퍼포먼스 면에서 뛰어나다. `isEmpty` 는 O(1)이고, `count == 0` 은 O(N) 이다.

* `@IBDesignable` 키워드를 선언하여 앱을 실행시키지 않고도 Storyboard에서 변경 및 적용 사항을 확인할 수 있다.



## Xcode

- Console에 시뮬레이터 디버깅 로그가 뜰 때

  - Xcode 8 부터 `OS_ACTIVITY_MODE` 가 추가되었는데, 시뮬레이터에서 출력하는 로그들이라고 한다. 로그들 때문에 제대로 된 로깅을 확인할 수 없을 때는 다음처럼 해결할 수 있다.

    `Edit Scheme` > `Run` > `Environment Variables` > `OS_ACTIVITY_MODE` 키 추가 > `disable` 설정

     <img width="1904" alt="스크린샷 2021-05-22 오후 7 37 56" src="https://user-images.githubusercontent.com/73573732/119223605-580f7180-bb35-11eb-8ed3-b6fabd805f43.png">

-   커스텀 TableViewCell을 Xib파일로 만들 때 주의해야할 점
    -   TableViewCell에는 contentView가 존재한다. TableViewCell에 커스텀 클래스명을 넣어야하는데 실수로 contentView에 설정하면 TableViewCell이 깨져보인다.
