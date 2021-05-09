
어노테이션은 클래스나 메소드 등의 선언시에 @를 사용하는 것을 말한다.

어노테이션은,
* 컴파일러에게 정보를 알려주거나
* 컴파일할 때와 설치 시의 작업을 지정하거나
* 실행할 때 별도의 처리가 필요할 때

사용한다.

## 1. 미리 정해져 있는 어노테이션

* @Override: 해당 메소드가 부모 클래스에 있는 메소드를 Override 했다는 것을 명시적으로 선언한다.
* @Deprecated: 컴파일러에게 더 이상 사용하지 않는다는 것을 알려주고 사용하면 경고를 해주는 역할을 한다.
* @SupressWarnings: 경고가 뜨는 상황에서 컴파일러에게 일부러 이렇게 코딩한 거니까 경고할 필요 없다고 말해준다.


## 2. 어노테이션을 선언하기 위한 메타 어노테이션

우리는 직접 어노테이션을 만들 수 있다. 메타 어노테이션은 우리가 어노테이션을 선언할 때 사용한다.

* @Target: 어노테이션을 어떤 것에 적용할지를 선언할 때 사용한다.
* @Retention: 얼마나 오래 어노테이션 정보가 유지되는지를 선언할 때 사용한다.
* @Documented: 어노테이션에 대한 정보가 Javadocs(API) 문서에 포함된다는 것을 선언한다.
* @Inherited: 모든 자식 클래스에서 부모 클래스의 어노테이션을 사용 가능하다는 것을 선언한다.

## 3. 어노테이션 선언

위에서 말했듯이, 어노테이션을 직접 선언할 수 있는데 어떻게 하는 것인지 확인해보자.

```java=
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface UserAnntion {
    public int number();
    public String textIO default "This is firest annotation";
}
```
이렇게 구현하면 우리는 메소드를 선언할 때 직접 만든 어노테이션을 사용할 수 있다.