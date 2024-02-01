```dart
flutter pub add google_fonts //터미널에서 설치
```

```dart
import 'package:google_fonts/google_fonts.dart';

final theme = ThemeData(

	useMaterial3: true,
	colorScheme: ColorScheme.fromSeed(
		brightness: Brightness.dark,
		seedColor: const Color.fromARGB(255, 131, 57, 0),
	),
	//Google 폰트 설정
	textTheme: GoogleFonts.latoTextTheme(),

);
```