# CH13 변수의 영역과 데이터 공유

## 변수 범위가 나뉘면 좋은 이유

- 메모리를 효율적으로 사용
- 디버깅에 유의
- 같은 이름을 여러번 쓸 수 있음

## 전역변수의 단점

- 전역변수의 이름을 바꾸면, 그 변수를 사용하는 모든 함수를 수정해야 함( 리팩토링이 어려움)
- 값의 접근이 잘못된 경우, 접근 가능한 모든 함수를 의심해야 함
- 코드 내 같은 이름의 지역변수를 사용하면, 그 영역에서는 전역 변수 사용 불가

## 변수의 범위

- 지역 변수
    - `auto` (보통 생략됨) 예약어로 선언. 블럭 안에서 유효
    - 같은 이름이면 가장 가까운 블럭에 선언된 변수가 우선
- 전역 변수
    - 여러 파일에서 접근 가능
    - main 함수 바깥에 선언하면 전역변수
    - 아무 keyword없이 `{}` 로 블록을 만들어 줄 수 있다.

## 변수의 수명

- 정적 지역변수 (static)
    - 함수가 반환되더라도 저장 공간을 유지함
    - 여러번 호출하는 경우 같은 변수를 공유하는 것이 가능하다!
    
    ```c
    void static_func(void)
    {
    	static int a;    //초기화 안하면 자동으로 0이 됨
    	a++;
    }
    ```
    
    - 다른 값으로 초기화도 가능
    
    ```c
    static int a = 10;  //초기화는 한 번만 됨
    ```
    
    - 실험코드
        
        ```c
        #include <stdio.h>
        
        void static_func(void);
        
        int main(void)
        {
          static_func();
          static_func();
          static_func();
        
          return 0;
        }
        
        void static_func(void)
        {
          static int a=10;
          a++;
          printf("%d\n",a);
        }
        
        //결과
        //11
        //12
        //13
        ```
        
- 정적 전역변수 (static) - 아직 안 배움

## 변수의 속도

- register
    - 반복문에서 변하는 변수 등 자주 접근하는 변수에는 register를 쓰면 더 빠르다.
        - 실제로 3배 이상 줄일 수 있다.
    - 메모리가 아닌 register에 기억함
    - 주의
        - 전역 변수는 레지스터 변수 선언이 안 됨
        - 레지스터 변수는 주소를 못 구함 → 메모리에 있는 것이 아니라서
        - 무조건 레지스터로 사용되는 건 아님 → 컴파일러가 상황에 따라 결정함

## 자동 초기화

- 지역변수 - 자동 초기화 되지 않는다.
- 전역변수, static 변수 - 선언만으로 자동 초기화 된다.

## 함수의 데이터 공유 (매개변수와 반환)

- 매개변수
    - 값 복사
    - 주소 전달
- 반환
    - 값 반환
    - 주소 반환
        - 정적 지역 변수, 전역 변수, 전달받은 주소

### 문법

```c
//값 복사
func1(a,b);  //호출
void func1(int a, int b) //선언
{

}

//주소 전달
func2(&a, &b); //호출
void func2(int *pa,int *ba) //선언
{
	
}

//값 반환
int a = func3(); //호출
int func3(void)  //선언
{
	return 3;//반환
}

//주소 반환
int b; //전역변수일 때
int *a = func4();  //호출
int *func4()  //선언
{
	return &b; //반환(주소)
}
```

- C에는 call by reference가 없다? (feat. chatGPT)
    - reference는 C++에서 등장한 개념. (C에는 없음)
        
        ```c
        int x =42;
        int &ref_x = x;
        ref_x = 100;
        
        void func(int &x)
        {
        	x=x*2
        }
        ```
        
        - 변수에 대한 별명 개념
        - 레퍼런스를 통해 변수에 접근하면 실제 변수의 값을 변경 가능, 변수의 메모리 주소를 직접 다루지 않아도 된다.
        - 포인터와의 차이점 : 초기화시 무조건 초기화 해야하고, 이후에 변경 불가능하다.
        - **함수에 레퍼런스를 넘겨주면, 복사가 아닌 원본 변수에 접근하여 작업할 수 있다**
            - 내부적으로는 포인터를 넘겨줬을 때도 똑같이 동작한다([어셈블리어 수준에서](https://woo-dev.tistory.com/43))
            - 포인터의 위험성이 줄어들어 레퍼런스를 대신 사용한다.
                - 초기화를 무조건 해야 하므로 Null 값이 없다.
                - 포인터의 연산을 통한 범위 값 메모리 접근의 위험이 레퍼런스에서는 없다.
- 주소값 전달도 결국엔 call by value이다.

## 문제

### 틀린문제

- 하나의 함수 내에서는 같은 이름의 변수를 선언할 수 없다.(X) →블록 쓰면 된다.

### 도전실전예제

- 내 풀이 - 정답과 같음
    
    ```c
    #include <stdio.h>
    
    void input_data(int *pa, int *pb);
    void swap_data(void);
    void print_data(int a, int b);
    
    int a, b;
    
    int main(void)
    {
      input_data(&a, &b);
      swap_data();
      print_data(a, b);
      
      return 0;
    }
    
    void input_data(int *pa, int *pb)
    {
      printf("두 정수 입력: ");
      scanf("%d %d",pa, pb);
    }
    
    void swap_data(void)
    {
      int temp;
      temp = a;
      a = b;
      b = temp;
    }
    
    void print_data(int a, int b)
    {
      printf("두 정수 출력: %d, %d",a, b);
    }
    ```