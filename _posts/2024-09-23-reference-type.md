---
layout: post
title: Call by Value 🆚 Call by Reference
tags:
  - java
  - reference-type
date: 2024-09-23 18:04 +0900
published: true
---

자바의 신 8장 참조 자료형에 대해 공부하다가 <br />
Pass by Value, Pass by Reference 에 대해 나와서 자세히 찾아봤습니다.

<hr />

# Call by Value

직역하면 값에 의한 호출..! <br />
메서드로 매개변수를 넘길 때 값의 **복사본**을 전달하는 방식입니다. <br />
**복사본**이 넘어오기 때문에 원본은 변하지 않습니다.

```java
class CallByValueExample {
  public static void main(String[] args) {
    int num = 10;
    changeValue(num);
    System.out.println(num); // 10
  }

  static void changeValue(int value) {
    value = 20;
  }
}
```

위 예제에서 *changeValue()* 메서드의 매개변수 *num*은 **복사**되어 넘어갔습니다. <br />
메서드에서 넘어온 값을 20으로 바꾼다고 하더라도, 복사받은 값을 수정하였기에 원본 *num*은 변하지 않습니다. <br />

# Call by Reference

직역하면 참조에 의한 호출 <br />
메서드로 매개변수를 넘길 때 **참조 자체**를 전달하는 방식입니다. <br />
**참조**를 직접 넘기기 때문에 호출자의 변수와 수신자의 매개변수는 **완전히 동일한 변수**입니다. <br />
메서드 내에서 매개변수를 수정하면 원본도 변경됩니다.

# 객체 참조 전달

Java에서 객체를 매개변수로 전달할 때는 조금 다른 동작을 보입니다. <br />
참조 자체가 아닌, **참조 값(주소)** 이 복사되어 넘어가는데, 이 값을 통해 원본 객체에 접근할 수 있습니다. <br />
값(x001)이 넘어가기에 이 또한 Call by Value의 한 형태입니다.

```java
class Person {
  String name;

  Person(String name) {
    this.name = name;
  }
}

public class ObjectReferenceExample {

  public static void main(String[] args) {
    Person person = new Person("Arthas");
    System.out.println(person.name); // Arthas
    changeName(person);
    System.out.println(person.name); // Grom
  }

  static void changeName(Person person) {
    person.name = "Grom";
    System.out.println(person.name); // Grom
  }
}
```

이 예제에서 *changeName()* 메서드로 *Person* 객체의 참조값이 복사되어 전달됩니다. <br />
메서드 내에서 이 참조를 통해 원본 객체의 *name* 필드를 "Grom" 으로 직접 변경할 수 있습니다. <br />
<br />
하지만 참조 자체를 변경하는 것은 불가능합니다.

```java
class Person {
  String name;

  Person(String name) {
    this.name = name;
  }
}

public class ObjectReferenceExample {
  public static void main(String[] args) {
    Person person = new Person("Arthas");
    System.out.println(person.name); // Arthas
    changeReference(person);
    System.out.println(person.name); // Arthas (변경되지 않음)
  }

  public static void changeReference(Person person) {
    person = new Person("Grom");
  }
}
```
이 예제에서 *changeReference()* 메서드는 새로운 *Person* 객체를 생성하여 *person* 매개변수에 할당합니다. <br />
하지만 메서드 내부의 *person*만 변경할 뿐, 원본 *person* 변수에는 영향을 미치지 않습니다.
