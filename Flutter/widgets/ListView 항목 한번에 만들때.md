
### 리스트 자동 생성
```dart
ListView.builder(
	//필수! list 길이를 넘겨줘야 함
	itemcount: meals.length,
	//index로 meal 한개씩 넘겨서 리스트로 만듦
	itembuilder: (ctx, index) => MealItem(meals[index]));
);
```