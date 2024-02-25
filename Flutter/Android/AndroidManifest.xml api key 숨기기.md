#### local.properties
```properties
kakao_api_key=12341234
```
- 이름은 맘대로(kakao_api_key로 하던 kakao.api.key로 하던)정하고 **따옴표 없이** 키 값 넣기
- (local.properties는 반드시 `.gitignore` 파일에 포함할 것!)
#### android/app/build.gradle
```java
def flutterVersionName = localProperties.getProperty//중략

//flutterVersionName 정의 된 곳 밑에 아래 코드 삽입
def kakaoApiKey = localProperties.getProperty("kakao_api_key")
if (kakaoApiKey == null) {
	throw new GradleException("kakao Api Key not found.")
}
```
- `kakaoApiKey` 정의 코드 삽입
<br>

```java
android {
	buildTypes {
		debug {
			manifestPlaceholders = [kakaoApiKey: kakaoApiKey]
		}
		release {
			// 중략
			signingConfig signingConfigs.debug
			manifestPlaceholders = [kakaoApiKey: kakaoApiKey]
		}
	}
}
```
- 같은 파일에서 밑에 내려가다 보면 **android -> buildTypes** 가 있는데 이 안에 `debug{}`와 `release{}` 안에 