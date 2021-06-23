# Collection Types

>   Reference
>
>   1.  [Swift.org - Collection Types](https://docs.swift.org/swift-book/LanguageGuide/CollectionTypes.html)

Swift는 값 컬렉션을 저장하기 위해 배열, 집합 및 딕셔너리으로 알려진 세 가지 기본 컬렉션 타입을 제공한다. 배열은 정렬 된 값 모음입니다. 집합은 순서가 지정되지 않은 고유한 값의 모음입니다. 딕셔너리는 순서가 지정되지 않은 키-값의 모음이다.

![../_images/CollectionTypes_intro_2x.png](https://docs.swift.org/swift-book/_images/CollectionTypes_intro_2x.png)

Swift에서 배열, 집합 그리고 딕셔너리는 저장할 수 있는 키와 값의 타입에 대해 항상 명확하다. 즉, 실수로 잘못된 타입의 값을 컬렉션에 삽입할 수 없음을 뜻한다. 또 컬렉션을 검색할 때, 값의 타입이 어떤 타입인지 정확하게 알 수 있다.

>   #### Note
>
>   Swift의 배열, 집합 및 딕셔너리 타입은 제네릭 컬렉션으로 구현되어 있다.



## Mutability of Collections

배열, 집합 또는 딕셔너리를 만들고 변수에 할당하면 변경이 가능하다. 즉, 컬렉션을 만든 후에 항목을 추가 또는 제거할 수 있다는 뜻이다. 컬렉션 타입을 상수에 할당하면 해당 컬렉션의 크기와 내용을 변경할 수 없다.

>   #### Note
>
>   컬렉션 타입의 변경이 필요한 경우를 제외하고는, 상수로 생성하는 것이 좋다. 그러면 해당 컬렉션에 대한 추적이 쉬워지고, 성능을 최적화할 수 있다.



## Arrays

배열은 같은 타입의 값들을 순서대로 저장한다. 값의 중복을 허용합니다.

>   #### Note
>
>   Swift의 배열은 `Foundation`의 `NSArray` 클래스와 브릿징된다.
>
>   `Foundation`과 `Cocoa`에서의 배열 사용에 대해 더 자세한 정보는 [해당 문서](https://developer.apple.com/documentation/swift/array#2846730)를 참고하자.

Swift 배열은 `Array<Element>`로 사용하는데, `Element`는 배열이 저장할 수 있는 값 타입을 나타낸다. `[Element]`처럼 축약형으로도 사용할 수 있다. 두 가지의 표현은 기능적으로 동일하지만 축약형이 선호되고, 해당 문서에서도 축약형으로 나타내도록 하겠다.

생성자 구문을 사용해서 특정 타입의 빈 배열을 만들 수 있다.

```swift
var someInts: [Int] = []
print("someInts is of type [Int] with \(someInts.count) items.")
// Prints "someInts is of type [Int] with 0 items."
```

`someInts` 변수는 생성자의 타입에서 `[Int]`타입으로 추론된다.

컨텍스트가 이미 타입 정보(함수의 인자 또는 타입이 확정된 변수 또는 상수)를 제공하고 있는 경우, 빈 배열을 빈 배열 리터럴인 `[]`을 통해 생성할 수 있다.

```swift
someInts.append(3)
// someInts now contains 1 value of type Int
someInts = []
// someInts is now an empty array, but is still of type [Int]
```

Swift의 배열은 모든 값이 동일한 기본 값으로 설정된 일정 크기의 배열을 생성할 수 있는 생성자를 제공한다. 이 생성자에 반복할 값(`repeating` 인자)과 반복 횟수(`count` 인자)를 전달한다.

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```

`+` 연산자를 통해서 호환 가능한, 기존에 존재하던 배열 두 개를 함께 추가하여 새로운 배열을 생성할 수 있다. 새 배열의 타입은 추가하는 두 배열의 유형으로부터 유추할 수 있다.

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5]

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

또 배열 리터럴을 사용해서 배열을 초기화 할 수 있다. 이는 하나 이상의 값을 배열 컬렉션으로 사용하는 간단한 방법이다. 배열 리터럴은 대괄호 내부에 쉼표로 값을 구분하여 작성된다.

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList has been initialized with two initial items
```

`shoppingList` 변수는 문자열 값의 배열로 선언되고 `[String]` 타입으로 작성된다. 왜냐하면 해당 배열은 문자열 타입으로 지정했기 때문이고, 그렇기에 문자열 값만 저장할 수 있다. `shoppingList` 배열은 리터럴 내에 사용된 두 개의 문자열 값인 `Eggs` 와 `Milk` 로 초기화 된다.

>   #### Note
>
>   위에서도 변경이 일어나는 경우를 제외하고는 상수로 선언하는 것이 좋다고 언급했는데, `shoppingList`를 변수로 선언한 이유는, 아래에서 더 많은 항목이 추가되기 때문이다.

이 경우 배열 리터럴에는 두 개의 문자열 값만 포함되며 다른 요소는 없다. 이것은 `shoppingList` 변수의 선언 타입과 일치하기 때문에 배열 리터럴의 할당으로 배열을 초기화하는 것이 허용된다.

Swift의 타입 추론의 덕분에 같은 타입의 값을 포함하는 배열 리터럴로 초기화하는 경우, 배열 타입을 따로 선언할 필요가 없다. `shoppingList`의 초기화는 대신 더 짧은 형식으로 작성될 수 있다.

```swift
var shoppingList = ["Eggs", "Milk"]
```

모든 값이 동일한 유형이기 때문에 Swift는  `shoppingList` 변수에 사용할 수 있는 타입으로 `[String]`이 올바르다고 추론한다.

메서드와 프로퍼티 또는 서브스크립트 문법을 통해 배열에 접근하고 수정할 수 있다.

배열의 항목 수를 확인하려면 읽기 전용 프로퍼티인 `count`를 사용할 수 있다.

```swift
print("The shopping list contains \(shoppingList.count) items.")
// Prints "The shopping list contains 2 items."
```

부울 타입인 `isEmpty` 프로퍼티를 사용하여 `count` 프로퍼티가 0과 같을 때의 조건을 간단하게 확인할 수 있다.

```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list isn't empty.")
}
// Prints "The shopping list isn't empty."
```

`append(_:)` 메서드를 통 배열 끝에 새로운 항목을 추가할 수 있다.

```swift
shoppingList.append("Flour")
// shoppingList now contains 3 items, and someone is making pancakes
```

또는 `+=` 연산자를 통해 하나 이상의 같은 타입의 항목을 배열에 추가할 수 있다.

```swift
shoppingList += ["Baking Powder"]
// shoppingList now contains 4 items
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items
```

서브스크립트 문법(검색하려는 값의 인덱스를 배열 이름 바로 뒤 대괄호에 전달)을 사용해서 값을 검색할 수 있다.

```swift
var firstItem = shoppingList[0]
// firstItem is equal to "Eggs"
```

>   #### Note
>
>   배열 첫 번째 항목의 인덱스는 1이 아니라 0이다. Swift의 배열은 항상 0부터 시작한다.

서브스크립트 문법을 통해 해당 인덱스에 있는 값에 접근하여 변경할 수 있다.

```swift
shoppingList[0] = "Six eggs"
// the first item in the list is now equal to "Six eggs" rather than "Eggs"
```

서브스크립트 문법을 사용할 때는 검색하려고 하는 인덱스가 유효해야한다. 예를 들어, `shoppingList[shoppingList.count] = "Salt"` 구문을 작성해서 배열 끝에 새 항목을 추가하려고 시도하면 런타임 오류가 발생한다.

서브스크립트 문법을 통해 대체 값의 범위가 기존 배열에서 대체하려는 값의 범위와 달라도 한 번에 변경할 수 있다. 아래는 "초콜릿 스프레드", "치즈", "버터" 총 3개의 항목을 "바나나", "사과" 총 2개의 항목으로 변경하는 예시이다.

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList now contains 6 items (7 items -> 6 items)
```

지정된 인덱스에 항목을 삽입하려면 `insert(_:at:)` 메서드를 호출한다.

```swift
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList now contains 7 items
// "Maple Syrup" is now the first item in the list
```

값의 대체가 아니라 기존의 값을 뒤로 밀어내고, 지정된 인덱스에 해당 값을 삽입하는 메서드이다.

마찬가지로 remove(at:) 메서드를 사용하여 배열에서 항목을 제거할 수 있다. 해당 메서드는 지정된 인덱스에서 항목을 제거하고 제거된 항목을 반환한다. (반환 값이 필요하지 않은 경우에는 무시할 수 있다.)

```swift
let mapleSyrup = shoppingList.remove(at: 0)
// the item that was at index 0 has just been removed
// shoppingList now contains 6 items, and no Maple Syrup
// the mapleSyrup constant is now equal to the removed "Maple Syrup" string
```

>   #### Note
>
>   배열의 기존 범위를 벗어난 인덱스 값에 접근하거나 수정하려고 시도할 때 런타임 오류가 발생한다. 인덱스를 사용하기 전에 배열의 `count` 속성과 비교하여 유효한지 확인할 수 있다. 배열에서 가장 큰 유효한 인덱스는 `count - 1` 이다. 배열은 0부터 인덱싱되기 때문이다. 그러나 `count`가 0이면 (배열이 비어 있음을 의미) 유효한 인덱스가 없다는 의미가 된다.

배열의 모든 간격은 항목이 제거될 때 닫히므로, 값은 다시 `Six eggs`가 된다.

```swift
firstItem = shoppingList[0]
// firstItem is now equal to "Six eggs"
```

배열에서 최종 항목을 제거하려면 `remove(at:)` 보다 `removeLast()` 메서드를 사용하는 것이 더 효율적이다.(`count` 속성을 쿼리하지 않아도 된다) `removeLast()` 메서드도 `remove(at:)`과 동일하게 제거된 항목을 반환한다.

```swift
let apples = shoppingList.removeLast()
// the last item in the array has just been removed
// shoppingList now contains 5 items, and no apples
// the apples constant is now equal to the removed "Apples" string
```

배열의 전체 값을 for-in 구문을 통해 반복할 수 있다.

```swift
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

각 항목의 정수 인덱스와 값이 필요한 경우 `enumerated()` 메서드를 사용하여 반복할 수 있다. 배열의 각 항목에 대해 `enumerated()` 메서드는 정수 인덱스와 매치된 항목으로 구성된 튜플을 반환한다. 정수는 0에서 시작하여 각 항목에 대해 1씩 증가한다. 전체 배열을 열거하는 경우, 정수 값은 인덱스 값과 일치한다. 반복의 일부로 튜플을 임시 상수 또는 변수로 분해할 수 있다.

```swift
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```



<kbd>진행 중</kbd>
