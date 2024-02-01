### 애니메이션 종류
애니메이션에는 Explicit, Implicit 두가지의 애니메이션이 있다. Explicit은 처음부터 끝까지 세세하게 내가 설정하는 애니메이션이고, Implicit은 만들어진 애니메이션을 쓰는거라고 보면 된다.
[[애니메이션 넣기(Implicit)|Implicit 애니메이션 넣는법]] 
<br>

### 사용
애니메이션은 당연히 **StatefulWidget** 에서만 가능하다.
```dart
class _someWidgetState extends State<someWidget>
	with SingleTickerProviderStateMixin {
	late AnimationController _animationController;

	@override
	void initState() {
		super.initState();
		_animationController = AnimationController(
			vsync: this,
			duration: const Duration(millisecond: 300),
			upperBound: 1,
			lowerBound: 0
		);

		_animationController.forward();
	}

	@override
	void dispose() {
		super.dispose();
		_animationController.dispose();
	}
}

```
- 우선 State 위젯이 **`TickerProviderStateMixin`** 을 상속받아야 한다. **`with`** 는 두 가지 클래스를 같이 상속받을 수 있게 하는 명령어다.
<br>

- 그리고 애니메이션 컨트롤러가 필요한데, 컨트롤러를 하나만 설정할거면 **`SingleTickerProviderStateMixin`** 을 더 권장한다고 한다.  
<br>

- 컨트롤러는 **`AnimationController`** 를 이용해 선언하는데, 이때 **`late`** 로 선언해야 한다. 애니메이션 컨트롤러는 initState로 해당 페이지가 만들어질때 값을 넣어줘야 한다. 아직 값을 넣어줄 수 없지만 나중에 값이 들어올거라는 뜻으로 **`late`** 를 쓴다.
<br>

- **`vsync: this`** 는 이 페이지가 TickerProviderStateMixin이 제공하는 애니메이션 로직들을 받을 수 있도록 한다. 정확한 기능은 잘 모른다.
<br>

- **`duration`** 은 애니메이션이 동작할 시간을 설정하고, **`upperBound`** **`lowerBound`** 는 나중에 설정할 애니메이션 값을 최저값과 최고값을 의미한다. 이건 0부터 20, 5부터 17 등 다양하게 설정할 수 있다. 밑에서 애니메이션 설정할 때 나온다.
<br>

- **컨트롤러를 설정했으면 `dispose`도 설정해주어야 한다.** 이건 페이지가 스택에서 사라질때 컨트롤러도 메모리에서 삭제해 메모리 오버플로우가 나지 않도록 해준다.
<br>

- **컨트롤러.forward()를 해야 애니메이션이 시작된다.**

<br>

```dart
@override
Widget build(context) {
	return AnimatedBuilder(
		animation: _animationController,
		child: /*원래 페이지를 구성하던 위젯*/,
		builder: (context, child) => Padding(
			padding: const EdgeInsets.only(
				top: 100 - _animationController.value * 100,
			),
			child: child,
		);
		
	);
}
```
- 이제 페이지 위젯에 **`AnimatedBuilder`** 를 씌워주면 된다.
<br>

- **`animation:`** 속성에 아까 만들었던 컨트롤러를 불여주고 **`child:`** 에 원래 페이지를 구성하던 위젯을 넣어주면 된다.
<br>

- **`builder:`** 는 애니메이션 효과를 넣을 위젯을 넣어주면 된다. **`builder`** 안에 들어가는 위젯은 애니메이션이 실행될때마다 새로 만들어진다. 그래서 원래 페이지 위젯을 여기다가 넣어도 되지만, 그렇게 하면 **매번 페이지의 모든 위젯을 새로 만들어야 하므로** 성능에 좋지 않다.
<br>

- **`_animationController.value`** 가 아까 설정한 **`upperBound`** **`lowerBound`** 의 사이값이다. 애니메이션이 **`duration`** 동안 실행되면서 **`_animationController.value`** 가 **`lowerBound`** 부터 **`upperBound`** 까지 값이 변화한다.
<br>

- 예를 들어 0과 1로 설정했으면 **`_animationController.value`** 가 0부터 1까지 애니메이션이 실행되는 동안 값이 천천히 변한다. **`Padding`** 위젯의 padding 값을 `top: 100 - _animationcontroller.value * 100` 으로 설정했으므로, padding 전체값은 애니메이션을 시작할 때는 100, 끝날때는 0의 값을 가진다. 결과적으로 페이지 전체가 아래에서 위로 올라오는 효과를 줄 수 있는 것이다.

<br>

### 화면 비율 이용하기
위의 방법은 크기를 직접 지정해준다는 점에서 한계가 있다. 화면크기가 기종마다 다르니까 동일한 크기로 애니메이션을 지정하면 똑같은 느낌을 주기 힘들다.
<br>
```dart
	//...
	builder: (context, child) => SlideTransition(
		position: _animationController.drive(
			Tween(
				begin: const OffSet(0, 0.3),
				end: const OffSet(0, 0),
			),
		),
	child: child,
	),
```
- **`Tween`** 은 시작값부터 끝값까지 **`drive`** 에 넘겨줘서 컨트롤러가 해당 값에 맞게 움직일 수 있도록 해준다. **`OffSet`** 은 x좌표, y좌표를 상대값을 받아 변환해준다. `OffSet(0, 0.3)` 이면 x는 제자리(왼쪽 끝) y는 위에서부터 30퍼센트 지점을 의미한다. 
<br>

- 이렇게 코드를 작성하면 시작점이 위에서부터 30퍼센트 지점(페이지 전체를 아래로 30퍼센트 내림), 끝값이 제자리이므로 페이지 전체가 아래에서부터 위로 올라온다.

<br>

### 애니메이션 효과
애니메이션 범위와 시간은 정했지만, 위의 방법만으로는 세부적인 애니메이션 설정이 불가하다. 
빠르게 올라왔다가 천천히 마무리하는것과 같은 세부 애니메이션 효과는 다음과 같이 설정할 수 있다.
<br>

```dart
	//...
	builder: (context, child) => SlideTransition(
		position: Tween(
			begin: const OffSet(0, 0.3),
			end: const OffSet(0, 0),
		).animate(CurvedAnimation(
					parent: _animationController, curve: Curves.decelerate)),
		child: child,		
	),
```
- **`Tween().animate()`** 를 활용하면 된다. 안에 효과를 설정할 애니메이션 객체(여러가지를 넣을 수 있다.) 그 중 **`CurvedAnimation`** 을 넣었다. `parent:` 에 컨트롤러를 넣고, `curve:` 속성에 효과를 넣으면 된다. 
<br>

- `Curves.` 를 사용하면 미리 만들어진 여러 효과들을 사용할 수 있다.