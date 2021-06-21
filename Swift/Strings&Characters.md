# Strings and Characters

>   Reference
>
>   1.  [Swift.org - Strings and Characters](https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html)
>   2.  [Bridging](https://soooprmx.com/swift-objctive-c와-swift-타입의-브릿징/)
>   3.  [정보통신기술용어해설](http://www.ktword.co.kr/abbr_view.php?m_temp1=670)



## Before We Go

Swift의 `String` 타입은 `Foundation` 의 `NSString` 클래스와 브릿징된다.

>   여기서 브릿징이란, Objective-C의 `Foundation` 타입에 대응되는 타입들이 있는데, Swift와 Objective-C에서 서로의 API를 아무조건 없이 사용할 수 있다.
>
>   예를들어 다음 코드를 보면
>
>   ```swift
>   let greetingSwift: String = "Hello, Swift"
>   let greetingObjC: NSString = "Hello, Objective-C"
>   
>   let greetingObjCWithExclamationMark: String = greetingObjC.appending("!")
>   ```
>
>   이렇게 `NSString` 타입의 메서드가 매개변수 및 결과 값을 `NSString`이 아닌, `String` 을 사용하는 것이 가능하다. 자세한 것은 [Bridging](https://soooprmx.com/swift-objctive-c와-swift-타입의-브릿징/)을 참고.



## String Literal

아래 코드와 같이 큰 따옴표로 감싸 표현할 수 있으며, 보통 문자열 리터럴은 한 줄로 표현된다.

```swift
let someString: String = "Some string literal value"
```

하지만, 여러 줄의 문자열 리터럴을 사용하고 싶다면 아래 예시처럼 `"""` 를 사용할 수 있다.

```swift
let dummyText = """
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Ut vitae augue venenatis, molestie diam eu, tempus lectus.
"""

// Lorem ipsum dolor sit amet, consectetur adipiscing elit.
// Ut vitae augue venenatis, molestie diam eu, tempus lectus.
```

여러 줄의 문자열 리터럴을 표현할 때, `"""` 안에서는 띄어쓰기 및 줄바꿈까지 반영된다. 만약 결과는 한 줄로 출력하고 싶은데 줄이 너무 길어져서 가독성에 문제가 생기는 것 같으면 아래 코드처럼 `\` 를 사용해서 이를 해결할 수 있다.

```swift
let softWrappedDummyText = """
Lorem ipsum dolor sit amet, \
consectetur adipiscing elit.

Ut vitae augue venenatis, \
molestie diam eu, tempus lectus.
"""

// Lorem ipsum dolor sit amet, consectetur adipiscing elit.
// Ut vitae augue venenatis, molestie diam eu, tempus lectus.
```

여려 줄 문자열 리터럴의 들여쓰기는 다음과 같은 메커니즘으로 동작한다.

![../_images/multilineStringWhitespace_2x.png](https://docs.swift.org/swift-book/_images/multilineStringWhitespace_2x.png)

닫는 `"""` 의 위치가 문자열 들여쓰기의 기준점이 된다고 생각할 수  있다. Swift의 문자열은 유니코드 기반이기 때문에 유니코드 스칼라를 이용하여 이모지를 나타낼 수 있다.

```swift
let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
```

만약 여러 줄의 문자열에서 이스케이프 문자열이 많아 귀찮을 때는 아래와 같이 양쪽 `"""`  이전에  `#`을 추가하여 사용할 수 있다.

```swift
let threeMoreDoubleQuotationMarks = #"""
Here are three more double quotes: """
"""#

// Here are three more double quotes: """
```



## Initializing an Empty String

빈 문자열로 초기화하는 방법은 두 가지가 있으며, 둘 다 동일한 빈 문자열로 초기화한다.

```swift
var ermptyString: String = ""
var anotherEmptyString: String = String()
```



## String Mutability

상수로 선언하느냐, 변수로 선언하느냐에 따라 문자열의 변경 가능 유무가 달라진다. Swift에서 말하는 변경 가능과 Objective-C에서 말하는 변경 가능(`NSString` and `NSMutableString`)은 다른 개념이다.

>   `NSString` 과 `NSMutableString` 은 메서드에서도 그 차이를 볼 수 있는데, 
>
>   ```swift
>   var mutation: NSMutableString = "Hello"
>   var immutation: NSString = "Hello"
>   
>   mutation.append("!") // 기존의 mutation에 !을 추가
>   let newString: String = immutation.appending("!") // 새로운 String 타입의 결과를 반환
>   ```
>
>   `NSString` 은 변경이 불가능하고, `NSMutableString` 은 변경이 가능하다.



## Strings Are Value Type

Swift의 문자열은 값 타입이다. 문자열은 함수 또는 메서드에 전달되거나 변수 또는 상수에 할당될 때 복사가 일어난다. (Copy-On-Write 방식) 원본과 다른 복사본이기 때문에 복사된 문자열은 직접 수정하지 않는 한 수정되지 않는다. 이러한 복사 메커니즘으로 인해 성능이 뛰어나다.



## Working With Characters

`for-in` 을 통해 문자열을 반복해서 개별 문자 값에 접근할 수 있다. 

```swift
for charcter in "DOG" {
    print(character) // String.Element
}

// D
// O
// G
```

반복문을 통해 `character` 에 접근할 때 해당 타입을 확인하면 `String.Element` 라고 되어있는 것을 확인할 수 있다. 해당 타입은 `Character` 의 타입 별명이다.

`Character` 라는 타입을 통해서도 개별 문자를 만들 수 있다.

```swift
let exclamationMark: Character = "!"
```

타입 추론으로는 개별 문자를 만들 수 없기 때문에 꼭 명시해줘야한다. 

`Character` 로 이루어진 배열을 `String` 이니셜라이저의 매개변수로 넘겨서 문자열로 치환할 수 있다.

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString: String = String(catCharacters)
print(catString) // Prints "Cat!🐱"
```



## Concatenating Strings and Characters

```swift
let string1: String = "Hello,"
let string2: String = " there"
var welcome: String = string1 + strigng2 // Hello, there

var instruction: String = "look over"
instruction += String2 // look over there

let exclamationMark: Character = "!"
welcome.append(exclamationMark) // Hello, there!
```

참고로 `Character` 는 `String` 타입에 `+=` 를 통해 바로 이어붙일 수 없다. 다른 타입이기 때문이다. 이와같이 Swift는 타입에 엄격하다.



## String Interpolation

문자열 보간은 리터럴 내에서 상수, 변수 및 표현식을 혼합해서 문자열을 구성할 수 있는 방법이다. `\()` 로 사용하며, 괄호 내부에 원하는 항목을 삽입하면 된다.

```swift
let multiplier: Int = 3
let message: String = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message is "3 times 2.5 is 7.5"
```

실제 문자열을 만들기 위해 문자열 보간 표현식이 평가될 때, 실질적인 `multiplier` 의 값인 3으로 대체된다.



## Unicode

유니코드는 다른 언어 시스템에서 텍스트를 표현 및 처리하기 위한 국제 표준 인코딩이다. 유니코드로 인해 모든 언어의 거의 모든 문자를 표준화된 형식으로 표현할 수 있고, 텍스트 파일 또는 웹 페이지와 같은 외부 소스에서 해당 문자를 읽고 쓸 수 있습니다. Swift의 문자열 및 문자는 유니코드와 완전히 호환된다.

Swift의 기본 문자열은 유니코드 스칼라 값으로 구성되어있다. 유니코드 스칼라 값은 문자 또는 수정자를 위한 고유한 21bit의 숫자이다. 참고로 모든 21비트 유니코드 스칼라 값이 문자에 할당되는 것은 아니다. 일부는 나중에 할당하거나, UTF-16 인코딩에 사용하기위해 예약되어있다.

확장 자소 클러스터란, (결합된 경우) 사람이 읽을 수 있는 단일 문자를 생성하는 하나 이상의 유니코드 스칼라의 나열이다. 예를들어, 문자 é는 단일 유니코드 스칼라(U+00E9)로 나타낼 수 있다. 하지만 동일하게 표준 문자 e(U+0065)와 결합용 악센트 문자 U+0301의 결합으로 표현될 수 있다. 결합용 악센트 문자는 앞에 있는 스칼라 값에 그래픽으로 적용되어 유니코드 인식 텍스트 렌더링 시스템에서 렌더링될 때 e를 é로 변환한다.

```swift
let eAcute: Character = "\u{E9}" // é
let combinedEAcute: Character = "\u{65}\u{301}" // é (e와 `의 결합)
```

한글의 경우도 다음과 같이 적용된다.

```swift
let precomposed: Character = "\u{D55C}" // 한
let decomposed: Character = "\u{u1112}\u{1161}\u{11AB}" // 한 (ㅎ, ㅏ, ㄴ 의 결합)

```



## Counting Characters

`count` 프로퍼티를 통해 문자열의 문자 갯수를 확인할 수 있다.

확장 자소 클러스터를 붙이거나 대체하는 경우에도 문자열 갯수는 영향을 미치지 않는다.

```swift
var word: String = "cafe"
print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in cafe is 4"

word += "\u{301}"    // e가 é로 대체됨 (확장 자소 클러스터 참고)

print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in café is 4"
```

>   확장 자소 클러스터는 여러 유니코드 스칼라로 구성될 수 있다. 같은 문자의 다른 표현은 저장하는데 필요한 메모리의 양이 다를 수 있다. 그래서 Swift의 문자는 문자열 표현 내에서 각각 동일한 양의 메모리를 차지하지 않는다. 즉, 확장 자소 클러스터의 경계를 결정하기 위해 문자열을 반복하지 않고는 문자열의 문자 수를 계산할 수 없다. 특히 긴 문자열 값으로 작업하는 경우 해당 문자열의 문자를 결정하려면 `count` 속성이 전체 문자열의 유니코드 스칼라를 반복해야한다.
>
>   `count` 프로퍼티가 반환하는 문자의 수는 동일한 문자를 포함하는 `NSString` 의 `length` 프로퍼티와 항상 일치하는 것은 아니다. `NSString` 의 길이는 문자열 내의 유니코드 확장 자소 클러스터 수가 아니라 문자열의 UTF-16 표현 내의 16비트 코드 단위 수를 기반으로한다. 아래 코드 예시를 보자.
>
>   ```swift
>class StringLengthOfSameString {
>      let swift: String = "\u{1112}\u{1161}\u{11AB}" // ㅎ, ㅏ ,ㄴ
>      let objc: NSString = "\u{1112}\u{1161}\u{11AB}" // ㅎ, ㅏ, ㄴ
>    }
>    
>   let instance: MemorySizeOfSameString = MemorySizeOfSameString()
>   
>   print(instance.swift.count) // 1
>   print(instance.objc.length) // 3
>   ```
>   
>   `String`은 유니코드 스칼라 방식을 따른다. 그렇기 때문에 확장 자소 클러스터에 의해서 문자열은 `한` 으로 구성되며, 길이는 1이 된다. 하지만 `NSString`은 UTF-16 방식을 따르기 때문에 `ㅎ`, `ㅏ`, `ㄴ`으로 구성되어 길이는 3이 된다.



## Accessing and Modifying a String

메서드와 프로퍼티 또는 `.`을 통해 문자열에 접근할 수 있다.

각 문자열은 각 문자 위치에 해당하는 `String.Index` 가 있다.

>   ### Swift의 문자열에서 정수 인덱스로의 접근을 허용하지 않는 이유
>
>   `String`은 유니코드 스칼라 방식을 따르는데, 확장 자소 클러스터라는 개념이 있다. 그렇기에 한 문자 또는 문자열에 있는 개별 단어가 동일한 메모리 양을 가지는 것이 아니다. 즉, "한국의 국보 1호는 숭례문이다" 라는 문자열에서 단일 스칼라로 표현되는 부분도 있고, 여러 스칼라가 모여 하나의 문자를 표현하는 경우도 있다. 문자를 확인하려면 해당 문자열의 시작 또는 끝에서 문자열을 반복해야하는데, 위의 특성 때문에 정수로는 정확한 위치를 알 수 없다고 한다.

`startIndex`프로퍼티를 사용하여 문자열의 첫 번째 문자에 접근할 수 있다.

`endIndex`를 사용하면 문자열의 마지막 문자 뒤의 위치에 접근하게 된다. 즉, 문자열에 유효한 위치가 아니다.

빈 문자열인 경우, `startIndex`와 `endIndex`가 동일하다.

`index(before:)` 및 `index(after:)` 메서드를 사용하여 주어진 인덱스의 앞뒤 인덱스에 접근할 수 있다.

기준 인덱스에서부터 정해진 거리만큼 떨어진 문자에 접근하려면 `index(_:offsetBy:)`메서드를 사용할 수 있다.

```swift
let greeting: String = "Guten Tag!"
greeting[greeting.startIndex] // G
greeting[greeting.index(before: greeting.endIndex)] // !
greeting[greeting.index(after: greeting.startIndex)] // u
let index: String.Index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index] // a
```

문자열의 범위를 벗어난 인덱스 또는 범위를 벗어난 인덱스에 있는 문자에 접근하려고 하면 런타임 에러가 발생한다.

```swift
greeting[greeting.endIndex] // Error
greeting.index(after: greeting.endIndex) // Error
```

문자열에 포함된 개별 문자의 인덱스 정보를 가지고 있는 `indices` 프로퍼티를 사용하여 개별 문자에 접근할 수 있다.

```swift
for index in greeting.indices {
    print("\(greeting[index]) ", terminator: "")
}
// Prints "G u t e n T a g ! "
```

원하는 위치에 단일 문자를 삽입하려면 `insert(_:at)`을, 문자열을 삽입하려면 `insert(contentsOf:at:)`를 사용할 수 있다.

마찬가지로 단일 문자열의 제거를 원하면 `remove(at:)`을, 지정된 범위에서 하위 문자열을 제거하려면 `removeSubrange(_:)`를 사용할 수 있다.

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome now equals "hello!"

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"

welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```



## Substrings

문자열에서 서브스트링을 얻을 때 (서브스크립트를 사용하거나 `prefix(_:)` 메서드를 사용하는 등), 그 결과 값은 서브스트링의 인스턴스이지 다른 문자열이 아니다. Swift의 서브스트링은 문자열과 거의 동일한 메서드를 가지고 있다. 즉, 문자열로 작업하는 것과 동일한 방식으로 서브스트링을 작업할 수 있다는 뜻이다. 그러나 문자열과는 다르게, 문자열에 대한 작업하는 잠시 동안에만 서브스트링을 사용한다. 결과 값을 더 오래 사용하려면 서브스트링을 문자열 인스턴스로 변환해야한다. 예를 들어:

```swift
let greeting = "Hello, world!"
let index = greeting.firstIndex(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
// beginning is "Hello"

// Convert the result to a String for long-term storage.
let newString = String(beginning)
```

문자열과 마찬가지로 각 서브스트링에는 서브스트링을 구성하는 문자가 저장되는 메모리 영역이 있다. 문자열과 서브스트링의 차이점은 성능 최적화를 위해 서브스트링이 원래 문자열을 저장하는데 사용된 메모리의 일부 또는 다른 서브스트링을 저장하는데 사용되는 메모리 일부를 재사용할 수 있다는 것이다. (문자열도 유사한 최적화를 갖지만, 두 문자열이 메모리를 공유한다면 두 문자열은 동일하다.) 이 말은, 문자열이나 서브스트링을 수정할 때까지 메모리 복사의 성능 비용을 지불할 필요가 없음을 의미한다. 위에서 언급햇듯, 서브스트링은 장기 저장에 적합하지 않다. (원래 문자열의 저장 공간을 재사용하기 때문에) 고로, 서브스트링이 사용되고 있는 동안 원래 문자열은 메모리에 유지되어야만 한다.

![../_images/stringSubstring_2x.png](https://docs.swift.org/swift-book/_images/stringSubstring_2x.png)

>   문자열과 서브스트링은 모두 `StringProtocol`을 준수한다. 즉, 위에서와 같은 이유로 문자열 조작 함수가 `StringProtocol`을 값을 받아들이는 것이 편리한 경우가 많다.



## Comparing Strings

Swift는 텍스트 값을 비교하는 세 가지 방법을 제공한다.

1.  문자열과 문자가 같음을 비교
2.  접두사가 같음을 비교
3.  접미사가 같음을 비교

'Equal' 연산자 (`==`)와 'Not Equal' 연산자(`!=`)를 사용하여 문자열과 문자가 같은지 확인할 수 있다.

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

확장 자소 클러스터가 정규적으로 동일하다면, 두 개의 문자열 값 (또는 두 개의 문자 값)은 동일한 것으로 간주된다. 확장 자소 클러스터는 다른 유니코드 스칼라로 구성되어 있더라도, 언어적 의미와 모양이 같으면 정규적으로 동일함을 의미한다.

예를 들어, é(U+00E9)는 e(U+0065) 와 `(U+0301)의 결합으로도 표현될 수 있으며, 이는 모두 문자 é를 나타내는 표현이다. 즉, 정규적으로 동일한 것으로 간주된다.

```swift
// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

반대로, 영어 알파벳 A(U+0041 또는 "A")와 러시아어에서 사용되는 A(U+0410 또는 "A")는 동일하지 않다. 두 문자는 시각적으로 차이가 없지만 언어적인 의미는 동일하지 않기 때문이다.

```swift
let latinCapitalLetterA: Character = "\u{41}"

let cyrillicCapitalLetterA: Character = "\u{0410}"

if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters aren't equivalent.")
}
// Prints "These two characters aren't equivalent."
```

>   Swift의 문자열 및 단일 문자 비교는 지역을 구분하지 않는다.

문자열에 특정한 접두사 또는 접미사가 있는지 확인하려면 문자열의 `hasPrefix(_:)`또는 `hasSuffix(_:)`메서드를 호출한다. 둘 다 문자열 하나만을 인수로 받고, 부울 값을 반환한다.

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]

var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// Prints "There are 5 scenes in Act 1"

var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        mansionCount += 1
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        cellCount += 1
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// Prints "6 mansion scenes; 2 cell scenes"
```

>   위의 두 메서드는 문자열 및 단일 문자 비교를 수행할 때, 확장 자소 클러스터가 정규적으로 동등한지 비교도 함께 수행한다.



## Unicode Representations of Strings

유니코드 문자열이 텍스트 파일 또는 다른 저장소에 기록될 때, 해당 문자열의 유니코드 스칼라는 여러 유니코드 정의 인코딩 형식 중 하나로 인코딩된다. 각 양식은 작은 청크(코드 단위)로 문자열을 인코딩한다. 유니코드 인코딩 형식은 여러가지가 있다:

-   UTF-8 (문자열을 8비트 코드 단위로 인코딩)
-   UTF-16 (문자열을 16비트 코드 단위로 인코딩)
-   UTF-32 (문자열을 32비트 코드 단위로 인코딩)

Swift는 문자열 유니코드 표현에 접근하는 여러가지 방법을 제공한다. `for-in` 구문으로 문자열을 반복하면, 확장 자소 클러스터로 개별 문자 값에 접근할 수 있다. 또는 다음 세 가지 다른 유니코드 호환 표현 중 하나로 문자열 값에 접근한다.

-   UTF-8 코드 단위 모음 (문자열의 `utf8` 프로퍼티로 접근)
-   UTF-16 코드 단위 모음 (문자열의 `utf16` 프로퍼티로 접근)
-   UTF-32 인코딩 형식과 동등한 21비트 유니코드 스칼라 값의 모음 (문자열의 `unicodeScalars` 프로퍼티로 접근)

아래의 각 예는 문자 D, o, g, !!(U+203C) 및 🐶(U+1F436) 문자로 구성된 다음 문자열의 다른 표현을 보여준다.

```swift
let dogString: String = "Dog!!🐶"
```

### UTF-8 표현

`utf8` 프로퍼티를 반복하여 문자열의 UTF-8 표현에 접근할 수 있다. 이 프로퍼티는  `UInt8` 값 모음인 `String.UTF8View` 타입이다.

![../_images/UTF8_2x.png](https://docs.swift.org/swift-book/_images/UTF8_2x.png)

```swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 226 128 188 240 159 144 182 "
```

위의 예시에서 처음 세 개의 10진수 codeUnit 값 (68, 11, 103)은 UTF-8 표현이 ASCII 표현과 동일한 문자 D, o, g를 나타낸다.

다음 세 10진수 codeUnit 값 (226, 128, 188)은 !! 문자의 3바이트 UTF-8 표현이다.

마지막 4개의 codeUnit 값은 개 이모티콘의 4바이트 UTF-8 표현이다.



### UTF-16 표현

`utf16` 프로퍼티를 반복하여 문자열의 UTF-8 표현에 접근할 수 있다. 이 프로퍼티는  `UInt16` 값 모음인 `String.UTF16View` 타입이다.

![../_images/UTF16_2x.png](https://docs.swift.org/swift-book/_images/UTF16_2x.png)

```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 55357 56374 "
```

앞의 D, o, g의 유니코드 스칼라는 ASCII 문자를 나타내기 때문에 UTF-8 과 동일한 값을 나타낸다.

네 번째 codeUnit 값(8252)은 !! 문자에 대한 유니코드 스칼라 U+203C를 나타내는 16진수 값 203C에 해당하는 10진수이다. 이 문자는 UTF-16의 단일 코드 단위로 표현될 수 있다. (위 표를 참고했을 때, 개는 2개의 codeUnit으로 표현되므로 단일 코드 단위로 표현될 수 없음)

다섯 번째 및 여섯 번째 codeUnit 값(55457 및 56374)은 🐶의 UTF-16 써로게이트 짝 표현이다. 이 값들은 상위 써로게이트 값인 U+D83D(55357)와 하위 써로게이트 값인 U+DC36(56374)으로 구성된다.

>   ### Surrogate(써로게이트) Pair 란
>
>   U+10000 이상은 써로게이트 영역이라 하여, 인코딩 값 2개를 합쳐서 구성되어있다.
>
>   즉, 다국어 평변에 포함되지 않는 문자(16비트로 값을 표현할 수 없는 문자들)은, 써로게이트 문자 영역에 해당하는 2개의 16비트 문자로 변환되어, 이들 한 쌍(즉, 32비트)이 그 문자를 나타내게 된다.
>
>   이 들은 상위 값과 하위 값을 가진다.
>
>   *Reference: [정보통신기술용어해설](http://www.ktword.co.kr/abbr_view.php?m_temp1=670)*



### 유니코드 스칼라 표현

`unicodeScalars` 프로퍼티를 반복하여 문자열의 유니코드 스칼라 표현에 접근할 수 있다. 이 프로퍼티는  `UnicodeScalar` 값 모음인 `UnicodeScalarView` 타입이다.

각 `UnicodeScalar`에는 스칼라의 21비트 값(`UInt32` 값으로 표현된)을 반환하는 `value` 프로퍼티가 있다.

![../_images/UnicodeScalar_2x.png](https://docs.swift.org/swift-book/_images/UnicodeScalar_2x.png)

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 128054 "
```

처음 세 개의 유니코드 스칼라 값은 D, o, g 를 나타낸다. 이는 UTF-8, UTF-16 모두 동일하다.

네 번째 값은 !! 문자에 대한 유니코드 스칼라(U+203C)를 나타내는 16진 값 203C에 해당하는 10진수이다. 이 또한 UTF-16과 동일하다.

마지막 스칼라 값인 128054는 🐶문자에 대한 유니코드 스칼라(U+1F436)을 나타내는 16진수 값 1F436에 해당하는 10진수이다.

`value` 프로퍼티를 쿼리하는 대신, 각 `UnicodeScalar` 값을 사용하여 문자열 보간과 같이 새 문자열 값을 생성할 수도 있다.

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```
