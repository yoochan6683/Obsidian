_*주의: 구글링이 아니라 혼자 파일을 뜯어보다가 발견한 방법이라 보안이 제대로 안될 수 있음_
<br>

앱을 개발하는 과정에서 권한 설정으로 인해 `Info.plist` 파일을 수정할때가 많은데, 문제는 `Info.plist` 파일을 깃 허브로 추적하자니 안에 api key가 있어서 키가 노출될까봐 추적할 수가 없다.

다른 파일에 민감한 키 정보를 넣어놓고 그 파일은 `.gitignore` 에 추가,  `Info.plist` 파일엔 추상화 된 키 이름을 입력하는 방식을 찾았다.
#### 값을 추상화하는 방법
{프로젝트 디렉토리} -> iOS -> Flutter에 들어가보면 `Debug.xcconfig`, `Generated.xcconfig`, `Release.xcconfig`파일을 발견할 수 있다.

자세히 보면 `Info.plist`의 추상화 된 값들이 여기에 정의된 것을 볼 수 있다.

예시:
`Info.plist`에 정의된 추상화 된 값
![](/images/Pasted%20image%2020240226102733.png)

`Generated.xcconfig` 파일
![](/images/Pasted%20image%2020240226102838.png)

그래서 마찬가지 방법으로 api key도 똑같이 처리했다.

#### Debug.xcconfig
위에서 설명한 경로의 `Debug.xcconfig` 파일안에 api key를 적는다. `Debug.xcconfig`가 `.gitignore`에 포함되어 있는 꼭 확인한다.
```xcconfig
KAKAO_API_KEY=12341234
```

#### Info.plist
$()을 이용해 호출한다.
```xml
<string>kakao$(KAKAO_API_KEY)</string>
```
