[Firestore 공식문서](https://firebase.google.com/docs/firestore/quickstart)
```dart
flutter pub add cloud_firestore
```
<br>
### 데이터 구조

firestore는 `collection`과 `document`로 이루어져 있다. `document`에 데이터가 담겨 있고, `collection`은 그러한 `document`들로 이루어져 있다.
- `document`안에 또 다른 `collection`을 넣을 수도 있다.
<br>

### 데이터 저장하기
```dart
import 'package:cloud_firestore/cloud_firestore.dart';

await FirebaseFirestore.instance
		.collection('users')
		.doc(userCredentials.user!.uid)
		.set({
	'username': _enteredUsername,
	'email': _enteredEmail,
	'imageUrl': imageUrl,
});
```
- `collection`을 만들고 그 안에 데이터를 넣을 `document`를 만든다. 데이터는 `.set()`함수 안에 **`Map`** 형태의 데이터를 넣어주면 된다. 
<br>
### 데이터 읽어오기
```dart
final userRef = FirebaseFirestore.instance.collection('users')
```