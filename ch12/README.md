# CH12 문자열

## 문자열 상수

- 문자열 상수는 컴파일 과정에서 char 배열 형태로 따로 보관하고, 문자열 상수가 있던 곳에는 배열의 위치 값을 사용한다.
- 같은 문자열 상수는 계속 같은 주소를 가리킨다.
- **문자열 상수 주소로 접근해 값을 바꾸면 안된다.**
    - read-only 이므로 운영체제에 따라 프로그램을 강제 종료한다.
- char 포인터에도 대입 가능
    
    ```c
    char *dessert = "apple";
    ```
    
    - 포인터를 통한 문자열 상수 변경도 불가
        
        ```c
        int main(void)
        {
          char *dessert ="apple";  
          dessert[2]='Z';
          printf("%s",dessert);
        }
        ```
        
    - 포인터 문자열로 scanf도 불가
        - scanf의 변수로 포인터를 넘기면, 문자열 상수를 새로 덮어씌워야 하는데, 문자열 상수는 변경이 불가하다.
- **일부 문자열 변경을 위해서는 char 배열을 사용하자**!

## 문자열 대입

- char배열
    - 문자열 상수 바로 대입 가능
    - 다른 문자열 대입하려면 `strcpy()` 써야 함.
    - 문제열 교체 가능
    - `scanf()` 입력 가능
- 포인터
    - 문자열 상수 바로 대입 가능
    - 이후 다른 문자열 대입 가능
    - 중간에 문자열 교체 못함
    - `scanf()` 입력 못함
    - 동적 할당 가능.
        - 동적 할당 이후 문자열 상수 바로 대입 하지 말자. → 동적 할당된 주소가 상수의 주소로 바뀐다.
        - 즉. `strcpy()` 써야 함.

## 문자열 입력 함수

### scanf()

- 버퍼 중 **공백 전까지** 문자만 배열로 가져가 문자열로 저장
- 다음 입력 시 %d,%s는 기존 공백 무시함

### gets()

- 공백까지도 문자로 가져가 배열에 저장.
- 개행문자**까지** 문자를 배열로 가져감 → **마지막 개행을’ \0’으로 바꿔 문자열에 저장**

### fgets()

- gets에서 배열의 사이즈도 입력하여, 사이즈만큼만 문자열을 입력함.
- stream 정할 수 있음 (stdin, file 등)
- **개행 문자도 배열에 저장**함 → 마지막 개행 ‘\0’으로 **안 바꿈**

### 버퍼 공유 문제

- Enter등으로 버퍼가 남으면, 다음 버퍼 이용 함수에 side effect가 발생할 수 있다.
    - gets는 남은 **개행문자**를 ‘입력완료’로 인식한다.
        - 문자 입력 하나도 없이 엔터만 쳐도 입력완료 (’\n’)이 배열에 저장된다.
    - scanf는 문자 입력 전의 공백(enter, spacebar tab 등) 은 ‘입력완료’로 인식하지 않고 계속 기다린다.
- 질문.  **이 코드는 왜 될까?**
    
    ```c
    #include <stdio.h>
    
    int main() {
        char name[20];
        int age;
    
        printf("Enter your age: ");
        scanf("%d", &age);
    
        printf("Enter your name: ");
        scanf("%s", name);
        printf("Name: %s, Age: %d\n", name, age);
    
        return 0;
    }
    ```
    
    - 답변: scanf는 문자열(%s)를 입력할 때 문자 입력 전 space bar, tab, enter는 입력해도 계속 입력을 기다린다.
        - **%d도 마찬가지.  %c는 문자이므로, ‘\n’ 도 문자로 받아들이는 것으로 보인다.**
        - `scanf(” %c”)` 로 하면 커버 가능할 듯
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/66d8752d-3be4-4738-b470-67f1773c7ea6/1cee4df9-d4d0-4478-8079-b6e065c65741/Untitled.png)
        
    

### 해결법 - 버퍼 미리 비우기

```c
getchar();
scanf("%*c");  //*이 할당하지 않을 값을 나타냄. 변수를 쓸 필요 없게 만들어줌
fgetc(stdin);
```

## 문자열 출력 함수

### puts()

- 출력 후 줄 바꿈

### fputs()

- 출력 후 줄 안 바꿈.
- printf 보다 string 특화, stream을 정할 수 있음.

## 문자열 연산 함수

- `<string.h>` include

### strcpy()

- 문자열 대입 함수
- 첫번째 인수 - char배열, char배열명을 저장한 포인터만 가능
- 두번째 인수 - 다른 배열의 문자열, 문자열상수 연결 포인터, 배열 연결 포인터 모두 가능

### strncpy()

- strcpy() 와 같은 기능에 복사할 길이가 정해진 함수
- **null은 자동 저장하지 않음에 유의**

### strcat(), strncat()

- 문자열에 문자열을 이어붙임
- 기존 문자열은 반드시 초기화가 되어 있어야 함.
    
    ```c
    char str[80] ={'\0'};
    char str[80] ={0};
    char str[80] = "";
    char[0] = '\0';
    ```
    
    - `\0`을 기준으로 그 뒤에 이어붙이기 때문에.
- 기존 배열 메모리 overflow에 유의

### strlen()

- sizeof는 배열 전체의 크기를 계산. (선언된 크기를 계산한다.)
- strlen은 \n 까지의 문자열 갯수를 센다.
- 정상적이라면 strlen ≤ sizeof

### strcmp(), strncmp()

- 문자열 비교
- ASCII순으로 비교한다. (완전 사전식은 아님)
- `strcmp(str1,str2)`
    - `str1>str2` : 1 (양수인 듯)
    - `str1 == str2` : 0
    - `str1 < str2` : -1 (음수인 듯)

## 연산 함수 직접 구현해보기

## 문제

- 헷갈린 문제
    
    ```c
    char str[20] ="apple";
    printf("%s",str[0]);  //이게 안 되는 이유?
    //-> 변수로 주소값이 아닌 값 'a'가 넘어간다.
    ```
    
- 문제 3번
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
      char ch;
      printf("단어 입력: ");
      ch = getchar();
      printf("입력한 단어: ");
      int sum = 0;
      while(ch!='\n')
      {
          char res = ch;
          if(sum>4){
            res = '*';
          }
          printf("%c",res);
          ch=getchar();
          sum++;
      }
    }
    ```
    

### 도전 실전 예제

- 풀이
    
    ```c
    #include <stdio.h>
    #include <string.h>
    
    void cmpswap(char *, char *);
    
    int main(void) {
      char str[3][80];
      printf("세 단어 입력: ");
      scanf("%s %s %s", str[0], str[1], str[2]);
    
      cmpswap(str[0], str[1]);
      cmpswap(str[1], str[2]);
      cmpswap(str[0], str[2]);
    
      printf("%s, %s, %s", str[0], str[1], str[2]);
    }
    
    void cmpswap(char *a, char *b) {
      char temp[80];
      printf("%d\n", strcmp(a, b));
      printf("temp:%s, a:%s b:%s\n", temp,a,b);
      
      if (strcmp(a, b) > 0)
      {
        // temp = a;  //얘가 안 되는 이유는 뭘까?
        // a = b;
        // b = temp;
        strcpy(temp,a);
        strcpy(a,b);
        strcpy(b,temp);
        printf("변경");
      }
      printf("temp:%s, a:%s b:%s\n", temp,a,b);
    }
    ```
    
    - `temp=a` 이 코드는 함수 안에서는 작동했다.
    - 그러나 cmpswap에 넘겨준 것도 결국 주소의 ’복사값’ 이므로 함수 안에서 주소를 바꿔도 함수 밖에 반영되지는 않는다. (변수 값을 바꿔도 밖에 반영 안되는거랑 똑같음)
    - 따라서 `strcpy` 를 통해서 각 주소가 가리키고 있는 배열의 값을 직접 바꿔줘야 한다.