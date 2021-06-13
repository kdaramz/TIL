# Strings and Characters

>   Reference
>
>   1.  [Swift.org - Strings and Characters](https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html)
>   2.  [Bridging](https://soooprmx.com/swift-objctive-c와-swift-타입의-브릿징/)



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
>   이렇게 `NSString` 타입의 메서드가 매개변수 및 결과 값을 `NSString`이 아닌, `String` 을 사용하는 것이 가능하다. 자세한 것은 [Bridging]()을 참고.



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
>   <kbd>내용 추가하기</kbd>
>
>   `count` 프로퍼티가 반환하는 문자의 수는 동일한 문자를 포함하는 `NSString` 의 `length` 프로퍼티와 항상 일치하는 것은 아니다. `NSString` 의 길이는 문자열 내의 유니코드 확장 자소 클러스터 수가 아니라 문자열의 UTF-16 표현 내의 16비트 코드 단위 수를 기반으로한다. 아래 코드 예시를 보자.
>
>   ```swift
>   class StringLengthOfSameString {
>       let swift: String = "\u{1112}\u{1161}\u{11AB}" // ㅎ, ㅏ ,ㄴ
>       let objc: NSString = "\u{1112}\u{1161}\u{11AB}" // ㅎ, ㅏ, ㄴ
>   }
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



<kbd>진행 중</kbd>
