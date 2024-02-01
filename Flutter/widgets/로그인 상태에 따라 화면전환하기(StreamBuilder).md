```dart
StreamBuilder(
	stream: FirebaseAuth.instance.authStateChanges(),
	builder: (ctx, snapshot) {
		if(snapshot.connectionState = ConnectionState.waiting) {
			return const SplashScreen();
		}
		if(snapshot.hasData) {
			return const ChatScreen();
		}
		return const AuthScreen();
	}
),
```
- `FutureBuilder`는 기다렸다가 한번 만들어주는거라면, `StreamBuilder`는 `stream:`속성의 값이 변함에 따라서 화면을 바꿔준다.