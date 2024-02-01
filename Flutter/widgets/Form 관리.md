<br>

### Form 사용
```dart
final _formKey = GlobalKey<FormState>();

@override
Widget build(context) {
//...
	Form(
		key: _formKey
		child: //Form 구성 위젯
	),
```
- `Form`은 자동 State 관리, Invalid Error 메세지 설정에 용이한 위젯이다. `Form`을 구성하는 위젯을 감싸고, `key`를 설정해주면 된다.
<br>
- `_formKey`는 각 `Form`을 인식해 데이터를 저장, 관리 한다. `Form`안에 어떤 버튼의 `onPressed`함수를 통해 `_formKey.currentState.save()` `_formKey.currentState.reset()` 등을 실행할 수 있다.
<br>
<br>

### Form을 위한 위젯
기존에 입력값을 받던 위젯들에 Form이 붙은 위젯들이다. 아래 예시들을 포함해 다양한 종류가 있다.
<br>
#### TextFormField()
```dart
TextFormField(
	decoration: const InputDecoration(
		label: Text('Name'),
	),
	initialValue: '',
	validator: (value) {
		//사용자가 입력한 값 유효성 검사
		if(value == null || value.isEmpty) {
			return 'Error message';
		}
		return null;
	},
),
```
- **`TextFormField`** 는 `Form`안에서 쓸 수 있는 특수한 `TextField`이다. `Form`이 필드의 입력값을 저장, 변경할 수 있도록 해주며, `validator`를 통해 유효성도 검사할 수 있다.
<br>
- `validator`는 사용자가 입력한 `value`값을 인자로 받아 유효성 검사를 할 수 있는 속성이다. 유효하지 않은 조건을 설정해 에러 메세지를 반환할 수 있고, 그 경우 플러터가 텍스트 필드 밑에 자동으로 에러 메세지를 띄워준다.
<br>
#### DropdownButtonFormField
```dart
DropdownButtonFormField(
	value: _selectedCategory,
	items: [
		//아이템은 별도의 Form 위젯 없음
		for(final category in categories.entries/*:Map을 iterable로*/)
			DropdownMenuItem(
				value: category.value
				child: 
					//메뉴 아이템을 구성할 위젯
					Row(
						chlidren: [
							Container(
								width: 16, 
								height: 16, 
								color: category.value.color,
							),
							const SizedBox(width: 8),
							Text(category.value.title),
						],
					),
			),
	],
	onChanged: (value) {
		_selectedCategory = value;
	}
),
```
- `enum`을 활용한 카테고리를 드롭다운 버튼으로 구성하는 코드이다.  Map<Categories, Category>(하나는 enum, 하나는 title과 color로 이루어진 Category 객체)는 iterable이 아니기 때문에 for in 구문을 쓸 수 없지만, .entries를 사용하면 가능하다
	[enum을 iterable로 만들기](/Flutter/일반/배열%20선언%20final%20vs%20const.md)
<br>

- `value:`속성은 사용자가 버튼 하나를 골랐을 때 반환할 데이터를 정의하는 것으로, 여기선 `title`과 `color`를 들고 있는 `Category`(Map에서 값에 해당)를 반환하게끔 작성했다.
<br>

- `onChanged:`속성은 사용자가 드롭다운 버튼을 조작해 변화가 생겼을 때 실행되는 함수이다. 지금은 현재 선택된 `Category`객체를 저장하는 지역변수 `_selectedCategory`에 선택된 `value`를 저장하는 코드가 작성되었다.
<br>

## 전체 Form 데이터 저장 & 다른 페이지로 전송하기
```dart
class _NewItemState extends State<NewItem> {
	final _formKey = GlobalKey<FormState>();
	var _enteredName = '';
	var _enteredQuantity = 1;
	var _selectedCategory = categories[Categories.vegetables]!;

	@override
	Widget build(context) {
		return Form(
			key: _formKey,
			child: //Form 구성 객체
		);
	}
}
```
- 우선 **`Form`** 내부의 `TextFormField`나 `DropdownButtonFormField`와 같은 위젯들에서 받아오는 입력값들을 저장할 지역변수를 만든다. 
<br>

해당 지역변수는 초기값으로 설정하여 각 `Field`에서 제일 처음 아무것도 입력하지 않았을 때 이 초기값을 기본값으로 설정하도록 할 수도 있다.
```dart
//...
TextFormField(
	initialValue: _enteredName,
	//...
	onSaved: (value) {
		_enteredName = value!;
	}
),

//...
TextFormField(
	initialValue: _enteredQuantity.toString(),
	//...
	onSaved: (value) {
		_enteredQuantity = value!;
	}
),

//...
DropdownButtonFormField(
	value: _selectedCategory,
	//...
	onChanged: (value) {
		_selectedCategory = value;
	}
),
```
- 각 `FormField`는 어떤 버튼으로 인해서 `_formKey.currentState!.save()`함수가 호출되었을 때 호출되는 `onSaved:` 속성을 가지고 있다. 사용자의 입력값이 시시각각 변하기 때문에 그때마다 지역변수에 저장하는 것이 아니라 사용자가 다 작성하고 제출 버튼을 눌렀을 때 `_formKey.currentState!.save()`를 호출하고 이 함수가 호출되었을 때 비로소 지역변수에 입력값을 저장하는 것이다.
<br>

- 드롭다운 버튼의 경우 사용자의 입력이 계속 변하는 것이 아니기 때문에(정해진 항목에서 고르니까) 다른 항목을 고를 때마다 지역변수에 저장한다. (`onChanged:` 속성)
<br>

이제 제출 버튼을 만들어 `_formKey.currentState!.save()`를 호출해보자
```dart
class _NewItemState extends State<NewItem> {
	final _formKey = GlobalKey<FormState>();
	var _enteredName = '';
	var _enteredQuantity = 1;
	var _selectedCategory = categories[Categories.vegetables]!;
	
	void _saveItem() {
		if(_formKey.currentState!.validate()) {
			_formKey.currentState!.save();
			Navigator.of(context).pop(GroceryItem(
				id: DateTime.now().toString(),
				name: _enteredName,
				quantity: _enteredQuantity,
				category: _selectedCategory!,
			));
		}
	}
	
	@override
	Widget build(context) {
		return Form(
			//...
			Row(children: [
				//취소 버튼
				TextButton(
					onPressed: () {
						_formKey.currentState!.reset();
					},
					child: const Text('reset'),
				),
				
				//제출 버튼
				ElevatedButton(
					onPressed: _saveItem,
					child: const Text('Add Item'),
				),
			]),
		);
	}
}
```
- 앞서 각 `FormField`에 `validator:`속성을 이용해 유효성 조건을 설정해 놓았던 것이 여기에서 쓰인다. `_formKey.currentState!.validate()`함수는 모든 유효성 조건을 검사하고 만약 하나라도 유효성 조건을 만족하지 못하면 `false`를 반환한다. 이때 에러 메세지는 앞서 설명한 것처럼 각 `FormField`에서 설정한 에러 메세지가 실행된다.
<br>

- 모든 필드에서 유효성 조건을 만족하면 `_formKey.currentState!.save()`가 실행된다. 이때 각 `FormField`의 `onSaved:` 에 설정한 함수가 실행되면서 지역변수에 입력값이 저장되는 것이다. 
<br>

- 이후 `Navigator.of(context).pop()`을 통해 앞의 페이지로 `.pop()`안의 데이터를 넘겨준다.
[Navigator를 통한 페이지 간 데이터 전달](/Flutter/widgets/화면%20이동시%20데이터%20전달(PopScreen).md)