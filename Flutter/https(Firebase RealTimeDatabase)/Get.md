[[사용준비]]

```dart
void _loadItems() async {
	final url = Uri.https('text-da726-default-rtdb.firebaseio.com', 'shopping-list.json');
	final response = await http.get(url);
	final Map<String, dynamic> decodedData = json.decode(response.body);
	print(decodedData);
}
```
<br>

#### 결과
```dart
flutter: {
	-NoGr92NT72NK1fqGl8H: {category: Fruit, name: Apples, quantity: 24}, 
	-NoKy44xluHep3PXKMh5: {category: Vegetables, name: Abbles, quantity: 2}, 
	-NoKy6T4AFmien9AdeDT: {category: Carbs, name: Accles, quantity: 3}, 
	-NoKyHmrBit8WfmuL_Sf: {category: Fruit, name: banana, quantity: 4},
}
```
- 통신이므로 `async await` 비동기처리를 해주어야 한다.
- 파이어베이스에는 데이터가 `json`형태로 저장되어 있다. 따라서 `http.get()`메서드로 데이터를 받아온 후 `json.decode(response.body)`로 decode하면 데이터를 받을 수 있다.
<br>

- 이때 `Map<String, dynamic>`으로 변환되기 때문에, 이걸로 리스트를 만들려면 Map을 for 문 안에 넣어서 만들면 된다.
	[[enum을 iterable로 만들기|Map 데이터 for문 안에 넣기]]
