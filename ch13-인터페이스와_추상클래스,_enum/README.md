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

위 코드와 같이 implments를 사용하여 인터페이스를 구현할 수 있고 상속과 다르게 여러 개를 implements 할 수 있다.

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