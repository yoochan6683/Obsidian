### 라이브러리 설치
[Firebase CLI 설치하기](https://firebase.google.com/docs/flutter/setup?platform=ios)
파이어베이스를 앱에 설치하기 전에 CLI부터 설치해야 한다.
<br>

### 라이브러리 설치 후
설명을 따라 설치하고 나면 2가지 변화가 생기게 된다.
<<<<<<< HEAD
- firebase_options.dart 파일 생성<br>
![이미지](/images/Pasted%20image%2020240128220156.png)<br>
=======
- firebase_options.dart 파일 생성
![이미지](/images/Pasted%20image%2020240128220156.png)

>>>>>>> origin/main
- main.dart에 코드 추가
```dart
void main() async {
	WidgetsFlutterBinding.ensureInitialized();
	await Firebase.initializeApp(
		options: DefaultFirebaseOptions.currentPlatform,
	);
	runApp(const App());
}
```
Firebase는 초기화를 위해 `Firebase.initializeApp()`를 호출해야 한다. 이 과정에서 플랫폼 별(Android, iOS) 네이티브 코드를 호출해야 하는데, 이게 비동기적으로 이루어지기 때문에 비동기처리와 `WidgetsFlutterBinding.ensureInitialized()`코드가 필요하다. 
<br>
### 이메일, 비번으로 유저 생성하기
```dart
import 'package:firebase_auth/firebase_auth.dart';

final _firebase = FirebaseAuth.instance;

//...
await _firebase.createUserWithEmailAndPassword(
				email: _enteredEmail, 
				password: _enteredPassword);
```
- `FirebaseAuth.instance`를 통해 `createUserWithEmailAndPassword()`함수를 호출하고, 일반적인 `String`으로 저장해놓은 `_enteredEmail`과 `_enteredPassword`를 파라미터로 넘겨주면 된다. 물론 비동기처리는 필수다.
<br>

### 이메일, 비번으로 로그인하기
```dart
import 'package:firebase_auth/firebase_auth.dart';

await _firebase.signInWithEmailAndPassword(
				email: _enteredEmail, 
				password: _enteredPassword);
```
- 마찬가지 방법으로 `signInWithEmailAndPassword()`함수를 호출하고 `_enteredEmail`과 `_enteredPassword`를 넘겨주면 된다.
<br>

![](/images/Pasted%20image%2020240129210058.png)<br>
이곳에서 가입한 유저 정보를 확인할 수 있다.
<br>
### 로그아웃하기
```dart
FirebaseAuth.instance.signOut();
```
- 필요한 곳에다가 이 함수를 호출하기만 하면 된다. 로그인/로그아웃 상태에 따라 화면을 다르게 처리하는 것은 StreamBuilder 위젯을 통해 가능하다.
[[로그인 상태에 따라 화면전환하기(StreamBuilder)]]
