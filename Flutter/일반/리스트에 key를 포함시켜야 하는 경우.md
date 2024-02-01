![[Pasted image 20240108170138.png]]
<mark>State</mark>는 Element Tree에 붙어 있기 때문에 Widget의 순서가 바뀌어도 Element Tree가 바뀌지 않았으므로 ㅏkey를 붙여주지 않으면 state가 서로 바뀌는 결과를 낳게 된다.

예를 들어 화면에서 사용자가 아이템 순서를 바꿀 수 있도록 만들었다고 가정하자. 사용자가 TodoItem('A')와 TodoItem('B')의 Widget 순서를 바꿨을 때, 내부적으로 key가 없으면 Widget 순서만 바뀌고 state는 안 바뀌어 A가 B의 state를, B가 A의 state를 가지게 된다.

투두 리스트에서 A에 체크를 하고 B에 체크를 안한 상태에서 사용자가 투두리스트 순서를 바꾸면 B가 체크되고 A가 체크가 풀리는 것이다.

때문에 리스트를 나열할 때 상태가 변하는 성질의 Widget 이라면 key 속성을 부여해 각각의 아이템이 자기 state를 가질 수 있도록 해주어야 한다.

```dart
Column(
children: [
	for(final todo in todos) {
		Todo(
			//ValueKey로 키 생성, key 속성에 부여만 해주면 끝
			key: ValueKey(todo.text),//todo.text와 같은 고유 값으로 설정해야 함.
			todo.text,
			todo.property,
		);
	}
],
),
```