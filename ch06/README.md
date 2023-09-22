# CH06 반복문

- 반복 횟수를 알 수 있도록 작성하자
- 반복 횟수를 세는 변수를 반복문 안에서 바꾸지 말자.

반드시 **do~while을 써야 하는 경우?  → 초기값을 while에서 참으로 못 만들어주는 경우**

- 조건식에 들어갈 값이 미리 결정되지 않는 경우. (do~ while 문 안에서 결정되는 경우)

- 반복되는 기본 문장을 basic operation 이라 부름.
- break
    - 자신을 포함하는 반복문 하나만 벗어남.
    - 반복문 이외의  블록에서 사용하면 그 블록을 포함한 반복문을 벗어남
    - switch~ case 문에서 사용하면 switch~case 블록만 벗어남

- **중첩문 안에서 int 선언 안 알려준 듯**

### 도전 실전 예제

```c
#include <stdio.h>

int main(void) {

  int number;
  int prime;
  int column = 0;
  
  printf("2 이상의 정수를 입력하세요:\n");
  scanf("%d",&number);
  for(int i=2; i<number; i++){

    prime=1;
    for(int j=2; j<i; j++){
      if(i%j==0) {
        prime=0;
        break;
      }  
    }
    if(prime){
      printf("%5d",i);
      column++;
      if(column%5==0){
        printf("\n");
      }
    }
  }
  
  return 0;
}
```

- %5d가 포인트!