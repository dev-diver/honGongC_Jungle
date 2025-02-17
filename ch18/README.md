# CH18 파일 입출력

## 파일 개방과 폐쇄

- 개방 : 데이터를 입출력 하기 전에 준비하는 과정
- 폐쇄 : 사용이 끝난 파일을 닫는 과정
- `fopen()`: 실제 파일이 있는 장치와 연결되는 스트림 파일을 메모리에 만듬. 파일 포인터 반환
    - `FILE *fopen(const char *, const char *)`
- `flose()` : 스트림 파일은 메모리를 사용하므로, 이를 회수해야 함. 닫으면서 데이터에 기록함(?)
    - `int fclose(FILE *)`
        - 정상이면 `0` 반환, 비정상 시 `-1` 반환

## 읽기 모드

- r : 읽기 위해 개방
- w : 파일의 내용을 지우고 씀  -  같은 이름이 있어도 새로 만드므로 주의!
- a: 파일의 끝에 추가
- 모드 상관 없이 NULL 체크는 해주는 것이 좋다.
    - 개방하라 수 있는 파일 수를 넘거나, 하드디스크 장치에 문제가 생길 수도 있다.

## 스트림이란?

- 프로그램과 입출력 장치 사이의 다리 역할을 하는 논리적인 **파일** (메모리에 있음)
    - ?? ‘스트림 파일’ 이라는 개념이 혼공씨에만 나온다.

### 장점

- 입출력 효율을 높이고, 장치로부터 독립된 프로그래밍이 가능
- 프로그램과 장치의 입출력 속도 차이를 줄일 수 있다.

## 활용코드

```c
(fp=NULL) // 모드
```

## fgetc()

- 파일의 크기와 현재까지 읽어들인 데이터의 크기를 비교하여, 파일의 입력이 끝났는지 판단

## 기타

- `NULL`은 `<stdio.h>` 에서 `#define NULL ((void *)0)` 로 정의되어 있다.
- `EOF` 는 `<stdio.h>` 에서 `#define EOF (-1)` 로 정의되어 있다.

## 다양한 파일 입출력 함수

## 궁금한 것

- 스트림 파일이란 무엇인가?

## 이해 안감

- 예제 18-6 실행해보기

## 문제

- 2 - 프로그램은 필요하다면 스트림을 여러개 개방할 수 있다.