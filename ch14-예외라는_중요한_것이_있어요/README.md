
> 자바에서 예외는 "우리가 예상한, 혹은 예상치도 못한 일이 발생하는 것을 미리 예견하고 안전장치를 하는 것"

우리는 자바에서 **try-catch문**을 사용하여 예외를 처리할 수 있다.

```java=
public void arrayOutOfBoundsTryCatch() {
    try {
        int[] intArray = new int[5];
        System.out.println(intArray[5]);
    } catch (Exception e) {
    }
}
```

위 코드를 보자.
try문 안에 있는 intArray[5]를 출력하기 때문에 에러가 나게 된다. 하지만 try-catch 문을 통해 예외를 처리할 수 있게 되고 결론적으로 arrayOutOfBoundsTryCatch() 메소드는 정상적으로 컴파일 되게 된다.

## 1. finally

finally는 무슨 일이 생겨도 반드시 실행되는 try-catch 문에서 사용하는 단어이다.
보통 try-catch문 마지막에 작성해준다.

## 2. 두 개 이상의 catch

try-catch문을 작성할 때 여러 개의 catch를 사용하여도 문제 되지 않는다.

```java=
public void multiCatchOrderChange() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
    } catch (Exception e) {
        System.out.println(intArray.length);
    } catch (ArrayIndexOutOfBoundsException e) {
        System.out.println("ArrayIndexOutOfBoundsException occurred");
    }
}
```
위 코드를 실행시키면 어떻게 될까?

에러가 나게 된다. 이유를 살펴보자.

> 모든 예외의 부모 클래스는 **java.lang.Exception 클래스**다. 이 코드에서 예외는 부모 예외 클래스가 이미 catch를 하고, 자식 클래스가 그 아래에서 catch를 하도록 되어 있을 경우에는 자식 클래스가 예외를 처리할 기회가 없다.

**ArrayIndexOutOfBoundsException**은 **Exception 클래스**의 자식 클래스이기 때문에 절대로 Exception 클래스로 처리한 catch 블록 이후에 선언한 블록은 처리될 일 없다. 그래서 컴파일러가 에러를 던져준다.

```java=
public void multiCatchOrderChange() {
    int[] intArray = new int[5];
    try {
        System.out.println(intArray[5]);
    } catch (ArrayIndexOutOfBoundsException e) {
        System.out.println("ArrayIndexOutOfBoundsException occurred");
    } catch (Exception e) {
        System.out.println(intArray.length);
    
}
```
위 코드와 같이 **ArrayIndexOutOfBoundsException**을 먼저 선언하게 되면

> ArrayIndexOutOfBoundsException occurred

우리가 원하는 출력값이 뜨게 된다.

책에서는 다음과 같이 정리해준다.

> * try 다음에 오는 catch 블록은 1개 이상 올 수 있다.
> * 먼저 선언한 catch 블록의 예외 클래스가 다음에 선언한 catch 블록의 부모에 속하면, 자식에 속하는 catch 블록은 절대 실행될 일이 없으므로 컴파일이 되지 않는다.
> * 하나의 try 블록에서 예외가 발생하면 그 예외와 관련이 있는 catch 블록을 찾아서 실행한다.
> catch 블록 중 발생한 예외와 관련있는 블록이 없으면, 예외가 발생되면서 해당 쓰레드는 끝난다. 따라서, 마지막 catch 블록에는 Exception 클래스로 묶어주는 버릇을 들여 놓아야 안전한 프로그램이 될 수 있다.

## 3. 예외의 종류

자바에서는 세 종류의 예외가 존재한다.

* checked exception
* error
* unchecked exception or runtime exception

### 3-1. error

에러는 자바 프로그램 밖에서 발생하는 예외를 말한다.
자바 프로그램에서 오류가 발생했을 때, 오류의 이름이 **Error**로 끝나면 에러이고, **Exception**으로 끝나면 예외이다.

> Error와 Exception으로 끝나는 오류의 가장 큰 차이는 프로그램 안에서 발생했는지, 밖에서 발생했는지 여부이다. 더 큰 차이는 프로그램이 멈추어 버리느냐 계속 실행할 수 있느냐의 차이다. 
> 
> Error는 프로세스에 영향을 주고, Exception은 쓰레드에만 영향을 준다.


### 3-2. runtime exception

런타임 예외는 예외가 발생한 것을 미리 감지하지 못했을 때 발생한다.
런타임 예외에 해당하는 모든 예외들은 **RuntimeException**을 확장한 예외들이다.

이 예외를 묶어주지 않는다고 해서 컴파일할 때 예외가 발생하지 않는다. 하지만 실행시에는 발생할 가능성이 있다. 컴파일 시에 체크를 하지 않기 때문에 **unchecked exception**이라고도 한다.

Exception을 바로 확장한 클래스들이 Checked 예외, RuntimeException 밑에 확장되어 있는 클래스들이 runtime exception이다.

## 4. java.lang.Throwable 

Exception과 Error 클래스는 Throwable 클래스를 상속받아 처리한다.

Throwable 클래스에 선언되어 있고, Exception 클래서에서 Overriding한 메소드는 10개가 넘는데 대표적으로 아래와 같은 메소드를 많이 사용한다.

* getMessage() 
    * 메시지를 활용하여 별도의 예외 메시지를 사용자에게 보여주려고 할 때 좋다.
* toString()
    * getMessage() 메소드보다 약간 더 자세하게, 예외 클래스 이름도 제공한다.
* printStackTrace()
    * 첫 줄에 예외 메시지를 출력하고, 두 번째 줄부터는 예외가 발생하게 된 메소드들의 호출 관계를 출력한다.

## 5. throws, throw

자바에서는 **throw**를 사용하여 예외를 던질 수 있다.

```java=
public void throwException(int number) {
    try {
        if (number > 12) {
            throw new Exception("Number is over than 12");
        }
        System.out.println("Number is " + number);
    } catch(Exception e) {
        e.printStackTrace();
    }
}
```

위 코드를 보면 12 이상인 수에 대해 예외를 던지고 있다.

catch 블록 중에 throw한 예외와 동일하거나 상속 관계에 있는 예외가 있다면 그 블록에서 예외를 처리할 수 있다. 위 코드에서는 e.printStackTrace() 메소드를 호출한다.

만약 해당하는 예외가 없다면 발생된 예외는 메소드 밖으로 던져버린다. 

```java=
public void throwsException(int number) throws Exception {
    try {
        if (number > 12) {
            throw new Exception("Number is over than 12");
        }
        System.out.println("Number is " + number);
    } catch(Exception e) {
        e.printStackTrace();
    }
}
```

위 코드와 같이 메소드 선언에 **throws**를 해놓으면 예외가 발생했을 때 try-catch로 묶어주지 않아도 그 메소드를 호출한 메소드로 예외 처리를 위임하는 것이기 때문에 문제가 되지 않는다.

```java=
public static void main(String[] args) {
    ThrowSample sample = new ThrowSample();
    try {
        sample.throwsException(13);
    } catch(Exception e) {
    
    }
}
```

위 코드와 같이 throws를 사용한 메소드를 try-catch문을 통해 처리해줌으로서 예외를 처리할 수 있게 된다.

## 6. 예외 만들기

예외를 처리하는 클래스를 개발자는 임의로 만들 수 있다. 한 가지 조건이 있는데, Throwable이나 그 자식 클래스의 상속을 받야아만 한다. 보통 예외를 처리하는 클래스라면 
Exception 클래스의 상속을 받는다.

```java=
public class MyException extends Exception {
    public MyException() {
        super();
    }
    public MyException(String message) {
        super(message);
    }
}
```
위 코드와 같이 예외 클래스를 만들 수 있고 위 클래스를 사용하여 다른 예외 클래스들을 처리하는 것과 같이 예외 처리를 할 수 있다.

예외 만들기에 대해 책에서 정리하는 글을 보자.

> * 임의의 예외 클래스를 만들 때에는, 반드시 try-catch로 묶어줄 필요가 있을 경우에만 Exception 클래스를 확장한다. 일반적으로 실행시 예외를 처리할 수 있는 경우에는 RuntimeException 클래스를 확장하는 것을 권장한다.
> * catch문 내에 아무런 작업 없이 공백을 놔두면 예외 분석이 어려워지므로 꼭 로그 처리와 같은 예외 처리를 해줘야만 한다.
