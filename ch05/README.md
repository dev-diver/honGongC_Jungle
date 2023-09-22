# CH05 선택문

### 도전 실전 예제

```c
#include <stdio.h>

int main(void)
{
  int a,b,res;
  char op;
  printf("사칙연산 입력(정수) : ");
  scanf("%d%c%d",&a,&op,&b);
  
  switch(op)
  {
    case '+':res=a+b;break;
    case '-':res=a-b;break;
    case '*':res=a*b;break;
    case '/':res=a/b;break;
  }
  printf("%d%c%d=%d",a,op,b,res);

  return 0;
}
```

- 문자는 ‘  ‘ 표시하는 것에 주의!  (”  “  아님!)