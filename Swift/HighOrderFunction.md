# 고차 함수 (Higher-order function)란?

 다른 함수를 전달인자(argument)로 받거나 함수 실행의 결과를 함수로 반환하는 함수를 뜻한다. 스위프트는 함수를 일급 객체로 취급하기에, 함수를 다른 함수의 전달인자로 사용할 수 있다. 대표적으로 `map`, `filter`, `reduce` 가 있고, 각각 축약형을 사용하여 사용할 수 있다. 고차 함수를 사용함으로써 얻을 수 있는 장점은 다음과 같다. 

- 코드가 간결해진다.
- 멀티 스레드 환경에서 생길 수 있는 부작용을 방지할 수 있다.
- 새로운 값을 받을 때 변수가 아닌 상수로 받을 수 있다.

## 1. Map

### 1-1. 특징

- 자신을 호출할 때, 매개변수로 전달된 함수를 실행하여 그 결과를 다시 반환해주는 함수다.
- `Sequence` 와 `Collection` 프로토콜을 준수하는 타입과 옵셔널은 맵을 사용할 수 있다.
- 기존의 값을 변경하는 것이 아니라 새로운 값을 반환한다.
- 기존 데이터의 변형이 필요할 때 사용한다.
- 원소의 갯수에 변화가 없다.

### 1-2. 예시

```swift
// 소문자가 들어있는 알파벳 모두를 대문자로 변경
let lowercaseAlphabets = ["a", "b", "c", "d", "e"]
let uppercaseAlphabets = lowercaseAlphabets.map({ (elem: String) -> String in
    return elem.uppercased()
})
```

```swift
let lowercaseAlphabets = ["a", "b", "c", "d", "e"]
let uppercaseAlphabets = lowercaseAlphabets.map { $0.uppercased() }
```

## 2. Filter

### 2-1. 특징

- 기존 컨테이너 내부의 값을 특정 조건으로 걸러서 추출하는 역할을 하는 함수다.
- 함수의 반환 타입은 Bool 타입이다.
- 기존의 값을 변경하는 것이 아니라 새로운 값을 반환한다.
- 원소의 갯수의 변화가 있을 수 있다.

### 2-2. 예시

```swift
// "X" 문자열이 포함된 원소 필터링
let products = ["iPhone 12 Pro", "iPhone 12 mini", "iPhone XS", "iPhone X", "iPhone SE"]
let productsContainX = products.filter({ (elem: String) -> String in
    return elem.contain("X")
})
```

```swift
let products = ["iPhone 12 Pro", "iPhone 12 mini", "iPhone XS", "iPhone X", "iPhone SE"]
let productsContainX = products.filter { $0.contains("X") }
```

## 3. Reduce

### 3-1. 특징

- 컨테이너 내부의 콘텐츠를 하나로 합해주는 역할을 하는  함수이다.
- `map` 과 `filter` 와 다르게 두 가지 방법이 있는데, 각 요소를 전달받은 후 연산한 값을 반환하는 방법과 연산한 값을 반환하지 않는 방법이 있다.
- 초기 값 개념이 존재한다.
- 모든 원소에 대해서 순회하며 연산을 하기 때문에 원소의 갯수가 하나로 줄어든다.

### 3-2. 예시

```swift
// 각 숫자들의 합
let numbers = [11, 22, 33, 44, 55]
let numberSum = numbers.reduce(0, { (lhs: Int, rhs: Int) -> Int in
    return lhs + rhs
})
```

```swift
let numbers = [11, 22, 33, 44, 55]
let numberSum = numbers.reduce(0) { $0 + $1 }
```

## 4. 고민한 점 / 해결 방법

- 멀티 스레드 환경에서 어떤 문제점이 있을까?

    → 공유 메모리 자원의 사용에 문제가 발생할 수 있다고 한다. 예를 들어 1번 스레드에서 a + b에 대한 연산을 수행해야 하는데, 2번 스레드에서 a의 값을 변경시킬 수 있는 연산을 수행한다면, 1번 스레드에서 기대한 값과 다른 결과를 초래할 수 있다. 이렇게 공유 자원(메모리 지역의 전역변수)을 보호하기가 어렵다. 그런데 고차 함수를 사용하면 해당 메모리에 접근하는 것이 아니라, 사용하려는 변수를 복사해와서 복사된 메모리에 접근하기 때문에 해당 문제를 방지할 수 있다고 한다.

# 💁 참고

- 야곰의 스위프트5 프로그래밍