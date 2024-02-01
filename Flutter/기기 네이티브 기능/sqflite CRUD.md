### Create
```dart
/* 
	var <생성후 ID값> = 
		await db.insert( <테이블명> , <저장시킬 데이터 (Map<String,Object> 타입)> );
*/

// 방식 1
var id = await db.insert(
				'user', 
				{'name': '홍길동', 'age': 22, 'nickname': '신출귀몰'},
				);

// 방식 2
var rawId = await db.rawInsert(
      'INSERT INTO user(name, age, nickname) VALUES("홍길동", 22, "신출귀몰")'
);

// 방식 3
var rawId1 = await db.rawInsert(
      'INSERT INTO user(name, age, nickname) VALUES(?,?,?)',
      ["홍길동", 22, "신출귀몰"]
);
```
<br>

### Read
```dart
/* 
	var <조회된 데이터> = 
		await db.query(<테이블명> , 
					where : <조건식> , 
					whereArgs : <조건식에 비교할 값(Array)>);
*/

// 방식 1
var user = await db.query('user', where: 'name = ?', whereArgs: ['홍길동']);

// 방식 2
var rawUser = await db.rawQuery('SELECT * FROM user WHERE name="홍길동"');

// 방식 3
var rawUser1 = await db.rawQuery('SELECT * FROM user WHERE name=?', ['홍길동']);
```
<br>

### Update
```dart
// 방식 1(만약 새로운 속성 '키'를 추가할 시)
var id = await db.update(
    'user',
    {'키': 180},
    where: 'name = ?',
    whereArgs: ['홍길동'],
);

// 방식 2(홍길동의 이름, 나이 -> GOAT 길동, 34로 변경)
var updateID = await db.rawUpdate(
      'UPDATE user SET name = "GOAT 길동", age = 34 WHERE name = "홍길동"'
);

// 방식 3
var updateID = await db.rawUpdate(
      'UPDATE user SET name = ?, age = ? WHERE name = ?', 
      ['GOAT 길동', 34, '홍길동']
);
```
<br>

### Delete
```dart
// 방식 1
var isDeleted = await db.delete('user', where: 'name = ?', whereArgs: ['홍길동']);

// 방식 2
var isDeleted1 = await db.rawDelete('DELETE FROM user WHERE name = "홍길동"');

// 방식 3
var isDeleted2 = await db.rawDelete('DELETE FROM user WHERE name = ?', 
									['홍길동']);
```