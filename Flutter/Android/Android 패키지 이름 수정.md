앱 패키지 명을 수정하려면 4가지 요소를 수정해야 한다.

- {프로젝트 디렉토리}/android/app/build.gradle 파일
- {프로젝트 디렉토리}/android/app/src/main/AndroidManifest.xml 파일
- {프로젝트 디렉토리}/android/app/src/main/kotlin/이하 폴더 디렉토리 이름
- {프로젝트 디렉토리}/android/app/src/main/kotlin/이하 폴더에 존재하는 `MainActivity.kt`파일
#### build.gradle
- **{project}/android/app/build.gradle**의 `applicationId`항목을 수정
```java
android {
	defaultConfig {
		applicationId "{내가 원하는 패키지 이름}"
		//...
	}
}
```
#### AndroidManifest.xml
- **{project}/android/app/src/main/AndroidManifest.xml**의 `package`항목 수정
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	package="{내가 원하는 패키지 이름}">
	<!-- 중략 -->
</manifest>
```

#### 디렉토리 이름 수정
- **{project}/android/app/src/main/kotlin/** 경로로 이동하면 아래와 같은 디렉토리 구조가 있다.
>{project}/android/app/src/main/kotlin/com/example/{프로젝트 이름}

