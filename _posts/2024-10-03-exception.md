---
layout: post
title: Exception ⚠️
tags:
  - java
  - exception
date: 2024-10-03 15:28 +0900
published: true
---

프로그래밍을 하면 항상 예외의 상황을 만납니다. 자바에선 이를 체계적으로 관리하고 유용하게 다루기 위하여 **Exception**시스템을 제공합니다. <br />

<hr />
<br />

# Java 예외 계층 구조
![exception.png](https://velog.velcdn.com/images%2Fljinsk3%2Fpost%2F82e87be8-cfb6-461f-8b8b-6a86ac7bca69%2FException-in-java1.png)
자바에서 오류는 크게 Error(오류) 와 Exception(예외)로 나뉩니다. <br />

<br />

# Throwable
Error 와 Exception 이 공통적으로 상속하는 부모 클래스이며 오류나 예외에 대한 내용을 가집니다. <br />
*getMessage()*, *printStackTrace()* 메서드로 확인할 수 있습니다.

<br />

# Error
Error 는 시스템 레벨의 오류로, 발생 후 개발자가 처리할 수 없는 예외입니다.<br />
(e.g., StackOverflowError, OutOfMemoryError...)

<br />

# Exception
반면 예외는 개발자가 구현한 로직에서 발생한 실수나 사용자의 영향에 의해 나타납니다.<br />
이는 개발자가 미리 예측하고 방지할 수 있기때문에 상황에 맞는 예외 처리(Exception Handling)가 필요합니다. <br />
(e.g., IOException, NullPointerException, IllegalArgumentException...)

참고로 예외는 오류와 다르게 개발자가 임의로 던질 수 있습니다.

<br />

# Checked Exception 🆚 Unchecked Exception
Exception 은 Checked Exception, Unchecked Exception 으로 나눌 수 있습니다. <br />
RuntimeException 클래스를 상속하면 Unchecked, 상속하지 않으면 Checked Exception 입니다.

<br />

## Checked Exception
* 컴파일러에 의해 처리가 강제되는 예외입니다. 반드시 핸들링을 해줘야 합니다.
* 컴파일 단계에서 확인 가능합니다.
* 예외 발생 시, 트랜잭션 롤백 하지 않습니다.
* Exception 클래스를 직접 상속받습니다.

```java
try {
    FileInputStream file = new FileInputStream("file.txt");
} catch (FileNotFoundException e) {
    System.out.println("File Not Found: " + e.getMessage());
}
```

## Unchecked Exception
* 명시적 예외 처리를 강제하지 않습니다.
* 런타임 시점에 확인 가능합니다.
* 예외 발생 시, 트랜잭션 롤백 합니다.
* RuntimeException 클래스를 상속받습니다.

```java
List<String> list = new ArrayList<>();
String item = list.get(0); // IndexOutOfBoundException 발생 가능, 하지만 처리를 강제하지않음
```

<hr />
<br />

## 예외 활용 팁
* 메서드에서 Checked Exception 을 발생시킬 경우, **throws** 를 사용하여 호출자에게 핸들링을 위임할 수 있습니다.

```java
// 메서드 선언 부
public void readFile(String filename) throws IOException {
    // codes...
}

// 사용처
try {
    readFile("example.txt");
} catch (IOException e) {
    System.out.println("Read File Error: " + e.getMessage());
}
```

* 가능한 **구체적인 예외**를 잡아 처리하는 것이 좋습니다. <br />
막연하게 *(Exception e)* 로 캐치했다면 내가 생각했던 예외가 아니라 다른 경우가 걸릴 수 있습니다.
* 저수준의 예외를 잡아 고수준의 예외로 치환합니다. <br />
구체적인 세부사항을 사용처에 노출하지 않고, 더욱 의미있으며 내부 구현이 변경되어도 동일한 예외 타입을 유지할 수도 있습니다.

<br />

# Custom Exception
특정 상황에 맞는 사용자 정의 예외를 만들 수 있습니다.
```java
public class CustomException extends Exception { // Exception 클래스를 상속받아 정의
    public CustomException(String message) {
        super(message);
    }
}

if (isExist) {
    throw new CustomException("Custom Exception Occurred");
}
```
