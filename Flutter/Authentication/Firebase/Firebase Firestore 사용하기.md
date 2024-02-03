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
final userRef = FirebaseFirestore.instance.collection('users').doc(userCredentials.user!.uid);

userRef.get().then((DocumentSnapshot doc) {
	final userData = doc.data() as Map<String, dynamic>;
	//...
})
```
- 일회성으로 한번 가져오는 경우 `.get()`을 통해 접근하고 비동기 처리를 해준 후 `.data()`를 통해 받아올 수 있다. 
<br>

### 데이터 실시간으로 불러오기
`StreamBuilder`를 이용해 데이터가 바뀔 때마다 실시간으로 불러올 수 있다. 
```dart
StreamBuilder(
	stream: FirebaseFirestore.instance
		.collection('user')
		.orderBy(
			'createdAt', 
			decending: false,)
		.snapshots(),
	builder: (ctx, snapshot) {
		final loadedUser = snapshot.data!.docs;

		return ListView.builder(
			itemCount: loadedUser.length,
			itemBuilder: (ctx, index) => Text(loadeUser[index].data()['text']));
	}
),
```
- Firebase에서 제공하는 `snapshots()`함수를 이용하면 해당 경로의 실시간 반영 데이터를 받아올 수 있다. `StreamBuilder`의 `stream:`속성에 `FirebaseFirestore.instance.collection(컬렉션 이름).snapshots()`를 넣어 놓으면 해당 경로의 `document`들이 바뀔 때마다 화면을 새로 그려준다.
- `.orderBy()`함수는 리스트를 불러올 순서를 설정하는 것으로, 선택사항이다. 첫 인자로 순서의 기준이 될 속성(데이터 안에 무조건 있어야 한다.), 두번째 인자로 오름차순으로 할건지 내림차순으로 할건지 정해주면 된다.
- `builder:`속성에서 `snapshot.data.docs`로 불러오며, 리스트 안에서 `index`번째의 `.data()`로 접근하면 된다. 데이터 안의 속성은 `.data()['text'(속성이름)]`으로 접근한다.