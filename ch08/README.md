# CH08. 배열

## 사용 이유

- 같은 형태의 데이터를 반복문으로 처리하기 위해서.
- 메모리에 연속적으로 저장해 놓고 쪼개서 사용하는 방법

## 기본

- 배열 요소(element) - 배열의 나누어진 조각
- 첨자(index) - 배열에서의 위치

### 문법

```c
int ary1[5];
int ary2[3]={0,1,2};
int ary3[3]={0,}; //자동채움
int ary4[] = {0,1,2,3} //메모리 크기 자동으로 정해짐.
char ary5[] ="apple" //char 배열은 문자열로 바로 지정 가능   {'a','p','p','l','e'} 와 같음

sizeof(ary) //크기
count = sizeof(ary)/sizeof(ary[0]) // 배열 요소의 수
for(int i=0; i<count; i++){}  // 배열 반복

```

## 자동 초기화

- `int arr[10] ={1,};` 인 경우 뒷쪽은 모두 0으로 자동채움한다.
- char 의 경우 **널문자(Null; ’\0’)** 로 자동 채움.

## char형 배열 주의점

- 배열의 크기는 최대한 넉넉하게 선언
- 배열 요소의 개수는 최소한 ‘문자열의 길이 +1’   (마지막에 null 들어갈 자리 있어야 함)

```c
char str1[]="cat";
char str2[80];
strcpy(str2,str1)  //str2에 str1 복사.    str2=str1 안됨.
strcmp(str1,str2) // 크기 비교.  0 이면 둘이 같은 string임
```

- 직접 대입이 안되는 이유:  각 변수는 실제로 값이 아니라 주소이기 때문에.
- String 덮어 씌웠을 때, /n 이후에 이전 자료가 남아 있음
    
    ```c
    #include <stdio.h>
    
    int main(void) {
      char str[200];
      printf("긴 문자열 입력:");
      scanf("%s", str);
      printf("덮어씌울 문자열 입력:");
      scanf("%s", str);
      printf("%s", &str[5]);
    
      return 0;
    }
    
    //applepie
    //app
    //> pie 출력됨
    ```
    
- 초기화하지 않고 일일히 문자열을 넣는 경우 **반드시 null을 마지막에 넣어줘야 함**
    - 안하면 쓰레기 값이 계속 나옴. (printf 등은 null로 char 배열의 끝을 판단한다.)
    - **Null**문자의 상수 표현법  **‘/0’**
- scanf()의 단점. 중간에 빈 칸이 있는 경우 **빈 칸 전까지만 입력**한다.
    - **gets()**
- puts() : 출력 이후에 자동으로 줄을 바꾼다.
- **scanf(), gets() 에서 컴파일 보안 문제가 있는 이유는, 뒷쪽 메모리까지 침범할 수 있기 때문이다 (Buffer Overflow, BOF)**
    - [버퍼오버플로우를 이용한 해킹](https://kaspyx.tistory.com/2) → stack smashing, heap smashing
        - 요약: buffer를 넘어서 함수 호출부까지 덮어씌워, 내가 원하는 코드를 실행하게 할 수 있다.
    - [위키 읽어보기](https://en.wikipedia.org/wiki/Buffer_overflow)
    - 대안은? → **길이를 받는 함수를 이용**한다.
        - scanf() → fscanf()
        - strcpy() → strncpy()
        - gets() →gets_s()
        - strcmp()→strncmp()→memcmp()
            - gets와 조합됐을 때 취약
        - strcat()→strncat()
        - getwd()→getcwd()
            - char *getwd(char *buf);
            - 현재 위치를 출력하는 함수.
        - sprintf() → snprintf()
            - 문자열을 문자열 변수에 저장하는 함수
    - 대안 함수를 쓰더라도, 길이를 잘못 입력하게 프로그래밍하는 경우 경우에는 BOF 발생
        - 버퍼 길이가 아닌, 입력의 길이 사용하기
        
        ```c
        #include<stdio.h>
        #include<string.h>
        
        int main(int argc, char *argv[]){
        
        	char buffer[10];
        	int length;
        	length = strlen(argv[1]); 
        
        	strncpy(buffer,argv[1],length); // 버퍼의 크기보다 큰값이 들어갈 수 있음.
        }
        
        ```
        
        - 버퍼 길이 체크 했음에도, 음수 처리돼서 통과
        
        ```c
        #include<stdio.h>
        #include<string.h>
        int main(){
        
        	char length = 0; //int가 아니라 char로 선언함!
        	char buffer[20] = {0,};
        
        	length = strlen(argv[1]) // 128바이트 이상 입력시 음수로 인식됨
        
        	if(length>20){
        		printf("에러! 20바이트를 넘어섰습니다 !");
        	}else{
        		printf("글자수를 만족합니다. 함수를 실행합니다"); // 프로그래머 부주의
        		strcpy(buf,argv[1]);
        	}
        
        }
        ```
        
        - unsigned형인 size_t을 이용해야 하는 이유.

## 궁금한 점

- 배열의 크기를 바꿀 수 없나?
    - 메모리가 이미 할당되어 있어 어렵다.
    - 포인터와, malloc을 사용하여 동적할당 할 수 있다. (공부 필요)

### 도전 실전 예제

```c
#include <stdio.h>

int main(void)
{
  char str1[200];
  char str2[200];
  int chn = 0;
  
  
  printf("문장 입력 :");
  gets(str1);

  int i = 0;
  char s;
  while(str1[i]!='\0')
  {
      s = str1[i];
      if(s>='A'&&s<='Z')
      {
        s+='a'-'A';
        chn++;
      }
      str2[i]=str1[i];
      i++;
  }
  str2[i]='\0';
  
  printf("바뀐 문장 : %s\n",str2);
  printf("바뀐 문자 수: %d\n",chn);
  
  return 0;
}
```

- 풀이
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
         char str[80];
         int cnt = 0;
         int i;
    
         printf("문장 입력 : ");
         gets(str);
    
         for(i=0; str[i] != '\0'; i++)
         {
               if((str[i] >= 'A') && (str[i] <= 'Z'))
               {
                    str[i] += ('a'-'A');
                    cnt++;
               }
         }
    
         printf("바뀐 문장 : ");
         puts(str);
         printf("바뀐 문자 수 : %d\n", cnt);
    
         return 0;
    }
    ```
    
    - str2 필요 없음
    - gets로 띄어쓰기도 받을 수 있음