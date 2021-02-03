# Generics

> 강력한 기능! 제네릭

* 타입 매개변수(T) 는 사용 시, 실제 타입으로 변환되는 placeholder 라고 볼 수 있다. 타입 매개변수는 꺽쇠 안에서 선언하며 하나 이상이 올 수 있고, 그 것들은 콤마로 구분한다. 그리고 대문자로 써주는 것이 원칙이다! (하지만 꼭 T가 아니어도 됨.)

```swift
func swapValue<T>(lhs: inout T, rhs: inout T) {
    let temp = lhs
    lhs = rhs
    rhs = temp
}
```

* T 에 올 수 있는 타입에는 제약이 없기 때문에 비교 연산자의 사용에 제한이 있다. 즉 비교연산이 구현되지 않은 타입이 올 수도 있으므로 코드에서 에러를 띄워준다.

```swift
func swapValue<T>(lhs: inout T, rhs: inout T) {
    if lhs == rhs { return } // Error: == 연산자에 대한 비교 기능이 구현되지 않은 타입이 올 수 있음
    let temp = lhs
    lhs = rhs
    rhs = temp
}
```

* 그렇기 때문에 비교연산을 사용하려면 직접 구현하거나, 프로토콜을 채택함으로써 해결할 수 있다. 이를 Type Constraints(타입 제약) 라고 한다.

```swift
func swapValue<T: Equatable>(lhs: inout T, rhs: inout T) {
    if lhs == rhs { return } // Equtable 프로토콜을 준수하는 타입만 올 수 있으므로 에러가 나지 않음
    let temp = lhs
    lhs = rhs
    rhs = temp
}
// Equtable 은 == 또는 != 를 통해 비교할 수 있는 속성을 나타내는 프로토콜이다.
```

* Associate Type(연관 타입)은 타입에 이름을 부여한다. `associatedtype` 키워드를 사용하며, 프로토콜 내에서 타입 프로퍼티와 같은 역할을 한다.

```swift
protocol Container<T> { // Error: Protocols do not allow generic parameters
    ///
}

protocol Container {
    associatedtype T
    mutating func append(_ item: T)
    var count: Int { get }
    subscript(i: Int) -> T { get }
}
```

* `where` 절을 통해 연관 타입에 제약을 걸 수 있다.

```swift
protocol SuffixableContainer {
    // 연관 타입 T는 SuffixableContainer를 준수하고, Container의 T 와 같은 타입이어야 한다는 제약이다.
    associatedtype T: SuffixableContainer where Suffix.T == T
    func suffix(_ size: Int) -> Suffix
 }
```

* `extension` 을 통해 특정 함수가 포함된 Stack은 반드시 Equatable 프로토콜을 준수해야하는 제약을 추가한 예시이다.

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```