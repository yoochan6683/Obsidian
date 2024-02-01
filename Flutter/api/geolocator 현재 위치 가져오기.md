## 환경 설정
### Android
#### AndroidX 버전 관리
geolocator 패키지를 사용하려면 AndroidX 버전이 필요하다고 한다.
```dart
android.useAndroidX=true
android.enableJetifier=true
```
`gradle.properties` 파일에 위 두 문장을 추가한다. 난 원래 그렇게 되어 있었다.
<br>

```dart
android {
  compileSdkVersion 33

  ...
}
```
`android/app/build.gradle`파일에 `compileSdkVersion`을 33으로 바꿔야 한다. 주의할 점은 `android{`밑에 `defaultConfig{}` 안에도 `compileSdkVersion`이 있는데, 위 코드처럼 `android{`바로 밑에 있는 놈을 바꿔야줘야 한다.
<br>
#### 권한 설정
```dart
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```
`android/app/src/main`에 `AndroidManifext.xml`파일에서, `<manifest>` 태그 바로 안에 둘 중 하나를 추가하면 된다. `ACCESS_COARSE_LOCATION`은 상대적으로 빠른 위치 정보이고, `ACCESS_FINE_LOCATION`은 정확한 위치 정보라도 한다. 근데 높은 정확도를 위해 둘 다 적어주는게 좋다고 한다.
<br>
안드로이드 10부터는 백그라운드 권한 설정도 해주어야 한다. 위에 권한 설정한 곳 바로 밑에 추가하면 된다.
```dart
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
```
<br>
<br>

### iOS
```dart
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access to location when open.</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>This app needs access to location when in the background.</string>
```
`iOS/Runner`의 `Info.plist`파일에 추가하면 된다
![](/images/Pasted%20image%2020240122143204.png)
<br>

```dart
<key>NSLocationTemporaryUsageDescriptionDictionary</key>
<dict>
  <key>YourPurposeKey</key>
  <string>The example App requires temporary access to the device&apos;s precise location.</string>
</dict>
```
iOS에서도 마찬가지로 백그라운드 권한 설정을 하려면 위코드를 추가하면 되는데, 정확한 추가 방법은 아래 링크에서 확인할 수 있다.(이해 못함)
[geolocator 다트 패키지](https://pub.dev/packages/geolocator)
<br>

<br>

### geolocator
```dart
import 'package:geolocator/geolocator.dart';

Future<Position> determinePosition() async {
	bool serviceEnabled;
	LocationPermission permission;

	serviceEnabled = await Geolocator.isLocationServiceEnabled();

	if (!serviceEnabled) {
		return Future.error('Location services are disabled.');
	}

	permission = await Geolocator.checkPermission();

	if (permission == LocationPermission.denied) {
		permission = await Geolocator.requestPermission();
		if (permission == LocationPermission.denied) {
			return Future.error('Location permissions are denied');
		}
	}
	
	if (permission == LocationPermission.deniedForever) {
		return Future.error(
'Location permissions are permanently denied, we cannot request permissions.');
	}

	return await Geolocator.getCurrentPosition();
}
```
이제 패키지 설명에 나와 있는 코드를 들고 와서 쓰면 된다. 위치를 받기 위한 권한을 처리하는 과정이 담겨 있다. 반환 값으로는 `Future<Position>`이 반환된다.
<br>

`Position`이란 객체는 위도, 경도부터 고도, 정확도, 속도 등등 다양한 속성을 갖고 있는 객체이다.
![](/images/Pasted%20image%2020240122150817.png)
<br>

여기서 내가 필요한 위도, 경도 값만 뽑아서 쓸 수 있다.
![](/images/Pasted%20image%2020240122150632.png)
<br>

참고로 위도 경도는 `double`타입이다.
![](/images/Pasted%20image%2020240122150953.png)