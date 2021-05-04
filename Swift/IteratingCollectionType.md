# Iterating collection type

> [스위프트 공식 문서](https://docs.swift.org/swift-book/LanguageGuide/CollectionTypes.html) 참조

<br/>



일반 배열을 반복할 때는 쉽게 사용할 수 있다.

```swift
let campManagers: [String] = ["지니", "코지"]

for campManager in campManagers {
    print(campManager)
}

// 지니
// 코지
```

만약 반복되는 배열에 인덱스를 부여하고 싶다면, 위의 코드를 조금 변형할 수 있다.

```swift
let campManagers: [String] = ["지니", "코지"]

for (index, campManager) in campManagers.enumerated() {
    print(index, campManager)
}

// 0 지니
// 1 코지
```

`enumerated()` 메서드를 사용하면 0부터 시작하는 인덱스를 부여할 수 있다. 하지만 주의할 점이 있는데, 모든 컬렉션 타입이 원소들의 순서를 보장하지 않는다는 점이다. 배열의 경우는 순서가 보장되는 타입이지만 세트 또는 딕셔너리는 순서를 보장하지 않는 원소들이다.

</br>



원소의 순서를 보장하지 않는 세트 또는 딕셔너리를 반복한 후, 인덱스로 접근하고 싶을 때는 `zip(_:_:)` 메서드를 활용할 수 있다.

```swift
let campers: [String: String] = ["글렌": "부산", "이니": "경기도", "오동나무": "부산", "찌로": "서울"]
var dictionaryIndicies: [Dictionary<String, String>.Index] = []
for (index, camper) in zip(campers.indices, campers) {
    if camper.value == "부산" {
        dictionaryIndicies.append(index)
    }
}

for index in dictionaryIndicies {
    print(campers[index])
}

// (key: "글렌", value: "부산")
// (key: "오동나무", value: "부산")
```