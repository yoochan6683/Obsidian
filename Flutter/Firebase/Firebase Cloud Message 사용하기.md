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

