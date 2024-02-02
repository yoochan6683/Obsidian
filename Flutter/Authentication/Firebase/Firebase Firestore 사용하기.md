[Firestore 공식문서](https://firebase.google.com/docs/firestore/quickstart)
```dart
flutter pub add cloud_firestore
```
<br>
### 데이터 구조

firestore는 `collection`과 `document`로 이루어져 있다. `document`에 데이터가 담겨 있고, `collection`은 그러한 `document`들로 이루어져 있다.
<br>
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
