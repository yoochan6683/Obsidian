#### 폰트 다운
{프로젝트 디렉토리}/assets/fonts 파일을 만들어 그 안에 필요한 만큼 다운받은 폰트를 넣어준다.
![](images/Pasted%20image%2020240224194126.png)
#### pubspec.yaml
```dart
fonts:
	- family: GmarketSans
	  fonts:
		- asset: assets/fonts/GmarketSansTTFBold.ttf
		- asset: assets/fonts/GmarketSansTTFLight.ttf
		- asset: assets/fonts/GmarketSansTTFMedium.ttf
```
파일 아래 예시에 따라 폰트명과 해당 폰트 파일들의 경로를 지정해준다.

#### main.dart
```dart
@override
Widget build(BuildContext context) {
	return MaterialApp(
		title: 'Flutter Demo',
		theme: ThemeData(
		colorScheme: ColorScheme.fromSeed(seedColor: Colors.pinkAccent),
		fontFamily: 'GmarketSans',
		textTheme: const TextTheme(
		titleLarge: TextStyle(
		fontSize: 30,
		fontWeight: FontWeight.w600,
		),
	),
	useMaterial3: true,
),
```
`MaterialApp`에서 `theme:ThemeData()` 안에 `fontFamily:`속성에 아까 지정한 폰트명을 넣으면 된다.