```dart
enum Categories = {
	vegetables,
	fruit,
	meat,
	dairy
}

const Category {
	const Category(this.title, this.color);

	final String title;
	final Color color;
}

const categories = {
	Categories.vegetables: Category(
		'Vegetables',
		Colors.green,
	),
	Categories.fruit: Category(
		'Fruit',
		Colors.orange,
	),
	Categories.meat: Category(
		'Meat',
		Colors.red,
	),
	Categories.dairy: Category(
		'Dairy',
		Colors.white,
	),
};
```
- 만들고 싶은 카테고리를 `enum`으로 정의하고, 해당 카테고리에 필요한 `title`과 `color`를 **`Category`** 객체로 정의했다. 이후 둘을 묶어 **`Map<Categories, Category>`** 형태의 객체 `categoreis`를 만들었다.
<br>
## Map 데이터 for 문 안에 넣기
위와 같은 `categories`를 이용해 리스트를 만들고 싶다면 문제가 생긴다. `Map`은 iterable하지 않기 때문에 for 문으로 리스트를 만들 수 없다. 
<br>
예를 들어 `categories`를 이용해 드롭다운 버튼을 만들어 보자.
```dart
DropdownButtonFormField(
	items: [
		for(final category in categories) //문제 발생!!!
			DropdownMenuItem()
	]
),
```
<br>

`categories`안에 있는 모든 카테고리 종류마다 연결된 `title`과 `color`로 드롭다운 버튼을 만들고 싶다.
![[Pasted image 20240115133052.png]]
<br>
그럴 때 `Map`을 iterable하게 만들어주는 함수가 있다. `.entries`이다.
```dart
	for(final category in categories.entries)
		DropdownMenuItem(
			value: category.value,//유저가 버튼을 선택했을 때 반환한 데이터
			child: Row( children: [
				Container(
					width: 16,
					height: 16,
					color: category.value.color//color 접근
				),
				const SizedBox(width: 8),
				Text(category.value.title)//title 접근
			]),
		),
```
- `Map`을 담고 있는 객체에 `.entries`함수를 호출하면 된다. 그럼 Map 안의 각각의 데이터를 `category`로 호출할 수 있다.
<br>
- 이때 `category`안에는 두 객체가 담긴다. **`Map`** 의 `key`와 `value`이다.  위의 예시에서는 **`Map`** 이 `Map<Categories, Category>`이므로, `key`에는 `Categories.vegetables`, `Categories.fruit` 등이 담기게 되고, `value`에는 `Category(title, color)` 가 담기게 된다.
<br>
- 접근은 간단하게 위의 예시처럼 `category.key`, `category.value.title`, `category.value.color`와 같이 접근하면 된다.