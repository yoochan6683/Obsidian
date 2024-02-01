<br>

직접 다 설정하는 방식(Explicit)과 달리 미리 만들어진 애니메이션을 쓰면 쓰는 방식이 훨씬 간단하다.
[애니메이션 직접 설정하는 방법](/Flutter/widgets/애니메이션%20넣기(Explicit).md)

<br>

```dart
icon: AnimatedSwitcher(
	duration: const Duration(millisecond: 300),
	transitionBuilder: (child, animation) {
		return ScaleTransition(
			scale: Tween(begin: 0.5, end: 1.0).animate(animation),
			child: child,
		);
	}
	child: Icon(isFavorite ? Icons.star : Icons.star_border, 
				key: ValueKey(isFavorite)),
),
```
- Widget 객체를 반환하는 곳이면 **`AnimatedSwitcher`** 을 사용해 애니메이션을 넣을 수 있다. **`AnimatedSwitcher`** 는 `child`에 정의된 위젯의 변화를 감지해 애니메이션을 실행한다.
- `transditionBuilder` 안에는 여러 Transition을 설정할 수 있다. `RotateTransition`은 `child`를 회전시키고, `ScaleTransition`은 확대하는 애니메이션을 주는 등 상황에 맞는 Transition을 넣으면 된다.
- **`AnimatedSwitcher`** 의 `child`에 위젯이 `transitionBuilder`의 `child`로 넘어와 `ScaleTransition`의 `child`로 넘어오는 다소 복잡한 구조를 띄고 있다.
- <mark>중요!</mark> `key` 값을 설정하지 않으면 `AnimatedSwitcher` 입장에서 `Icon` 인채로 변하지 않은 것으로 판단하기 때문에 다른 `key` 값을 부여해야 한다. 
<br>

[플러터 Implicit Widget](https://docs.flutter.dev/ui/animations/implicit-animations)