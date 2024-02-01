```c
void setup() {
  Serial.begin(9600);
}

void loop() {
  int sum, num1, num2;

  Serial.println("Enter 2 Integers to add");

  while (Serial.available() == 0) {
   
  }

  num1 = Serial.parseInt();

  while (Serial.available() == 0) {

  }

  num2 = Serial.parseInt();

  sum = num1 + num2;
  
  Serial.print(num1 + " + " + num2 + " = " + sum);

  Serial.println();

}
```

>[Serial.available()](/aduino/functions/Serial.available().md) 이 0이면 while 안에서 계속 돌고(입력 기다림), 다음 입력값이 있으면 while이 깨지면서 값을 저장함

- No Line Ending 상태에서 해야 함