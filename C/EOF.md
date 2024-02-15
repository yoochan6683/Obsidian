#### EOF(End Of File)
주어진 입력 파일만 갖고 입력을 받을 때 더이상 읽을 수 있는 데이터가 없는 경우 즉, 파일의 끝

*터미널에서 문장의 끝을 Enter로 치지만 Enter(개행) 또한 하나의 문자*(0x0a)

파일:
```js
abcd<EOF>
```

일때, status는 true,true,true,true,false 로, 단순히 d까지 읽었다고 EOF가 되는 것이 아니라 끝에 도달 한 후 더 읽으려고 할 때 EOF가 됨

<mark>에디터에서는 입력 파일을 따로 생성하여 읽지 않는 한 일반적인 키보드에서는 EOF키가 없음. CTRL + D로 입력 가능(윈도우 CTRL + Z)</mark>

### EOF로 입력 중단(백준 10951)
```c
#include <stdio.h>

int main()
{
int a, b;

while(scanf("%d %d", &a, &b) != -1) {
printf("%d\n", a + b);
}

return 0;
}
```


