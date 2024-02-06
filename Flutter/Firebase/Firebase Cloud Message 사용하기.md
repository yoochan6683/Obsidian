[Firebase Cloud Message 공식문서](https://firebase.google.com/docs/cloud-messaging/flutter/client)
<br>

### 1. xcode설정
- 프로젝트 내의 ios/Runner.xcworkspace 파일을 xcode로 연다
![](/images/Pasted%20image%2020240204172149.png)<br>

- `Runner`파일 -> `Signing & Capabilities`항목 -> `+ Capability`버튼을 누른다.
![](images/Pasted%20image%2020240204172423.png)<br>

- `push`검색 -> `Push Notification`을 더블 클릭해서 추가한다
![](images/Pasted%20image%2020240204172651.png)<br>

- 밑에 `Push Notification`이 추가 되었는지 확인하고 `Team`항목에 애플 개발자 계정을 설정한다. 그리고 `Bundle Identifier`의 이름을 살짝 바꾸어 준다(남들이랑 겹치면 안된다).
![](images/Pasted%20image%2020240204172944.png)
에러 메세지가 없어지면  `+ Capability`를 눌러서 `Background Modes`를 추가해준다. 사진에서처럼 `Background fetch`랑 `Remote notifications`를 체크 표시해준다.
<br>

### 2. APN 인증키 업로드
![](images/Pasted%20image%2020240204191633.png)
우선 애플 개발자 계정으로 APN 인증키를 발급해야 한다.[apple account 홈페이지](https://developer.apple.com/account)
여기서 `Certificates, Identifiers & Profiles` -> `Keys`로 들어간다.
<br>

![](images/Pasted%20image%2020240204192004.png)
![](images/Pasted%20image%2020240204192037.png)
플러스 버튼을 누르고 key 이름을 입력한다. 밑에 `Apple Push Notifications service (APNs)`를 체크해준다.
<br>

![](images/Pasted%20image%2020240204192133.png)
다운로드를 누르면 APN키가 다운로드 된다. 다운 횟수가 1번으로 제한되어 있으니 지우면 안된다.
<br>
이 키를 파이어베이스에 업로드하면 된다. `Firebase` -> `Project settings`로 들어간다.
![](images/Pasted%20image%2020240204192547.png)
<br>
`cloud messaging`바에 들어간 뒤 `Upload`클릭
![](images/Pasted%20image%2020240204200812.png)
<br>

아까 다운로드 받았던 키 파일을 업로드하고, `Key ID`와 `Team ID`를 넣어야 한다. `Key ID`와 `Team ID`는 APN 인증키를 다운 받았던 페이지에서 확인할 수 있다.
![](images/Pasted%20image%2020240204201137.png)
<br>
**Key ID** 
![](images/Pasted%20image%2020240204204502.png)<br>

**Team ID**(우측 상단)
![](images/Pasted%20image%2020240204204655.png)
<br>

### 3. 라이브러리 설치
```dart
flutter pub add firebase_messaging
```
준비는 이걸로 끝이다.
<br>

### 토큰으로 push 알림 보내기
```dart
void setupNotifications() async {
	final fcm = FirebaseMessaging.instance;

	await fcm.requestPermission();

	final token = await fcm.getToken();
	print(token);
	/*토큰 출력 결과: cOshU1QrSOCvjGINe9LDA6:APA91bG2AE_p1V0xDkTu_bCw47nNCu4twvq73CJCyYXE80bPRzW3Wwv85QMohYrA0URes0YDsE9hWdYSZ2_pu80ksA38iaFLai-cEHXmB7dR2Aq_oMk875yQa2KT-gMXM1fa-3ot-EOr*/
}

@override
void initState() {
	super.initState();

	setupPushNotifications();
}
```
- `FirebaseMessaging.instance.requestPermission()`을 먼저 처리하고, `.getToken()`을 통해 토큰을 받아올 수 있다.
<br>

- 토큰은 해당 기기에 고유하게 부여되는 것으로, 이 토큰 정보를 백엔드에 저장해놨다가 알림을 발송할 때 사용한다.
<br>
`Messaging` -> `새 캠패인`버튼을 누르면 알림 제목과 알림 텍스트를 설정할 수 있다.
![](images/Pasted%20image%2020240205172047.png)<br>

`테스트 메시지 전송`버튼을 누르면 토큰을 추가할 수 있는데, 여기에 토큰을 붙여놓고 `테스트`를 누르면 알림을 전송할 수 있다.
![](images/Pasted%20image%2020240205172437.png)
- 특정 기기에만 알림을 보내고 싶다면 이 토큰을 백엔드에 저장해놨다가 위와 같은 방법으로 전송할 수 있다.

### 여러 기기로 보내기
![](images/Pasted%20image%2020240205173307.png)
`알림` 섹션에서 테스트 메세지를 보내지 않고 `타겟` 섹션으로 넘어오면 주제를 설정할 수 있는데, 예를 들어 `chat` 주제를 설정하고 검토를 누르면 `chat`이라는 주제를 갖고 있는 모든 기기들에 알림을 전송할 수 있다.
<br>

```dart
void setupPushNotifications() async {
	final fcm = FirebaseMessaging.instance;
	await fcm.requestPermission();
	
	//주제 설정
	fcm.subscribeToTopic('chat');

}
```
<br>

### 자동으로 push 알림 보내기
어떤 조건을 통해서 여러 기기에 자동으로 알림을 보낼려면 프런트코드만으론 부족하다. 그래서 firebase는 Firestore와 Real Time Database를 연동해 벡엔드 코드를 올리고 실행할 수 있는 Firebase Functions를 지원한다.
