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
![](images/Pasted%20image%2020240225160352.png)
![](images/Pasted%20image%2020240225160428.png)
- `*.env`는 모든 .env 파일을 무시하겠다는 뜻
<br>

#### flutter_dotenv 설치
[flutter_dotenv 설치](https://pub.dev/packages/flutter_dotenv/install)

```dart
$ flutter pub add flutter_dotenv
```
