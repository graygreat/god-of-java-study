**String**은 자바에서 가장 많이 사용하는 클래스이다.

평소에 코딩 테스트 문제를 풀 때 String을 가지고 노는 문제들이 많은데 String만 떼어놓고 따로 공부한 적이 없어서 어려움을 많이 겪었다. 이번 스터디를 통해 기초를 확실하게 다져보겠다.

## 1. String 클래스 구조

### 1-1. public final class String

String은 **public final**로 선언되어 있다. 
public이 선언된 클래스는 누구나 다 사용할 수 있는 클래스이고, final이 선언된 클래스는 더 이상 클래스를 확장할 수 없다. 있는 그대로 사용해야 한다.

### 1-2. 상속, 구현

String 클래스는 Object 클래스를 확장하여 사용한다. 
또한 **Serializable, Comparable<String>, CharSequence 인터페이스**를 구현한다.

#### 1) Seriablizable
해당 객체를 파일로 저장하거나 다른 서버에 전송 가능한 상태가 된다.

#### 2) Comparable<String>
이 인터페이스는 compareTo()라는 메소드 하나만 선언되어 있다. 이 메소드는 매개 변수로 넘어가는 객체와 현재 객체가 같은지를 비교하는 데 사용된다.

#### 3) CharSequnce
해당 클래스가 문자열을 다루기 위한 클래스이다.

## 2. null 체크
String 뿐만 아니라 모든 객체를 처리할 때, 널 체크는 반드시 필요하다.
객체가 널이라는 것은 객체가 아무런 초기화가 되어 있지 않으며, 클래스에 선언되어 있는 어떤 메소드도 사용할 수 없다는 것을 의미한다. 
다시 말해서, 널 체크를 하지 않으면 객체에 사용할 수 있는 메소드들은 모두 예외를 발생시킨다. **(NullPointException)**

## 3. 여러가지 메소드

String 클래스에는 여러가지 메소드들이 존재한다.

### 3-1. 내용을 비교하고 검색하는 메소드

* length(): 문자열의 길이 확인
* isEmpty(): 문자열이 비어 있는지 확인
* equals(): 문자열이 같은지 비교
* compareTo(): 문자열이 같은지 비교
* startsWith(): 매개 변수로 넘겨준 값으로 시작하는지 확인
* endsWith(): 매개 변수로 넘어온 값으로 끝나는지 확인

### 3-2. 위치를 찾아내는 메소드

* indexOf(): 객체의 특정 문자열이나 char가 있는 위치 확인
* lastIndexOf(): indexOf() 달리 뒤에서 부터 위치 확인

### 3-3. 값의 일부를 추출하기 위한 메소드

* charAt(): 특정 위치의 char 값을 반환
* copyValueOf(): char 배열에 있는 값을 문자열로 반환
* toCharArray(): 문자열을 char 배열로 변환하는 메소드
* substring(): 지정한 인덱스의 대상 문자열을 잘라 String으로 리턴
* split(): 정규 표현식에 맞추어 문자열을 잘라 String 배열로 리턴

### 3-4. 값을 바꾸는 메소드

* concat(): 매개 변수로 받은 str을 기존 문자열의 우측에 붙인 새로운 문자열 객체를 생성하여 리턴
* trim(): 문자열의 맨 앞과 맨 뒤에 있는 공백들을 제거한 문자열 객체를 리턴
* replace(): 해당 문자열에 있는 oldChar의 값을 newChar로 대치
* format(): format에 있는 문자열의 내용 중 변환해야 하는 부분을 args의 내용으로 변
* toLowerCase(): 모든 문자열의 내용을 소문자로 변경
* toUpperCase(): 모든 문자열의 내용을 대문자로 변경
* valueOf(): 기본 자료형을 문자열로 변환


## 4. StringBuffer, StringBuilder

String은 **immutable(불변의)한 객체**다. 다시 말해서 한 번 만들어지면 더 이상 그 값을 바꿀 수 없다. String 객체는 변하지 않는다. *우리가 문자열을 더하면 더해진 새로운 String 객체가 생성되고, 기존 객체는 버려진다*. 그러므로 계속 하나의 String을 만들어 계속 더하는 작업을 한다면, 계속 쓰레기를 만들게 된다.

그럼 StringBuffer와 StringBuilder는 무엇이 다른걸까?

### 4-1. 차이점

StringBuilder는 Thread safe하지 않고, StringBuffer는 Tread Safe하다.
속도는 StringBuilder가 더 빠르다.
두 메소드는 문자열을 더하더라도 새로운 객체를 생성하지 않는다. 
'+' 기호를 사용하는 것은 아니고 **append 메소드**를 통해 새로운 문자열을 더할 수 있다.

### 4-2. 공통점

String, StringBuilder, StringBuffer 클래스의 공통점은 모두 문자열을 다룬다는 것이다. 또한 CharSequnce 인터페이스를 구현했다.

### 4-3. 언제 사용할까?

**하나의 메소드 내에서** 문자열을 생성하여 더할 경우에는 *StringBuilder*를 사용해도 전혀 상관 없다. 하지만 어떤 클래스에 문자열을 생성하여 더하기 위한 문자열을 처리하기 위한 인터페이스 변수가 선언되었고, *여러 쓰레드에서 이 변수를 동시에 접근하는 일이 있을 경우*에는 반드시 StringBuffer를 사용해야 한다.
