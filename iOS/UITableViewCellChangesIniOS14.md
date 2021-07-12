# UITableViewCell Changes in iOS 14

>   ## Reference
>
>   1.  [ZeddiOS - Modern Cell Configuration(1)](https://zeddios.tistory.com/1205)

iOS 14.0 이하에서는 

```swift
let cell: UITableViewCell = ...

cell.textLabel?.text = "Hello, World!"
cell.detailTextLabel?.text = "Welcome to iOS."
cell.imageView?.image = UIImage(systemName: "like")
```

위 코드처럼 정해진 셀 타입을 사용해서 간단하게 사용할 수 있었다. (단, `detailTextLabel` 프로퍼티는 해당 프로퍼티를 지원하는 타입에서만 사용가능)

하지만 해당 프로퍼티들은 iOS 14.0 부터 deprecated 됐다.

<img width="819" alt="deprecated" src="https://user-images.githubusercontent.com/73573732/125259026-e637f500-e339-11eb-812b-6d9513136282.png">

그래서 문서에 추가적으로

<img width="739" alt="deprecated2" src="https://user-images.githubusercontent.com/73573732/125259666-7d9d4800-e33a-11eb-984e-81cdca4464e1.png">

라고 되어있는데, `defaultContentConfiguration()`을 사용하라고 되어있다.

그래서 iOS 14.0 부터는

```swift
let cell: UITableViewCell = ...
var content = cell.defaultContentConfiguration()

content.text = "Hello, World!"
content.secondaryText = "Welcome to iOS."
content.image = UIImage(systemName: "like")

cell.contentConfiguration = content
```

처럼 사용할 수 있다. 조금 주의해야할 부분은, 기존 프로퍼티들의 이름이 변경됐다는 점이다.

`textLabel`은 `text`, `detailTextLabel`은 `secondaryText`로 변경되었다.

그리고 구현상의 차이점도 약간 존재하는 것 같다.

<img width="300" alt="14+" src="https://user-images.githubusercontent.com/73573732/125273058-7f213d00-e347-11eb-8c43-42a3b5c61188.png">     <img width="300" alt="14-" src="https://user-images.githubusercontent.com/73573732/125273096-88aaa500-e347-11eb-8307-d9ad72952429.png">

왼쪽은 14.0부터 적용되는 방식이고, 오른쪽은 이전에 사용해오던 방식이다. 단순한 방법의 차이뿐만 아니라 이미지 또는 텍스트의 제약조건이 약간 수정된 것으로 보인다.

