```dart
flutter pub add flutter_riverpod
```
## Provider로 App 감싸기
```dart
runApp(
	const ProviderScope(child: App()),
);
```
ProviderScope로 감싼 위젯 안에 들어있는 모든 위젯은 Provider에 접근할 수 있게 된다.
<br>

## Provider 만들기

일단 provider 폴더 하나 만들고, `favorite(하고 싶은 이름)_provider.dart` 파일 하나 만든다.  
안에 만들 Provider는 용도에 따라 크게 3가지로 볼 수 있다.

#### 정적 데이터만 저장
```dart
final favoriteProvider = Provider((ref) => [1, 2, 3]);
```
- 이렇게 하면 그냥 데이터 읽어오는 것 밖엔 못한다. 그냥 더미 데이터 읽는 용도로나 쓸 수 있는데, 그래서 굳이? Provider를 쓸 필요가 있나 싶다

#### 상태 변경 가능
```dart
final favoriteProvider = StateProvider<int>((ref) {
	return 0;
});
```
- 사용하는 위젯에서 `state`를 통해 값을 변경할 수 있다는데, 써보진 않았다.

#### 상태 변경 & 로직
```dart
enum Filter {
	a,
	b,
	c,
	d,
}

class FavoriteNotifier extends StateNotifier<Map<Filter, bool>> {
	FavoriteNotifier() : super({
		Filter.a : false,
		Filter.b : false,
		Filter.c : true,
		Filter.d : false,
	});

	void setFilters(Map<Filter, bool> choosenFilters) {
		state = choosenFilters;
	}
}


final favoriteProvider = StateNotifierProvider<FavoriteNotifier, Map<Filter, bool>>((ref) => FavoriteNotifier());
```
- `Map<Filter, bool>`형태의 데이터를 저장하는 Provider를 만든 것이다. 
	별도의 `FavoriteNotifier` 클래스를 만들어 초기값, 데이터를 변경할 때 쓸 로직들을 저장한 후 `StateNotifier`를 통해 `favoriteProvider`에 넣어준다.
- 상태를 읽어오고 변경할 로직까지 저장할 수 있어서 주로 쓰는 방법이다.
<br>
<br>
## 위젯에서 사용하기

Stateless, Statefull 위젯에 따라서 사용 방식이 다르다.

#### StatelessWidget
```dart
class someWidget extends ConsumerWidget {
	//...
	
	@override
	Widget build(context, WidgetRef ref) {
		final data = ref.watch(favoriteProvider);
	}
}
```
- <mark>WidgetRef ref</mark>를 파라미터로 받고, <mark>ref.watch</mark> 안에 아까 만든 Provider를 넣어주면 된다.
	그럼 data에 현재 데이터가 담기고, 위젯에서 쓸 수 있다.

>**StatelessWidget** 을 **ConsumerWidget** 으로 바꿔야 함!

<br>

#### StatefulWidget
```dart
class someWidget extends ConsumerStateWidget {
	//...

	@override
	ConsumerState<someWidget> createState() => _someWidgetState();
}

class _someWidgetState extends ConsumerState<someWidget> {
	@override
	Widget build(context) {
		final data = ref.watch(favoriteProvider);
	}
}
```
- 얘도 마찬가지로 **ConsumerStateWidget**으로 바꿔주고 **ref.watch()** 를 통해서 접근하면 된다. 특이한 점은 별도로 **WidgetRef** 가 필요하진 않다.
<br>
```dart
ref.read(favoriteProvider.notifier).setFilters({/*...*/});
```
- 아까 지정해놨던 데이터 변경용 함수를 쓰고 싶으면 notifier를 호출한 후 미리 정의된 함수 `setFilters`를 사용하면 된다.

<br>

## 여러 Provider 연결
```dart
final filterProvider = Provider((ref) => {
	
})
```