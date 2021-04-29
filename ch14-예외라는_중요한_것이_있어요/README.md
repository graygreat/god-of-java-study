## 1. 예외

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

### 1-1. finally

finally는 무슨 일이 생겨도 반드시 실행되는 try-catch 문에서 사용하는 단어이다.
보통 try-catch문 마지막에 작성해준다.

### 1-2. 두 개 이상의 catch

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
