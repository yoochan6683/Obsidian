플러그인 설치
```dart
flutter pub add firebase_storage
```
<br>

```dart
import 'package:firebase_storage/firebase_storage.dart';
```
<br>

```dart
final storageRef = FirebaseStorage.instance.ref()
					.child('user_images')
					.child('${userCredentials.user!.uid}.jpg');

await storageRef.putFile(_selectedImage!);
```
- `FirebaseAuth`때와 유사하게 `FirebaseStorage.instance`로 필요한 함수를 호출할 수 있다.`ref()`를 통해 저장공간에 접근할 수 있고, `child()`를 통해 파일을 생성하고 내부에 접근할 수 있다.
<br>
- 이후 그 공간에 `putFile()`을 통해 `File _selectedImage`를 저장한다.
<br>

- `user-images`라는 파일을 하나 만들고, 그 안에 유저의 사진을 저장하고 있다. 이때 유저 고유의 id를 통해 사진 이름을 설정하여 나중에 어느 사진이 어느 유저의 것인지 확인할 수 있도록 한다. 유저의 id는 아래 코드로부터 받아올 수 있다.
```dart
final userCredentials = FirebaseAuth.instance.createUserWithEmailAndPassword(email: _enteredEmail, password: _enteredPassword);
```
- `userCredentials.user`는 유저에 대한 정보에 접근할 수 있는 함수이다. 
<br>
#### Storage에 저장된 이미지 가져오기
```dart
final imageUrl = await storageRef.getDownloadURL();
```
