```dart
PopScope(
	canPop: false,
	onPopInvoked: (didPop) {
		if(didPop) return;
		Navigator.of(context).pop(/*전달할 데이터*/);
	}
	child: //해당 화면 위젯
)
```

```dart
final result = await Navigator.of(context).push(MaterialPageRoute((ctx) => /*변경할 페이지*/))
```

- Navigator.pop()에 데이터를 넣으면 해당 페이지로 이동시켰던 Navigator.push()가 데이터를 비동기적으로 받을 수 있음

>canPop: pop 요청이 들어오면 일단 그걸 막음(false)
>onPopInvoked: pop 요청이 들어오면 실행. 안에서 pop이 됐을 때, 안됐을 때 함수 설정 가능
**위 코드는 일단 pop을 막고 Navigator.of(context).pop()으로 대체하는 코드**

<br>

### 아이폰 BackSwipe 살리기
```dart
body: PopScope(
	canPop: true,
	onPopInvoked: (didPop) async {
		if (didPop) {
			ref.read(filtersProvider.notifier).setFilters({
				Filter.glutenFree: _glutenFreeFilterSet,
				Filter.lactoseFree: _lactoseFreeFilterSet,
				Filter.vegetarian: _vegetarianFilterSet,
				Filter.vegan: _veganFilterSet,
				});
		}
	},
```
**Navigator.pop()** 으로 데이터를 전달하는게 아니라 앱 전역 state를 사용하면 유저가 <mark>실제로</mark> 뒤로 갔을 때 데이터를 변경할 수 있다.

[앱 전체 state 접근, 변경하기(riverPod)](/Flutter/외부%20라이브러리/앱%20전체%20state%20접근,%20변경하기(riverPod).md)