카카오 로그인을 구현하다보면 `main.dart`에서 네이티브 앱키를 넘겨줘야 하는 상황이 생긴다
```dart
void main() async {
	WidgetsFlutterBinding.ensureInitialized();
	
	//네이티브 앱 키가 그대로 노출됨
	KakaoSdk.init(nativeAppKey: "12341234");
	
	runApp(const MyApp());
}
```
dart 파일 내에서 키를 하드코딩해 키가 노출되지 않도록 `.env`파일로 키를 관리해야 한다
<br>

#### .env 파일 생성
프로젝트 디렉토리에 .env 파일을 생성하고 .gitignore에 추가한다.
[.gitignore 사용법](/git/gitignore%20사용)
![](/images/Pasted%20image%2020240225160352.png)
![](/images/Pasted%20image%2020240225160428.png)
- `*.env`는 모든 .env 파일을 무시하겠다는 뜻
<br>

#### flutter_dotenv 설치
[flutter_dotenv 설치](https://pub.dev/packages/flutter_dotenv/install)
flutter_dotenv는 .env에서 키를 관리하고 쉽게 호출할 수 있도록 도와준다
```dart
$ flutter pub add flutter_dotenv
```
#### pubspec.yaml
사진 추가할 때와 똑같이 `assets:` 속성에 `- .env`를 추가한다.
```yaml
assets:
	- assets/images/이미지1.png
	- assets/images/이미지2.png
	- .env
```

#### .env 안에 키 정의
필요한 키들을 밑에서 호출할 때 사용할 이름과 함께 정의한다. 따옴표 안에 
```env
NATIVE_APP_KEY="12341234"
```
#### 키 호출
```dart
import 'package:flutter_dotenv/flutter_dotenv.dart';
//아까 정의한 이름으로 호출
dotenv.env['NATIVE_APP_KEY'];
```
- 필요한 dart 파일 내에서 패키지 import하면 위와 같이 정의한 이름으로 키 호출 가능
<br>

```dart
import 'pacakge:flutter_dotenv/flutter_dotenv.dart';

void main() async {
	WidgetsFlutterBinding.ensureInitialized();
	
	//네이티브 앱 키가 그대로 노출됨
	KakaoSdk.init(nativeAppKey: dotenv.env['NATIVE_APP_KEY']);
	
	runApp(const MyApp());
}
```
- 이제 아까 하드코딩으로 들어갔던 네이티브 앱키를 보호할 수 있음