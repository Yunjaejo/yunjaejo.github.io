---
layout: post
title: Try - Catch - Finally
tags:
  - java
  - try-catch-finally
  - jvm
date: 2024-10-17 17:30 +0900
published: true
---
# Java 의 try-catch-finally 이해하기

Java 에서 예외 처리를 위해 사용하는 try-catch-finally 구문에서 return 을 한다면 언제 메서드가 종료될까요?
<hr />

```java
class Main {
  public static int whenReturn() {
    try {
      throw new Exception();
    } catch (Exception e) {
      return 1;
    } finally {
      return 2;
    }
  }

  public static void main(String[] args) {
    int result = whenReturn();
    System.out.println(result); // 2
  }
}
```
try 구문에서 예외가 발생하였을 때, catch, finally 에서 각각 다른 값을 반환하는 코드를 작성하였습니다. <br />
일반적으로 메서드는 return 키워드를 만날 때 종료되고, try-catch 는 finally 블럭이 성공-실패에 상관없이 항상 실행됩니다. <br />
<br />
정답부터 얘기하자면 finally 에서 반환한 값 2가 출력됩니다. 왜 이런 것일까요? <br />
JVM 의 동작 원리를 보며 간단하게 정리해보았습니다.

## 1. JVM의 예외 테이블 생성

```java
try {
  // 코드
} catch (Exception e) {
  // 예외 처리
} finally {
  // 정리 작업
}
```

컴파일러는 이 코드를 바이트코드로 변환하면서 예외 테이블(Exception Table)을 생성합니다. <br />
예외 테이블에는 다음 정보들이 포함됩니다.
* try 블럭의 시작과 끝 위치 (from, to)
* catch 블럭의 위치 (target)
* finally 블럭의 위치
* 처리해야 할 예외 타입의 정보 (type)

## 2. JVM의 finally 블럭 처리 방식
JVM은 try 블럭 실행 전에 finally 블럭의 주소를 특별한 레지스터에 저장합니다. <br />
try 구문에서 예외가 발생하거나 return이 실행되면, JVM은 저장된 finally 블럭의 주소를 확인하고 그 코드를 동작시킵니다. <br />
(당연하게도 *System.exit()* 가 호출되는 경우는 finally 의 실행없이 즉시 종료됩니다.) <br />
<br />

## 3. 바이트코드 레벨에서의 동작
try 혹은 catch 블럭에서 return 한다면, 그 값을 임시 저장한 후 finally 블럭을 실행합니다. <br />
간단한 슈도코드로 나타내보자면
```shell
TRY_START:
    push 1
    store to temp variable
    jump to FINALLY
FINALLY:
    push 2
    return
TRY_END:
```

일반적으로 finally 블럭에서는 자원 해제나 상태 정리를 해야하지만 만약 값을 반환한다면 <br />
임시 저장된 값을 덮어씌워 최종 값을 수정합니다. <br />
<br />

<hr />

finally 의 존재는 약간의 오버헤드를 발생시킬 수 있습니다만 현대 컴퓨팅 파워에서는 무시할만한 수준이라고 얘기들을 합니다. <br />
try-with-resources 구문으로 finally 구문 없이도 자원 관리를 효과적으로 할 수 있어 다음에 한번 다뤄보겠습니다.
