# Customizing the Separator Appearance

>   ## Reference
>
>   1.  Apple Developer Documentation - [SeparatorStyle]()
>   2.  Apple Developer Documentation - [SeparatorInset](https://developer.apple.com/documentation/uikit/uitableview/1614851-separatorinset)
>   3.  Apple Developer Documentation - [SeparatorInsetReference](https://developer.apple.com/documentation/uikit/uitableview/separatorinsetreference)
>   4.  Apple Developer Documentation - [readableContentGuide](https://developer.apple.com/documentation/uikit/uiview/1622644-readablecontentguide/)
>   5.  Apple Developer Documentation - [cellLayoutMarginsFollowReadableWidth](https://developer.apple.com/documentation/uikit/uitableview/1614849-celllayoutmarginsfollowreadablew)
>   6.  Apple Developer Documentation - [SeparatorColor](https://developer.apple.com/documentation/uikit/uitableview/1614984-separatorcolor)
>   7.  StackOverFlow - [What is UITableView separatorEffect property for?](https://stackoverflow.com/questions/26090913/what-is-uitableview-separatoreffect-property-for)



## SeparatorStyle

`tableView(_:cellForRowAt:)` 에서 반환되는 셀의 구분선 스타일을 설정한다.

해당 프로퍼티 타입은 `UITableViewCell.SepartorStyle` 열거형이며, 다음 값들이 존재한다.

1.  `none` : 구분선이 없도록 설정한다.
2.  `singleLine` : 기본 값으로, 하나의 줄로 구분선을 설정한다.
3.  `singleLineEtched` : iOS 11.0 + 부터 deprecated 되어, 사용하지 않는다.



## SeparatorInset

셀 구분선의 인셋 값이다.

`rowHeight` 속성이 셀의 기본 높이를 설정하는 것처럼, 해당 속성도 모든 셀에 대하여 기본 인셋 값을 설정한다.

iOS 7.0 + 부터는 셀 구분선이 테이블 뷰의 끝까지 확장되지 않는다.

셀이 제공하는 이미지 뷰 속성을 사용하고 있다면, 이미지를 기준으로 구분선 인셋이 자동으로 적용된다. (하단 이미지 참고)

<img width="300" alt="스크린샷 2021-07-17 14 02 06" src="https://user-images.githubusercontent.com/73573732/126026316-8c111879-a7f9-440e-b5a9-60965edc2660.png">

만약, 양쪽 인셋을 10씩 추가하고 싶다면 아래 코드로 설정할 수 있다. 

```swift
separatorInset = UIEdgeInset(top: 0, left: 10, bottom: 0, right: 10)
```

`top`과 `bottom`에 값을 주어도 해당 속성에서는 적용되지 않으며, RTL의 경우 속성 값이 자동으로 반전된다.



## SeparatorInsetReference

구분선 인셋을 설정할 때, 어떤 방법을 기준으로 잡을지 설정하는 값이다. 해당 프로퍼티는 다음 값을 포함한다.

1.  `fromCellEdges` : 셀의 끝 부분부터 여백을 계산하는 방법이다.
2.  `fromAutomaticInsets` : 사용 환경 또는 설정에 따라 자동으로 여백을 계산하는 방법이다.

iOS 11.0 + 부터는 기본으로 `fromCellEdges`가 기본 값이 되었기 때문에, `UIEdgeInset` 이니셜라이저를 사용해서

```swift
separatorInset = UIEdgeInset(top: 0, left: 10, bottom: 0, right: 10)
```

값을 설정하면 셀 양쪽 끝 부분부터 10씩 여백을 가지도록 구분선을 생성한다.

iOS 11.0 이전에는 `readableContentGuide` 를 기준으로 설정했기 때문에 결과가 다를 수 있다.

만약 iOS 11.0 이전처럼 `readableContentGuide`을 기준으로 구분선을 설정하려면 해당 프로퍼티 값을 `fromAutomaticInsets` 으로 설정한 후, `cellLayoutMarginsFollowReadableWidth` 값을 `true`로 설정해주면 된다.

보통 `readableContentGuide`는 화면이 넓은 iPad에서 테이블 뷰 컨텐츠를 사용자가 조금 더 보기 쉽게 하기위함인 것 같다.

## SeparatorColor

셀 구분선의 색상을 설정하는 값이며, 기본 값은 회색이다.



## SeparatorEffect

`UIBlurEffect`를 통한 흐림처리 또는 여러 효과에서 구분선에도 효과를 줄 때 사용하는 속성인 듯하다.