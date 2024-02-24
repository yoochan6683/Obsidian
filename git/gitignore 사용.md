#### .gitignore
git에서 추적하지 말아야 할 파일들을 정의하는 곳
<br>
#### .gitignore 사용법
- `#`은 주석
- 표준 glob 패턴을 따름
- 디렉토리는 끝에 슬래시 `/`를 사용해 표현
- 느낌표`!`로 시작하는 경우는 예외로 처리
#### 예시
```dart
//파일 하나만 무시
filName.txt

//어떤 디렉토리의 어떤 파일
fileDirectory/fileName.txt

//어떤 디렉토리 전부
fileDirectory/

//특정 확장자 모두
*.txt

//현재 경로의 이 파일
/fileName.txt

//어떤 경로 안에 있는 모든 fileName 무시
fileDirectory/**/fileName.txt

//예외
!fileName.txt
```
저장하고 커밋, 푸시하면 자동적용됨
#### .gitignore 적용이 안될 때
- 깃의 캐시가 원인 -> git에 있는 캐시 파일을 지워주고 다시 add 해주면 됨
- 끝에 한칸 띄우고 점 찍어야 함
```
git rm -r --cached .
git add .
git commit -m "removed cached"
```