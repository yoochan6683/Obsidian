[사용준비](사용준비.md)
```dart
void _addItem() async {
	final url = Uri.https('text-da726-default-rtdb.firebaseio.com', 'shopping-list.json');
	final response = await http.post(
						url, 
						header: {'Content-Type' : 'application/json'}, 
						body: json.encode({
							'name': 'Apples', 
							'quantity': 24, 
							'category': 'Fruit'}),
						);
}
```
- `url`, `header`, `body` 를 넣어 `http.post`함수를 통해 새로운 아이템을 저장할 수 있다.
- `json`형식으로 저장해야 하므로 `json.encode()`를 통해 데이터를 `json`으로 바꿔줘야 한다.
<br>

이때 Firebase에서 id를 자동으로 생성해 주는데, response에서 해당 id를 받을 수 있다.
![](/images/Pasted%20image%2020240117160033.png)<br>

```dart
final Map<String, dynamic> decodedData = await json.decode(response.body);
						// decodedData => {'name' : '-NoGr92NT72NK1fqGL8H'}
```
<br>

Map 데이터로 변환해주므로 접근은 이렇게 하면 된다.

```dart
decodedData['name'];
```