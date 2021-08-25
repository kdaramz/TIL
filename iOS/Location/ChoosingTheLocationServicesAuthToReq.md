# Choosing the Location Services Authorization to Request

> ## Reference
>
> 1. Apple Developer Documentation - [Choosing the Location Services Authorization to Request](https://developer.apple.com/documentation/corelocation/choosing_the_location_services_authorization_to_request)
> 2. Medium: ITNEXT - [Swift iOS CLLocationManager All-In-One](https://itnext.io/swift-ios-cllocationmanager-all-in-one-b786ffd37e4a)
> 3. Hypertrack - [Impact of iOS13 and iOS14 permissions on background location access](https://hypertrack.com/blog/2020/06/24/impact-of-ios13-ios14-location-permissions-on-background-location-access/)



앱에서 위치 서비스를 이용하려면, 사용자에게 동의를 받아야 한다. 앱이 사용자에게 요청할 수 있는 권한은 두 종류가 있다.

<img width="1308" alt="위치 서비스 권한 동의 차이점" src="https://user-images.githubusercontent.com/73573732/130798612-3c98242c-0944-488d-80ef-e8977d121704.png">

- **When In Use**

    앱이 사용되는 동안에 모든 위치 서비스 이용을 허용한다.

    앱이 포어그라운드에서 실행되거나 위치 표시기가 활성화된 백그라운드에서 실행될 때 사용 중인 것으로 간주된다.

    <img width="370" alt="위치표시기 설명" src="https://user-images.githubusercontent.com/73573732/130793208-662e1184-f8ff-4b01-bf40-fa08d6020b50.png">

- **Always**

    사용자가 앱이 실행 중임을 인지하지 못하는 경우에도 모든 위치 서비스 이용을 허용한다.

    앱이 실행 중이 아닐 때도 시스템이 앱을 실행하고 위치 서비스를 통한 이벤트를 수신할 수 있다.



## Prefer When In Use Authorization

애플에서는 가능하면 **사용 중 승인**에 대해서만 요청하는 것을 권장하고 있다. 해당 모드는 다음과 같은 기능을 제공한다 :

- 사용자가 앱을 사용하는 동안 사용 가능한 모든 위치 서비스에 엑세스한다. 사용자가 앱 사용을 중단하면 엑세스도 일시 중지된다.

- 앱에 백그라운드 위치 업데이트가 활성화된 상태라면, 백그라운드 상태에서도 지속적으로 위치 업데이트를 받을 수 있다.

    <img width="738" alt="백그라운드 모드 활성화" src="https://user-images.githubusercontent.com/73573732/130799192-3c34900e-bef1-4f6d-b7c8-281c860dba6f.png">

- `UNLocationNotificationTrigger`를 통해 특정 지역을 진입하거나 벗어났을 때, 푸시 노티피케이션으로 사용자가 앱과 상호작용하도록 할 수 있다. (i.g. Reminder 지역 알림)

위치 서비스는 `CLAuthorization.authorizedWhenInUse`인 앱이 사용 중인 경우에만 사용할 수 있다. 사용 중 승인을 지원하는 모든 플랫폼에서는 다음과 같은 상황을 사용 중인 것으로 간주한다 :

- 앱이 포어그라운드에서 실행 중일 때

- 앱이 포어그라운드를 막 벗어났을 때(사용자가 시작한 현재 위치 작업을 처리할 수 있는 몇 초 간의 유예 기간이 있다.)

- 앱이 백그라운드에서 위치 사용 표시기를 표시할 때(`showBackgroundLocationIndicator`)

    좌측은 노치를 적용한 아이폰, 우측은 이전 아이폰이 위치 사용 표시기를 사용할 때의 상태바 색상을 나타낸다.

    <img width="721" alt="아이폰 위치 표시기" src="https://user-images.githubusercontent.com/73573732/130799827-87503951-c43a-481d-b951-26335eb6a428.png">



## Ask for Always Authorization

위치 정보가 필요할 때마다 사용자가 앱을 제어할 수 없거나 앱을 사용하고 싶지 않은 상황이 있을 경우, **항상 승인**이 필요할 수 있다. 다음과 같은 경우 항상 승인을 요청해야할 수 있다 :

- Home IoT와 같은, 자동화된 작업을 수행해야할 때
- 일기 앱과 같은, 많은 기록을 해야하거나 사용자가 위치 서비스를 항상 사용하는 것을 선호할 때

항상 승인을 요청했다고 해서 앱에서 승인이 보장되는 것은 아니다. 항상 승인을 요청하는 경우, 사용 중 승인으로 권한을 변경할 수 있도록 준비해야 한다.

> #### Note
>
> **사용 중 승인** 권한을 요청한 경우, 나중에 **항상 승인** 권한을 별도로 요청할 수 있다. 하지만 **항상 승인**은 단 한 번만 요청할 수 있다.

사용 중 승인에 대해서는 모든 플랫폼이 지원하지만 항상 승인의 경우, 플랫폼마다 상이하다 :

- tvOS

    사용 중 승인만 지원한다.

- watchOS

    일반적으로 사용 중 승인만 필요하다.

- macOS

    사용 중 승인과 항상 승인의 기능적 차이는 없으며, 승인 및 거부만 존재한다.

- Mac Catalyst

    iOS에서와 macOS에서의 각각의 특징에 맞게 지원한다.



## Request Authorization for Apps Running in iOS 12 and Earlier

iOS 12 이하에서 위치 서비스를 사용하기 위해 필요한 권한은 다음과 같다 :

| Service                                                      | When In Use | Always |
| ------------------------------------------------------------ | :---------: | :----: |
| Standard location service                                    |    지원     |  지원  |
| Significant-change location service (사용자의 위치가 크게 변경될 때) |   불가능    |  지원  |
| Visits service (특정된 지역을 진입 혹은 이탈할 때)           |   불가능    |  지원  |
| Region monitoring                                            |   불가능    |  지원  |
| iBeacon ranging                                              |    지원     |  지원  |
| Heading service (방향, 나침반)                               |    지원     |  지원  |
| Geocoding services (위도 및 경도)                            |    지원     |  지원  |

