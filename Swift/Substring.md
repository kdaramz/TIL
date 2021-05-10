# Substring

> Reference: [Swift Document](https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html#ID555)

* 성능 최적화를 위해 `Substring`은 원본 `String`의 메모리 부분을 사용함
  * 장점은 `Substring` 작업에 별도의 메모리를 할당하지 않아도 됨
  * 단점은 `Substring`을 위해 원본 `String`이 메모리에 계속 유지되어야한다는 점. 그래서 `Substring`을 새로운 문자열 인스턴스로 만들어주는 것이 해결방법이 됨
* `String`과 `Substring` 모두 `StringProtocol`을 준수하기 때문에, 관련 메서드를 사용하여 작업할 수 있음