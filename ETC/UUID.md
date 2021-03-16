# UUID

> Vapor를 사용하여 Server를 만들다보니 ID의 기본 타입이 UUID인 것을 확인할 수 있었는데, UUID는 무엇이며 어떻게 사용하는지 궁금해져서 공부해봤습니다.

<br/>



## 정의

UUID는 대체 무엇일까요? 우선 위키로 가서 보니, 이렇게 설명을 해놨네요.

> **U**niversally **U**nique **ID**entifier의 약자이며, 소프트웨어 구축에 쓰이는 식별자 표준으로, 개방 소프트웨어 재단(OSF)이 분산 컴퓨팅 환경의 일부로 표준화하였다.

역시나 무슨 말인지 잘 모르겠습니다. 쭉 읽어보면 [아래 쪽 개요](https://ko.wikipedia.org/wiki/%EB%B2%94%EC%9A%A9_%EA%B3%A0%EC%9C%A0_%EC%8B%9D%EB%B3%84%EC%9E%90)에는 잘 설명이 되어있네요! 대강 내용을 추려보면, 

> 네트워크 상에서 서로 모르는 개체들을 정확하게 식별 및 구별하기 위해서 스스로 이름을 짓도록 하되 고유성을 충족할 수 있는 식별자

이라네요? 자세한 사항은 위의 `아래 쪽 개요` 링크를 읽어 보시면 될 것 같습니다. 어쨌든, 고유성을 충족시킨다고 해서 **완벽하게** 고유성을 충족시킬 수 있는 것은 아닌데, 실제 사용 상 중복될 가능성이 거의 없다고 인정되어서 많이 사용된다고 합니다.



## 특징

UUID는 36개 문자(32개 문자와 4개의 `-`)로 이루어져 있으며, 하이픈은 각 그룹을 구분하는 구분자라고 합니다. 예를 들면 다음과 같은 형식이 될 수 있을 것 같네요.

`550e8400-e29b-41d4-a716-446655440000`

이 형식을 `8-4-4-4-12` 포맷이라고 하는 것 같은데, 해당 숫자는 문자의 개수를 뜻합니다. 그냥 보기만 봐도 절대 중복이 힘들 것 같이 느껴지네요...  심지어 위키에 보면

> 340,282,366,920,938,463,463,374,607,431,768,211,456개의 사용 가능한 UUID가 있다.

라고 정확한(?) 수치로도 나타내어 지고 있어요.



## iOS에서 UUID는?

[애플의 UUID](https://developer.apple.com/documentation/foundation/uuid) 를 보면 아래 사진처럼 어떤 방식을 채택하고 있는지 알 수 있습니다. 여기서 RFC 4122의 version 4를 사용하고 있는데, 그 version 4가 random bytes를 이용한 방식이라고 합니다.

<img width="720" alt="스크린샷 2021-03-17 오전 1 41 30" src="https://user-images.githubusercontent.com/73573732/111346863-fa3c5480-86c1-11eb-8fff-9511616606f0.png">

일단 뭔지 모르니까 `RFC 4122` 키워드로 구글로 검색해 들어가서 version 명세를 찾아봅시다! 그럼 [요기](https://tools.ietf.org/html/rfc4122)가 나옵니다. 저는 찾았어요! 아래 사진을 보세요.

<img width="742" alt="스크린샷 2021-03-17 오전 1 46 14" src="https://user-images.githubusercontent.com/73573732/111347497-9cf4d300-86c2-11eb-8fad-84b199a6ebf0.png">



여기 차례대로 Version 1 부터 5까지 설명이 되어있네요. 각 버전의 특징을 살펴보면

1. `Version 1`: 시간을 기반으로 만든 UUID 버전이라고 하네요. MAC 주소와 시간 정보를 가지고 만들기 때문에 언제 UUID를 생성했는지의 정보가 남아서 보안에 문제가 있다고 하네요!
2. `Version 2`: 분산 컴퓨팅 환경(**D**istributed **C**omputing **E**nvironment)의 보안 버전? 이라는데, 잘 쓰이지 않는다고 합니다. ~~잘 모르겠어요~~
3. `Version 3`: MD5 해시를 통해 생성하는 버전이라고 합니다.
4. `Version 4`: 랜덤한 16진수를 이용하여 UUID를 생성하는 버전이라고 합니다.
5. `Version 5`: SHA-1 해시를 통해 UUID를 생성하는 버전이라고 합니다.

각 프로그래밍 언어마다 UUID 버전을 달리 사용한다고 합니다! (자바는 여러 버전 다중 지원이 된다는...) 아무튼 결론은, UUID든 NSUUID든 iOS에서 UUID는 Version 4의 난수 형식으로의 생성 방법을 채택하고 있다는 것이 되겠네요!

## Reference

* [Apple Developer Document - UUID](https://developer.apple.com/documentation/foundation/uuid)
* [Clint Jang님 미디엄 - UUID](https://medium.com/@jang.wangsu/ios-swift-uuid%EB%8A%94-%EC%96%B4%EB%96%A4-%EC%9B%90%EB%A6%AC%EB%A1%9C-%EB%A7%8C%EB%93%A4%EC%96%B4%EC%A7%80%EB%8A%94-%EA%B2%83%EC%9D%BC%EA%B9%8C-22ec9ff4e792)