# Dynamic Type

>   ## Reference
>
>   1.   [Apple Developer Documentation - UIContentSizeCategory](https://developer.apple.com/documentation/uikit/uicontentsizecategory)



## 정의

다이나믹 타입은 사용자의 기기 설정에 따라 화면의 텍스트 크기를 조절할 수 있도록 하는 기능을 말한다. 

텍스트 크기를 작게 설정함으로써 화면에 더 많은 정보가 나타나게 하는 것을 선호하는 사용자가 있는 반면, 기본 혹은 작은 사이즈의 텍스트 크기를 읽는 것에 피로를 느끼는 사용자도 있다.

다이나믹 타입의 단계는 기본으로 제공되는 7단계와 접근성 설정에서 해당 옵션을 활성화했을 때 추가되는 5단계로, 총 12단계로 구성된다. 각 타입의 크기는 `UIContentSizeCategory` 구조체의 타입 변수로 구성되어 있다.



## 종류

### Font Siezs

-   `extraSmall`
-   `small`
-   `medium`
-   `large`
-   `extraLarge`
-   `extraExtraLarge`
-   `extraExtraExtraLarge`

### Accessibility Sizes

-   `accessibilityMedium`
-   `accessibilityLarge`
-   `accessibilityExtraLarge`
-   `accessibilityExtraExtraLarge`
-   `accessibilityExtraExtraExtraLarge`

