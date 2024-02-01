[사용준비](/Flutter/https(Firebase%20RealTimeDatabse)/사용준비.md)
```dart
void _deleteItem(GroceryItem item) async {
	final url = Uri.https('text-da726-default-rtdb.firebaseio.com', 'shopping-list/${item.id}.json');
	final response = await http.delete(url);
}
```
- 간단하게 url에 해당 아이템의 `id`를 붙이고 `http.delete()`를 통해서 삭제하면 된다.
