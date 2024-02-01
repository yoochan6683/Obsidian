```cpp
int leds[] = {2, 3, 4, 5};
int pattern = 1;
int index = 0;
int shift;

void setup() {
  for (int i = 0; i< 4; i++) {
    pinMode(leds[i], OUTPUT);
  }
}

void loop() {
  shift = (index < 4) ? index : 6 - index;

  for (int i = 0; i < shift; i++) {
    pattern = (pattern << 1) | 0x01;
  }
  for (int i = 0; i < 4; i++) {
    if (pattern % 2 == 1) {
      digitalWrite(leds[i], HIGH);
    } else {
      digitalWrite(leds[i], LOW);
    }
    pattern = pattern >> 1;
  }

  pattern = 1;
  index++;
  if (index == 7) {
    index = 0;
  }
  delay(1000);
}
```

