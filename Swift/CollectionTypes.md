# Collection Types

>   Reference
>
>   1.  [Swift.org - Collection Types](https://docs.swift.org/swift-book/LanguageGuide/CollectionTypes.html)

Swift는 값 컬렉션을 저장하기 위해 배열, 집합 및 딕셔너리으로 알려진 세 가지 기본 컬렉션 타입을 제공한다. 배열은 정렬 된 값 모음입니다. 집합은 순서가 지정되지 않은 고유한 값의 모음입니다. 딕셔너리는 순서가 지정되지 않은 키-값의 모음이다.

<img src="https://docs.swift.org/swift-book/_images/CollectionTypes_intro_2x.png" alt="../_images/CollectionTypes_intro_2x.png" style="zoom:50%;"/>

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

`for-in` 에 대한 자세한 정보는 [For-In Loops](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html#ID121) 문서를 참고하라.



## Sets

집합은 같은 타입의 고유한 값을 순서없이 저장한다. 그렇기에 항목 순서가 중요하지 않거나, 항목의 중복이 필요한 경우 배열 대신 집합을 사용할 수 있다.

>   #### Note
>
>   Swift의 집합은 `Foundation` 의 `NSSet` 클래스와 브릿징된다.
>
>   `Foundation`과 `Cocoa`에서의 집합 사용에 대해 더 자세한 정보는 [해당 문서](https://developer.apple.com/documentation/swift/set#2845530)를 참고하자.

집합에 저장되기 위해서는 `hashalbe` 해야 하는데, 즉, 타입 자체가 해시 값을 계산하는 방법을 제공해야만 한다는 뜻이다. 해시 값은 타입과 값이 같다고 비교되는 모든 객체에 대해 동일한 `Int` 값을 가진다. 따라서 `a == b` 인 경우 `a`의 해시 값은 `b`의 해시 값과 같다.

Swift의 모든 기본 유형(`String`, `Int`, `Double`, `Bool` 등)은 기본적으로 `Hashable` 하며, 집합의 값 또는 딕셔너리의 키로 사용할 수 있다. 열거형의 연관 값을 제외한 항목 값([Enumerations](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)을 참고)도 기본적으로 `Hashable`하다.

>   #### Note
>
>   커스텀 타입이 `Swift Standard Library`의 `Hashable` 프로토콜을 준수하도록 설정하면, 해당 타입을 집합의 값과 딕셔너리의 키로 사용할 수 있다. `hash(into:)` 필수 메서드 구현에 대한 정보는 [Hashable](https://developer.apple.com/documentation/swift/hashable) 문서를 참고하라. 프로토콜 준수에 관한 자세한 내용은 [Protocols](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html) 문서를 참고하라.

집합은 `Set<Element>`로 사용하며, `Element`는 저장을 허용할 타입이다. 배열과는 다르게, 단축 문법이 없다.

이니셜라이저를 이용하여 특정 타입의 빈 집합을 만들 수 있다.

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// Prints "letters is of type Set<Character> with 0 items."
```

>   #### Note
>
>   이니셜라이저 문법에 의해서 변수 `letter`는 집합으로 추론된다.

또는 함수 인자나 변수 타입이 선언된 변수 또는 상수와 같이, 이미 문맥에서 타입에 대한 정보를 제공하는 경우에는 빈 배열 리터럴을 사용해서 빈 집합을 생성할 수 있다.

```swift
letters.insert("a")
// letters now contains 1 value of type Character
letters = []
// letters is now an empty set, but is still of type Set<Character>
```

하나 이상의 값을 집합으로 사용하는 간단한 방법은, 배열 리터럴을 사용하여 집합을  초기화하는 것이다.

아래는 문자열 값을 저장하기 위해 `favoriteGenres` 라는 집합을 만든 예제이다.

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres has been initialized with three initial items
```

`favoriteGenres` 변수는 문자열 타입의 값을 저장하는 `Set<String>` 으로 사용된다. 저장 타입을 문자열 타입으로 지정했기 때문에 문자열 값만 저장할 수 있다. 여기서 `favoriteGenres` 집합은 배열 리터럴 내에 사용된 3개의 문자열 값(`"Rock", "Classical", "Hip hop"`)으로 초기화된다.

>   #### Note
>
>   `favoriteGenres` 집합은 아래 예제에서 항목이 추가 및 제거되기 때문에 상수 대신 변수로 선언했다.

집합은 배열 리터럴만 가지고는 타입 추론이 불가능하므로 `Set` 키워드를 통해 명시적으로 선언해야 한다. 그러나 Swift의 타입 추론으로 인해 특정 타입의 값만 포함하는 배열 리터럴로 초기화하는 경우, 제네릭 문법을 사용하여 선언할 필요가 없다.

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

배열 리터럴의 모든 값이 동일한 타입이기 때문에 Swift는 `favoriteGenres`의 타입이  `Set<String>` 이라고 추론할 수 있다.

메서드와 프로퍼티를 통해 집합에 엑세스하고 수정할 수 있다.

읽지 전용 프로퍼티인 `count`를 사용해서 집합에 포함된 항목 수를 확인할 수 있다.

```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// Prints "I have 3 favorite music genres."
```

`isEmpty` 프로퍼티를 통해 `count` 프로퍼티가 0과 같은지 확인하는 작업을 간단하게 할 수 있다.

```swift
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// Prints "I have particular music preferences."
```

`insert(_:)` 메서드를 호출해서 집합에 새 항목을 추가할 수 있다.

```swift
favoriteGenres.insert("Jazz")
// favoriteGenres now contains 4 items
```

`remove(_:)` 메서드를 호출하여 집합에서 항목을 제거할 수 있다. 제거 항목이 집합에 포함되어 있는 경우 제거된 값을 반환하고 , 집합에 없는 항목인 경우는 `nil` 을 반환한다. 또, 집합의 모든 항목을 `removeAll()` 메서드를 호출해 제거할 수 있다.

```swift
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// Prints "Rock? I'm over it."
```

집합에 특정 항목이 포함됐는지 확인하려면 `contains(_:)` 메서드를 사용하라.

```swift
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// Prints "It's too funky in here."
```

`for-in` 구문을 통해 집합의 값을 반복할 수 있다.

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
// Classical
// Jazz
// Hip hop
```

`for-in` 에 대한 자세한 정보는 [For-In Loops](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html#ID121) 문서를 참고하라.

Swift의 집합은 순서가 없다. 집합의 값을 특정 순서로 반복하려면 `<`를 사용하여 정렬시킨(오름차순)  `sorted()` 메서드를 호출하라.

```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz
```



## Performing Set Operations

기본적인 집합의 작업을 효율적으로 수행할 수 있다.

아래 그림은 a와 b, 두 집합의 연산 결과를 나타낸다.

<img src="https://docs.swift.org/swift-book/_images/setVennDiagram_2x.png" alt="../_images/setVennDiagram_2x.png" style="zoom:50%;"/>



-   두 집합에 공통된 값만 존재하는 새로운 집합을 만들려면 `intersection(_:)` 메서드를 호출하라. (교집합)
-   공통적인 값이 아닌, 각각의 집합에만 있는 값들만 존재하는 새로운 집합을 만들려면 `symmetricDifference(_:)` 메서드를 호출하라. (대칭차집합)
-   두 집합의 모든 값이 존재하는 새로운 집합을 만들려면 `union(_:)` 메서드를 호출하라. (교집합)
-   특정 집합에서 공통 값을 제외한 나머지 값만 가진 새로운 집합을 만들려면 `subtracting(_:)` 메서드를 호출하라. (a에 대한 b의 차집합)

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

아래는 3개의 집합(a, b 및 c)간의 관계를 나타내는 그림이다. 집합 a는 b의 모든 요소를 포함하므로 집합 b의 상위 집합이다. 반대로 b의 모든 요소도 a에 포함되므로 집합 b는 집합 a의 하위 집합이다. 집합 b와 집합 c는 공통 요소를 공유하지 않기 때문에 서로소 집합이라고 한다.

<img src="https://docs.swift.org/swift-book/_images/setEulerDiagram_2x.png" alt="../_images/setEulerDiagram_2x.png" style="zoom:50%;" />

-   `==` 연산자를 사용하여 두 집합에 동일한 값이 모두 포함되어 있는지 확인할 수 있다.
-   `isSubset(of:)` 메서드를 호출하여 인자로 넘겨준 집합이 호출한 집합의 하위 집합인지 확인할 수 있다.
-   `isSuperset(of:)` 메서드를 호출하여 인자로 넘겨준 집합이 호출한 집합의 상위 집합인지 확인할 수 있다.
-   `isStrictSubset(of:)` 또는 `isStrictSuperset(of:)` 메서드를 호출하여 인자로 넘겨준 집합이 호출한 집합과 동일함을 제외한 하위 또는 상위 집합인지 확인할 수 있다.
-   `isDisjoint(with:)` 메서드를 호출하여 두 집합이 서로소 집합인지 확인할 수 있다.

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]

houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```

<kbd>진행 중</kbd>

