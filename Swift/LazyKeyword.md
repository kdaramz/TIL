# Lazy

> 지연 저장 프로퍼티를 선언할 때 사용하는 `lazy` 키워드, 제대로 한 번 알아봅시다.



우선 공식 가이드에는 지연 저장 프로퍼티에 대해 다음과 같이 설명하고 있습니다.

> A *lazy stored property* is a property whose initial value is not calculated until the first time it is used.

해당 프로퍼티가 처음 사용되는 곳이 바로 초기화되는 시점이죠. 다음 코드를 봅시다.

```swift
class DataImporter { // 해당 클래스를 초기화하는데 시간이 조금 걸린다고 가정합시다.
    var filename: "data.txt"
    // 여기서 데이터 가져오기 기능을 제공합니다.
}

class DataManager {
    lazy var importer: DataImporter = DataImporter()
    var data: [String] = [] // 여기서 해당 프로퍼티가 초기화 됩니다.
    // 여기서 데이터 관리 기능을 제공합니다.
}

let manager: DataManager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
print(manager.importer.filename) // 여기서 importer 프로퍼티가 초기화 됩니다.
```

위 코드에서  `DataManager`의 데이터 관리 기능은 `DataImporter` 의 데이터 불러오기 기능이 없어도 동작합니다. 그런데 그냥 저장 프로퍼티로 선언했을 경우, 필요 없는 기능 때문에 필요한 작업을 하는데 시간이 더 소요될 수 있습니다. 그렇기 때문에 `lazy` 키워드를 통해 '나중에 초기화 하겠다' 라고 명시하는 것입니다.

그리고 확실하게 알아둬야 할 점이 또 하나 있습니다. 바로 `lazy` 키워드를 통해 프로퍼티를 만들기 위해서는 꼭 `var` 로 선언해야 한다는 것입니다. 왜일까요?

그 이유는 바로 초기화 시점에 답이 있습니다.

우리가 `let` 키워드를 통해 상수를 선언한다는 것은 값의 불변성을 보장하는 것입니다. 그래서 항상 선언과 함께 초기화가 이루어져야 합니다. 그러면 쓸 때 초기화를 하겠다고 말하는,  `lazy` 키워드를 통한 지연 저장 프로퍼티와는 이루어 질 수 없는 관계가 됩니다.

이제 이 정도만 알면 다 된 것 같겠지만, 어림도 없습니다. 신기한 문법이 더 있거든요. 아래 코드는 MemoList라는 테이블 뷰에 들어갈 커스텀 셀 클래스에 `titleLabel` 이라는 레이블을 코드로 오토 레이아웃을 적용시키는 코드입니다.

```swift
// A
class MemoListCell: UITableViewCell {
    lazy var titleLabel: UILabel = {
        let titleLabel = UILabel()
        titleLabel.translatesAutoresizingMaskIntoConstraints = false
        titleLabel.font = .preferredFont(forTextStyle: .headline)
        titleLabel.adjustsFontForContentSizeCategory = true
        return titleLabel
    }()
```

이 문법은 무슨 문법일까요? `lazy` 키워드와 함께 쓰인 연산 프로퍼티일까요? 그런데 조금 모양이 다르군요? 연산 프로퍼티에 `=` 는 없었던 것 같은데... 그리고 스위프트 공식 가이드에 보면 `lazy` 키워드는 연산 프로퍼티와 함께 사용할 수 없다고 명시가 되어있습니다. 그 이유는, `lazy` 는 처음 사용될 때 메모리에 값을 올리고 그 이후 부터는 계속해서 메모리에 올라온 값을 사용합니다. 즉 초기화만 나중에 이루어질 뿐, 초기화가 이루어지고난 후에는 저장 프로퍼티와 동일하게 사용된다는 겁니다. 그런데, 연산 프로퍼티는 사용할 때마다 값을 연산해서 사용하게 됩니다. 역시 `lazy` 키워드와 연산 프로퍼티는 이루어질 수 없는 관계입니다.

그럼 연산 프로퍼티는 아닌 것 같고... 자세히 보니 `return` 키워드가 있네요? 함수일까요? 그런데 함수인데 이름이 없어요. `func` 키워드도 없고... 이런 함수를 뭐라고 했었죠? 맞아요. 클로저입니다. 즉 지연 저장 프로퍼티에 특별한 연산을 한 후, 값을 넣어주려면 클로저를 활용할 수 있습니다. 그리고 이러한 사용에 주의할 점이 하나 있었습니다. 바로 '타입의 명시' 입니다. 다시 위의 코드를 수정해보겠습니다.

```swift
// B
class MemoListCell {
    lazy var titleLabel = { // Error
        let titleLabel = UILabel()
        titleLabel.translatesAutoresizingMaskIntoConstraints = false
        titleLabel.font = .preferredFont(forTextStyle: .headline)
        titleLabel.adjustsFontForContentSizeCategory = true
        return titleLabel
    }()
```

`A` 코드와 `B` 코드가 서로 다른 것은 타입을 명시해줬느냐 안해줬느냐의 차이를 제외하고는 동일합니다. 하지만 B 코드에서는 에러가 납니다. 왜그럴까요? 스위프트는 타입을 추론해주는데 말이죠. 

그 이유는 해당 타입이 반환 타입이기 때문입니다. 우리가 사용했던 함수에서 매개변수로 클로저를 사용할 때는 반환 타입을 추론할 수 있기 때문에 따로 명시해주지 않아도 됐었습니다만, 해당 지연 저장 프로퍼티에서는 타입을 명시해주지 않으면 반환 타입을 추론할 수 없습니다. 그래서 꼭! 타입을 명시해줘야한다는 것을 잊지 말아야 합니다. 

추가적으로 해당 에러는 자동 수정 기능을 지원하는데, 그 기능을 사용하면 아래와 같습니다. 즉,  `B` 코드는 아래 코드의 축약형이라고 볼 수 있겠네요!

```swift
lazy var titleLabel = { () -> UILabel in
        let titleLabel = UILabel()
        titleLabel.translatesAutoresizingMaskIntoConstraints = false
        titleLabel.font = .preferredFont(forTextStyle: .headline)
        titleLabel.adjustsFontForContentSizeCategory = true
        return titleLabel
    }()
```

### Reference
* [Swift Programming Language](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)
* [lazy variables](https://baked-corn.tistory.com/45)
