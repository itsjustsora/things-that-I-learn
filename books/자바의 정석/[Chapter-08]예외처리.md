## 1. 예외처리(exception handling)

### 1.1 프로그램 오류

---

- 컴파일 에러 : 컴파일 시에 발생하는 에러

  컴파일러가 소스코드(&#42;.java)에 대해 오타나 잘못된 구문 , 자료형 체크 등의 기본적인 검사를 수행하여 오류가 있는지 알려 준다. 에러를 수정하고 컴파일을 성공적으로 마치고 나면, 클래스 파일(&#42;.class)이 생성되고, 생성된 클래스 파일을 실행할 수 있게 된다.


- 런타임 에러 : 실행 시에 발생하는 에러

  자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 ‘에러(error)’와 ‘예외(exception)’ 두 가지로 구분하였다.

    - 에러 : 메모리 부족(OutOfMemoryError)이나 스택오버플로우(StackOverflowError)와 같이 일단 발생하면 복구할 수 없는 심각한 오류

      → 프로그램의 비정상적인 종료를 막을 길 X

    - 예외 : 발생하더라도 수습될 수 있는 비교적 덜 심각한 것.

      → 발생하더라도 개발자가 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있음

      | 단어 | 정의 |
                  | --- | --- |
      | 에러(error) | 프로그램 코드에 의해서 수습될 수 없는 심각한 오류 |
      | 예외(exception) | 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류 |
- 논리적 에러 : 실행은 되지만, 의도와 다르게 동작하는 것

<br>

### 1.2 예외 클래스의 계층구조

---

자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의하였다.

![Untitled](https://user-images.githubusercontent.com/80027033/169455136-9c238e1f-41d7-4877-afc4-3826feca0db4.png)

예외 클래스들은 다음과 같이 두 그룹으로 나눌 수 있다.

① Exception클래스와 그 자손들

- 주로 외부의 영향으로 발생할 수 잇는 것들로서, 프로그램의 사용자들의 동작에 의해서 발생하는 경우가 많다.
- Example
    - FileNotFoundException
    - ClassNotFoundException
    - DataFormatException

② RuntimeException클래스와 그 자손들

- 주로 프로그래머의 실수에 의해 발생될  수 있는 예외들로 자바의 프로그래밍 요소들과 관계가 깊다.
- Example
    - IndexOutOfBoundsException
    - ClassCastException
    - ArithmeticException


| 단어 | 정의 |
| --- | --- |
| Exception클래스들 | 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외 |
| RuntimeException클래스들 | 프로그래머의 실수로 발생하는 예외 |

<br>

### 1.3 예외처리하기 - try-catch문

---


    🧑‍🎓 예외처리(Exception handling)
    정의; 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
    목적; 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것


 처리되지 못한 예외는 JVM의 ‘예외처리기(UncaughtExceptionHandler)’가 받아서 예외의 원인을 화면에 출력한다.

<br>

### 1.5 예외의 발생과 catch블럭

---

① 예외가 발생하면 발생한 예외에 해당하는 클래스의 인스턴스가 만들어 진다.

② 첫 번째 catch블럭부터 차례로 괄호 내에 선언된 참조변수의  종류와 생성된 예외클래스의 인스턴스에 instanceof연산자를 이용해 true 값이 나올 때까지 검사한다.

③ 찾은 블럭 내에 모든 문장들을 수행한 후에 try-catch문을 빠져나가고 예외는 처리된다.

※ 모든 예외 클래스는 Exception클래스의 자손이므로, catch블럭의 괄호에 Exception클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 처리된다.

<br>

**PrintStackTrace()와 getMessage()**  

| 단어 | 정의 |
| --- | --- |
| printStackTrace() | 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다. |
| getMessage() | 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다. |


<br>

**멀티 catch블럭**

JDK1.7부터 여러 catch블럭을 ‘|’를 이용해서, 하나의 catch블럭으로 합칠 수 있게 되었다. 

```java
try { 
    ...
} catch (ExceptionA e) {
    e.printStackTrace();
} catch (ExceptionB e2) {
    e2.printStackTrace();
}
```

```java
try { 
    ...
} catch (ExceptionA | ExceptionB e) {
    e.printStackTrace();
} 
```

만일 멀티 catch블럭의 ‘|’기호로 연결된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생한다. 이때는 조상 클래스만 써주면 된다.

<br>

### 1.6 예외 발생시키기

---

```java
try {
    Exception e = new Exception("Test Message");
    throw e;
    // throw new Exception("Test Message");
} catch (Exception e) {...}
```

- 컴파일러가 예외처리를 확인하지 않는 RuntimeException클래스들은 ‘unchecked예외’라고 부르고, 예외처리를 확인하는 Exception클래스들은 ‘checked예외’라고 부른다.

<br>

### 1.7 메서드에 예외 선언하기

---

메서드의 선언부에 키워드에 throws를 사용해서 메서드 내부에서 발생할 수 있는 예외를 적어주면 된다. 그리고 예외가 여러 개일 경우에는 쉼표(,)로 구분한다.

```java
void method() throws Exception1, Exception2, ... ExceptionN {
 ...
}
```

※ 예외를 발생시키는 키워드 throw와 예외를 메서드에 선언할 때 쓰이는 throws를 잘 구별하자.

- 메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 이 메서드를 사용하기 위해서 어떤 예외들이 처리되어져야 하는지 쉽게 알 수 있다.
- 일반적으로 RuntimeException클래스들은 적지 않는다. 선언한다고 해서 문제가 되지는 않지만 보통 반드시 처리해줘야 하는 예외들만 선언한다.
- 예외를 메서드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라,  자신을 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것이다.

<br>

### 1.8 finally블럭

---

try-catch문과 함께 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다.

→ try블럭에서 return문이 실행되는 경우에도 finally블럭의 문장들이 먼저 실행된 후에 현재 실행 중인 메서드를 종료한다.

<br>

### 1.9 자동 자원 반환 - try - with - resources문

---

JDK1.7부터 try-with-resources문이라는 try-catch문의 변형이 새로 추가되었다. 이 구문은 주로 IO와 관련된 클래스를 사용할 때 유용하다.

```java
try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
    ...
} catch(IOException ie) {
    ie.printStackTrace();
} finally {
    dis.close();
}
```

close()가 예외를 발생시킬 수 있어 예외 처리를 추가한 코드

```java
try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
    ...
} catch(IOException ie) {
    ie.printStackTrace();
} finally {
    try {
        if(dis != null)
            dis.close();
    } catch (IOException ie) {
        ie.printStackTrace();
    }
}
```

<br>

**이렇게 예외를 처리했을 때 문제점**
- 코드가 복잡해짐
- try블럭과 finally블럭에서 모두 예외가 발생하면 try블럭의 예외는 무시된다.

try-with-resources문이 적용된 코드

```java
try (fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis)) {
    while(true) {
        score = dis.readInt();
        System.out.println(score);
        sum += score;
    }
} catch (EOFException e) {
    System.out.println("점수의 총합은 " + sum + "입니다.");
} catch (IOExecption ie) {
    ie.printStackTrace();
}
```

try-with-resources문의 괄호()안에 객체를 생성하는 문장을 넣으면, 이 객체는 따로 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동으로 close()가 호출된다.

<br>

### 1.10 사용자정의 예외 만들기

---

기존의 예외클래스는 주로 Exception을 상속받아서 ‘checked예외’로 작성하는 경우가 많았지만, 요즘은 RuntimeException을 상속받아서 작성하는 쪽으로 바뀌어가고 있다.

```java
class NewExceptionTest {
    public static void main(String args[]) {
        try {
            startInstall();
            copyFiles();
        } catch (SpaceException e) {
            System.out.println("에러 메시지 : " + e.getMessage());
        } catch (MemoryException me) {
            System.out.println("에러 메시지 : " + e.getMessage());
            ...
            System.gc(); // Garbage Collection을 수행하여 메모리를 늘려준다.
        } finally {
            deleteTempFiles();
        }
    }

    static void startInstall() throws SpaceException, MemoryException {
        if(!enoughSpace())
            throw new SpaceException("설치할 공간이 부족합니다.");
        if(!enoughMemory())
            throw new MemoryException("메모리가 부족합니다.");
    }

    ...
}

class SpaceException extends Exception {
    SpaceException(String msg) {
        super(msg);
    }
}

class MemoryException extends Exception {
    MemoryException(String msg) {
        super(msg);
    }
}
```

<br>

### 1.11 예외 내던지기(exception re-throwing)

---

하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용된다. 이 때 주의할 점은 예외가 발생할 메서드에서는 try-catch문을 사용해서 예외처리를 해줌과 동시에 메서드의 선언부에 발생할 예외를 throw에 지정해줘야 한다는 것이다.

```java
class ExceptionEx17 {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            System.out.println("main메서드에서 예외가 처리되었습니다.");
        }
    }

    static void method1() throws Exception {
        try {
            throw new Exception();
        } catch (Exception e) {
            System.out.println("method1메서드에서 예외가 처리되었습니다.");
            throw e;
        }
    }
}
```

※ 반환값이 있는 return문의 경우, catch블럭에도 return문이 있어야 한다.

<br>

### 1.12 연결된 예외(chained exception)

---

한 예외가 다른 예외를 발생시킬 수도 있다. 예를 들어 예외 A가 예외 B를 발생시켰다면, A를 B의 ‘원인 예외(cause expection)’라고 한다. 

```java
class ChainedExceptionEx {
    public static void main(String args[]) {
        try {
            install();
        } catch(InstallException e) {
            e.printStackTrace();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }

    static void install() throws InstallException {
        try {
            startInstall();
            copyFiles();
        } catch(SpaceException e) {
            InstallException ie = new InstallException("설치 중 예외발생");
            ie.initCause(e);
            thrwo ie;
        } catch(MemoryException me) {
            InstallException ie = new InstallException("설치 중 예외발생");
            ie.initCause(me);
            thrwo ie;
        } finally {
            deleteTempfiles();
        }
    }

    static void startInstall() throws SpaceException, MemoryException {
        if(!enoughSpace())
            throw new SpaceException("설치할 공간이 부족합니다.");
        if(!enoughMemory())
            throw new MemoryException("메모리가 부족합니다.");
    // throw new RuntimeException(new MemoryException("메모리가 부족합니다."));
    }

    ...
}

class InstallException extends Exception {
    InstallException(String msg) {
        super(msg);
    }
}

class SpaceException extends Exception {
    SpaceException(String msg) {
        super(msg);
    }
}

class MemoryException extends Exception {
    MemoryException(String msg) {
        super(msg);
    }
}
```