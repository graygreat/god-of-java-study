## 1. Interface

시스템을 개발 할 때 우리는 아래와 같은 과정을 거치게 된다.
> * 분석
> * 설계
> * 개발 및 테스트
> * 시스템 릴리즈

**설계 단계**에서 우리는 인터페이스와 추상 클래스를 만들 수 있다.
설계 단계에서 인터페이스나 추상 클래스를 만들게 되면 메소드의 이름을 어떻게 할지, 매개 변수를 어떻게 할지 고민하지 않아도 된다는 장점이 있다.

책에서는 추상 클래스와 인터페이스의 사용 이유를 다음과 같이 정의한다.
> * 설계시 선언해두면 개발할 때 기능을 구현하는 데에만 집중할 수 있다.
> * 개발자의 역량에 따른 메소드의 이름과 매개 변수 선언의 격차를 줄일 수 있다.
> * 공통적인 인터페이스와 abstract 클래스를 선언해 놓으면 선언과 구현을 구분할 수 있다.

### 1-1. 인터페이스 기본 구현

인터페이스는 인터페이스 선언을 위한 가장 상위의 중괄호만 있어야 한다.

즉, **내부에 선언된 메소드들의 내용이 있으면 안된다**는 뜻이다.

```java=
public interface MemberManager {
    public boolean addMember(MemberDto member);
    public boolean removeMember(String name, String phone);
}
```
위 코드와 같이 내용 없이 메소드명으로만 인터페이스를 구성한다.

그럼 이 인터페이스를 어떻게 사용할까?

```java=
public class MemberManagerImpl implements MemberManager {
    
    @Override
    public boolean addMember(MemberDto member) {
        return false;
    }
    
    @Override
    public boolean removeMember(String name, String phone) {
        return false;
    }
}
```

위 코드와 같이 **implments**를 사용하여 인터페이스를 구현할 수 있고 상속과 다르게 여러 개를 implements 할 수 있다.

주의해야 할 점은 인터페이스에서 구현한 메소드들을 모두 오버라이딩 해줘야 한다. 그렇지 않으면 에러가 뜬다.

> 설계 단계에서 인터페이스만 만들어 놓고, 개발 단계에서 실제 작업을 수행하는 메소드를 만들면 설계 단계의 산출물과 개발 단계의 산출물이 보다 효율적으로 관리된다.

인터페이스를 설계에서만 사용하는 것은 아니다. 인터페이스의 또 다른 용도는 외부에 노출되는 것을 정의해놓고자 할 때 사용된다.

> MemberManagerImpl이라는 클래스가 있는데, 이 클래스가 "저한테 직접 이야기하지 마시구요. 공식적인 것은 저의 대변인을 통해서 말씀하세요." 

여기서 말하는 대변인이 인터페이스이다.

### 1-2. 인터페이스 형 변환

우리는 인터페이스를 사용할 때 형변환을 할 수 있다.
```java=
public class InterfaceExample {
    public static void main(String[] args) {
        MemberManager member = new MemberManager();
    }
}
```

위 코드와 같이 코드를 작성하면 당연스레 에러가 난다. 인터페이스 자체에는 아무것도 구현되어 있지 않기 때문에 초기화하는 것이 이상하다. 

그럼 우리는 인터페이스를 어떻게 구현해야 될 것인가?

```java=
    MemberManager member = new MemberManagerImpl();
```

앞에 배웠던 형 변환을 사용하여 인터페이스를 구현할 수 있다.
MemberManageImpl 메소드에는 MemberManager의 모든 메소드가 구현되어 있기 때문에 실행하면 MemberManagerImpl에 있는 메소드들이 실행된다.

## 2. Abstract Class

추상화를 할 때 abstract 클래스를 사용할 수 있다.

abstract 클래스는 자바에서 마음대로 초기화하고 실행할 수 없도록 되어 있다. 그래서 그 abstract 클래스를 **구현해놓은** 클래스로 초기화 및 실행이 가능하다.

abstract 클래스는 abstract으로 선언한 메소드가 하나라도 있으면 선언해야한다. 

인터페이스와의 차이점을 생각해보면 인터페이스는 구현되어 있는 메소드가 있으면 안되지만, abstract 클래스는 구현되어 있는 메소드가 있어도 상관 없다. 또한 static이나 final 메소드를 선언하면 안되는 인터페이스와 달리 static, final 메소드가 있어도 된다.

### 2.1 Abstract Class 기본 구현

```java=
public abstract class MemberManagerAbstract {
    public abstract boolean addMember(MemberDto member);
    public abstract boolean removeMember(String name, String phone);
    public void pringLog(String data) {
        System.out.println("Data = " + data);
    }
}
```

위 코드는 추상 클래스를 작성한 코드이다.

위에서 설명했듯이, 인터페이스와 달리 구현되어 있는 메소드가 존재하는 것을 볼 수 있다.

추상 클래스를 구현하기 위해서는 어떻게 해야할까?

```java=
public class MemberManagerImpl2 extends MemberManagerAbstract {
    
    @Override
    public boolean addMember(MemberDto member) {
        return false;
    }
    
    @Override
    public boolean removeMember(String name, String phone) {
        return false;
    }
}
```
다음과 같이 추상 클래스를 상속 받는 것과 같이 **extends**를 사용하고 추상 클래스에 존재하는 메소드들을 모두 구현해주면 된다.

### 2.2 인터페이스와 추상 클래스에 대한 궁금증

나는 여기서 의문이 들었다.

**인터페이스랑 추상 클래스가 하는 역할이 비슷한거 같은데 왜 나눠진걸까?**

그 이유는 공통적인 기능을 미리 구현해놓기 위해서이다.

어떤 메소드는 미리 만들어 놓아도 문제가 없을 것 같을 때 추상 클래스로 미리 구현해두면 된다.

## 3. final 예약어

final은 클래스, 메소드, 변수에 선언할 수 있다.

### 3.1 클래스에 final 선언

```java=
public final class FinalClass {

}
```

클래스에 final을 선언할 때는 누군가 이 클래스를 상속 받아서 내용을 변경해서는 안될때 선언한다.

### 3.2 메소드에 final 선언

```java=
public abstract class FinalMethodClass {
    public final void printLog(String data) {
    
    }
}
```

메소드에 final을 선언할 때는 메소드를 오버라이딩 할 수 없게 된다.

### 3.3 변수에 final 선언


```java=
public class FinalVariable {
    final int instaceVariable = 1;
}
```

변수에 final을 선언하는 것은 클래스와 메소드에 final을 사용하는 것과는 개념이 조금 다르다.

변수에 final을 사용하는 것은 **더 이상 바꿀 수 없다는 뜻**이다.

위 코드와 같이 final 변수를 인스턴스 변수나 클래스 변수로서 선언할 때는 무조건 초기화를 해줘야 한다. 그렇지 않으면 에러가 나게 된다. 반면, 매개 변수나 지역 변수로 final을 선언하는 경우에는 반드시 초기화를 할 필요는 없다.


## 4. enum 클래스

어떤 클래스가 상수로만 만들어져 있을 경우 enum 클래스를 사용할 수 있다.


### 4.1 enum 클래스 구현
```java=
public enum OverTimeValue {
    THREE_HOUR(18000),
    FIVE_HOUR(30000),
    WEEKEND_FOUR_HOUR(40000),
    WEEKEND_EIGHT_HOUR(60000);
    
    private final int amount;
    
    OverTimeValue(int amount) {
        this.amount = amount;
    }
    
    public int getAmount() {
        return amount;
    }
}
```

위 코드와 같이 상수들을 열거하고 값을 지정해줄 수 있다. 물론 값을 지정하지 않아도 된다.

```java=
public class OverTimeManager {
    public static void main(String args[]) {
        OverTimeValue value = OverTimeValue.FIVE_HOUR;
        System.out.println(value);
        System.out.println(value.getAmount());
    }
}

/** 출력
 * FIVE_HOUR
 * 30000
 */ 
```
enum 타입은 **enum 클래스이름.상수 이름**을 지정함으로써 클래스의 객체 생성을 할 수 있다.

