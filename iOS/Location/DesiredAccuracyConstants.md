# Desired Accuracy Constatns

> ## Reference
>
> 1. Apple Developer Documentation - [desiredAccuracy](https://developer.apple.com/documentation/corelocation/cllocationmanager/1423836-desiredaccuracy)
> 2. Apple Developer Documentation - [kCLLocationAccuracyBest](https://developer.apple.com/documentation/corelocation/kcllocationaccuracybest)
> 3. Apple Developer Documentation -  [kCLLocationAccuracyBestForNavigation](https://developer.apple.com/documentation/corelocation/kcllocationaccuracybestfornavigation) 
> 4. Apple Developer Documentation -  [kCLLocationAccuracyNearestTenMeters](https://developer.apple.com/documentation/corelocation/kcllocationaccuracynearesttenmeters) 
> 5. Apple Developer Documentation -  [kCLLocationAccuracyHundredMeters](https://developer.apple.com/documentation/corelocation/kcllocationaccuracyhundredmeters) 
> 6. Apple Developer Documentation -  [kCLLocationAccuracyKilometer](https://developer.apple.com/documentation/corelocation/kcllocationaccuracykilometer) 
> 7. Apple Developer Documentation - [kCLLocationAccuracyThreeKilometers](https://developer.apple.com/documentation/corelocation/kcllocationaccuracythreekilometers)  
> 8. Apple Developer Documentation -  [kCLLocationAccuracyReduced](https://developer.apple.com/documentation/corelocation/kcllocationaccuracyreduced) 

<br/>



## desiredAccuracy

앱이 위치정보를 수신할 때의 정확도를 설정하는 프로퍼티이다.

앱이 정확한 위치 정보에 엑세스할 수 있는 권한이 없는 경우(`isAuthorizedForPreciseLocation` 이 `false` 일 때)에는 해당 프로퍼티 값은 항상 `kCLLocationAccuracyReduced` 이다. 즉, 값의 변경에 영향을 받지 않는다.

해당 이미지는 `isAuthorizedForPreciseLocation` 값이 `true` 일 때 권한을 요청하는 이미지이다. 지도에 <kbd>Precise: On</kbd> 이라고 표시된다.

<img width="223" alt="isAuthorizedForPreciseLocation" src="https://user-images.githubusercontent.com/73573732/131295814-427caf7e-7ea0-4e90-9556-2cc28ae6bd2b.png">

위치 데이터의 정확도에 따라 배터리 사용량과 위치 데이터를 사용하기까지의 시간이 증가하기 때문에, 배터리에 가해지는 영향과 수행 시간을 줄이려면 사용에 따라 적절한 값을 사용하는 것이 좋다고 한다.

높은 정확도의 위치 데이터를 요청하면 위치 데이터를 사용하기까지의 시간이 더 소요되기 때문에, 일정 기간 동안은 낮은 정확도의 위치 데이터를 사용할 수 있다. 높은 정확도의 위치 데이터를 사용할 수 있게 될 때, 더 정확한 위치 데이터를 수신한다.

iOS의 경우, 해당 프로퍼티의 기본 값은 `kCLLocationAccuracyBest`이고, macOS, watchOS 및 macOS의 경우는 `kCLLocationAccuracyHundredMeters`이다.

해당 프로퍼티는 중요한 위치 변경을 모니터링하지 않고, 표준 위치 서비스에만 영향을 준다.

<br/>



## kCLLocationAccuracyBest

가장 최고, 최적의 정확도를 제공한다.

`kCLLocationAccuracyBestForNavigation` 만큼은 아니지만, 매우 정확한 수준의 정확도가 필요한 경우 설정한다.

`isAuthorizedForPreciseLocation`가 `true` 일 때만 사용 가능하다.

<br/>



## kCLLocationAccuracyBestForNavigation

내비게이션 앱의 위치 데이터를 최적화하기 위한, 추가 센서 데이터를 사용하여 가장 최고의 정확도를 나타낸다.

항상 정확한 위치 정보가 필요한 내비게이션에서 사용하기 위한 것이므로, 많은 전력을 요구하기 때문에 디바이스가 충전되는 동안에만 사용하는 것을 권장한다고 한다.

`isAuthorizedForPreciseLocation`가 `true` 일 때만 사용 가능하다.

<br/>



## kCLLocationAccuracyNearestTenMeters

원하는 위치와의 오차를 10미터 이내로 설정한다.

`isAuthorizedForPreciseLocation`가 `true` 일 때만 사용 가능하다.

<br/>



## kCLLocationAccuracyHundredMeters

원하는 위치와의 오차를 100미터 이내로 설정한다.

`isAuthorizedForPreciseLocation`가 `true` 일 때만 사용 가능하다.

<br/>



## kCLLocationAccuracyKilometer

원하는 위치와의 오차를 킬로미터 이내로 설정한다.

`isAuthorizedForPreciseLocation`가 `true` 일 때만 사용 가능하다.

<br/>



## kCLLocationAccuracyThreeKilometers

원하는 위치와의 오차를 3킬로미터 이내로 설정한다.

`isAuthorizedForPreciseLocation`가 `true` 일 때만 사용 가능하다.

<br/>



## kCLLocationAccuracyReduced

다른 값들과 다르게 iOS 14 + 부터 지원한다.

해당 값은 대략적인 위치 값을 사용하며, 실제 위치에서 1-20km 이내에 있다. (사용자의 국가 또는 지역을 벗어나지 않는다.)

앱이 위치 정보에 완전히 접근할 수 있는 권한이 있지만, 권한이 없는 것처럼 위치 데이터에 접근하고자 할 때 사용할 수 있다.