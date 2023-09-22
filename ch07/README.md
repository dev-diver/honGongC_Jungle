# CH07 함수

## 함수 만들기

- 함수 정의(Definition) - 함수를 실제 코드로 만드는 것.
- 함수 호출(Call) - 함수를 실행함. 함수명 뒤에 괄호'()'를 붙임.
- 함수 선언(Declaration) - 프로그램 상단에서 어떤 함수를 사용할 것이라고 **컴파일러에 정보를 주는 역할**.
    - 함수 원형에 세미콜론을 붙인 것.
    - 매개 변수 이름은 생략 가능

## 함수 원형(function prototype)

- 함수명: 함수의 기능에 맞는 이름
- 매개변수: 기능 수행에 필요한 데이터
- 반환형: 수행된 후의 결과

## 주의

- 함수는 다른 함수 안에서 정의할 수 없음. main과 별도의 구역에서 만듦.
- 넘겨주는 ‘값’은 **인수(argument)**,  넘겨받는 ‘변수’는 **매개변수(parameter)**
- 컴파일러는 함수를 호출할 때 반환값을 저장할 공간을 미리 준비해둔다.
    - 반환값을 식별할 수 있는 이름은 없다.(변수에 복사할 수는 있다.)

### 함수 선언이 필요한 이유

1. 함수 선언에서 **반환값의 형태 확인**
    - 함수 정의 자체가 호출 전에 있다면 상관 없다.
    - 하지만 함수가 많아지면서 호출이 꼬이는 경우가 있으므로, 그냥 선언하자.
2. 함수 **호출 형식**에 문제가 없는지 검사 
    - 컴파일 단계에서 확인 가능.
        
        → main 앞쪽에서 선언하지 않으면 경고 자체를 안 해주고 이상한 값이 나옴
        
    - **C99 이후 버전에서는 암시적 함수 선언이 가능하다.**
        
        **→ 반환이 무조건 int형으로 됨.  반환형이 int가 아닌 경우 문제가 됨.**
        
    - **코드**
        
        ```c
        #include <stdio.h>
        int sum(int x, int y);
        int main(void)
        {
        	double a = 10.1, b=20.0;
          int result;
        	
        	result = sum(a,b);
        	printf("result: %d\n", result);
        
        	return 0;
        }
        
        int sum(int x, int y)
        {
        	return (int)(x+y);
        }
        ```
        
        - 선언형이 없으면 **형변환 없이 double로 처리**함.
3. 분할 컴파일에서 반드시 필요
- **예외처리** : 프로그램을 실행하다 발생할 수 있는 예외 상황에 대비하여 코드를 추가하는 것
    - 아래 코드에서 문자’a’를 입력할 시 예상치 못한 값을 반환함
        
        ```c
        int main(void) {
        
          int num;
          scanf("%d", &num);
          printf("%d",num);
          return 0;
        }
        ```
        
        - 예외처리를 위해서는 scanf의 반환값을 받아야 함.
            - scanf는 성공한 입력의 갯수를 숫자로 반환한다.  따라서, 1개 입력받았는데 실패한 경우 0을 반환
        - 문자를 입력했을 시 반환하는 값은, num의 초기화되지 않은 값임.
- 반환값이 `void`인 경우 `return`을 생략하면, 모두 실행하고 알아서 함수가 끝난다.

### 재귀호출 기본형

```c
void fruit(int count)
{
	printf("apple\n");
  if(count == 3) return;
  fruit(count + 1);
}
```

- 재귀의 장단점
    - 장 : 경우에 따라 복잡한 반복문을 간단히 표현할 수 있다.
    - 단:  코드 읽기가 쉽지 않다. 반복 호출되면서 메모리를 사용한다.

### 도전 실전 예제

```c
#include <stdio.h>

int res_func(int n);

int main(void)
{
  printf("%d",res_func(10));
  return 0;
}

int res_func(int n)
{
  int res;
  if(n==1) return 1;
  res = res_func(n-1) + n;
  
  return res;
}
```

- 백준 문제
    
    [재귀의 귀재](https://www.acmicpc.net/problem/25501)