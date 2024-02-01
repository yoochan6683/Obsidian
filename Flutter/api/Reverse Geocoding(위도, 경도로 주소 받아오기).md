```dart
import 'package:http/http.dart' as http;
```
네트워크를 통해 받아오므로 http 패키지가 필요하다.
<br>
### 사용 방법
```dart
final url = Uri.parse('https://maps.googleapis.com/maps/api/geocode/json?latlng=40.714224,-73.961452&key=Your Key Here');
final response = await http.get(url);
```
해당 url에 위도와 경도를 넣고 google API 키를 넣으면 주소가 담긴 `response`를 받을 수 있다.
<br>
`response` 는 아래와 같이 온다.
```dart
{
	"results" : [
		"address_components" : [
			{
				//...
			}
			//...
		 ],
		 //내가 원하는 주소!
		"formatted_address" : "277 Bedford Avenue, Brooklyn, NY 11211, USA"
	]
	//...
}
```
전체 주소를 얻고 싶을 때는 저 `formatted_address`를 가져오면 된다. 부분 주소를 가져올 수도 있는데, `response`의 구조를 잘 보고 접근하면 된다.
[역 지오코딩 설명 링크](https://developers.google.com/maps/documentation/geocoding/requests-reverse-geocoding)
<br>
역 지오코딩 사용 함수를 만들면 아래와 같이 만들 수 있다.
```dart
Future<String> getFullAddress(double latitude, double longitude) async {
	//latitude값, longitude값, API 키 url에 포함
	final url = Uri.parse('https://maps.googleapis.com/maps/api/geocode/json?latlng=$latitude,$longitude$&key=google API Key');

	//네트워크 요청
	final response = await http.get(url);
	//json 디코드
	final addressData = await json.decode(response.body);
	//reponse에서 전체 주소를 갖고 있는 부분 추출
	final address = addressData['results'][0]['formatted_address'];

	return address;
}
```
<br>

### 참고
`Future<String>`을 반환하도록 했으므로 `String`에 저장하려면 받는 부분도 `await`으로 받아야 함.
```dart
String? currentAddress;

void getAddress(double lat, double lng) async {
	currentAddress = await getFullAddress(lat, lng);
}
```