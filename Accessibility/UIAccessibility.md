# UIAccessibility

- 사용자 인터페이스 요소에 접근성 정보를 제공하는 비공식 프로토콜이다.
- `UIAccessibilityElement` 클래스는 비공식 프로토콜도 구현한다.
- 커스텀 뷰를 만드는 경우는 `UIAccessibilityElement`  인스턴스를 만들어야할 수도 있는데, `UIAccessibility` 속성을 지원하여 접근성 속성을 설정하고 반환한다.

> **`Formal Protocol` vs `Informal Protocol`**
>
> * **`Informal Protocol`**
>   * `Category`
>   * 선택적으로 구현
>   * 암시적으로 거의 모든 객체가 해당 프로토콜을 따르도록 되어있음
> * **`Formal Protocol`**
>   * `Extension`
>   * 필수적으로 구현하나 선택적으로도 구현 가능
>   * 프로토콜은 상속됨
>   * 프로토콜이 다른 프로토콜을 채택할 수 있음

