# extends와 Implements  

## extends 

extends는 상속의 대표적인 형태이다.  
부모 클래스에 구현된 메서드를 오버라이딩할 필요 없이 직접 사용할 수 있다.

```java
class Vehicle {
  protected int speed = 3;
  
  public int getSpeed(){
    return speed;
  }
  public void setSpeed(int speed){
    this.speed = speed;
  }
}

class Car extends Vehicle{
  public void printspd(){
    System.out.println(speed);
  }
}

public class ExtendsSample {
  public static main (String[] args){
    Car A = new Car();
    System.out.println(A.getSpeed());
    A.printspd();
  }
}
```
위의 코드를 보면 
* Car 클래스는 Vehicle 부모 객체를 상속받았고, 그 안에 부모 클래스에 없는 메서드를 새로 생성하였다.
* 또한 부모 클래스에 있는 메서드를 오버라이딩하여 재정의하지도 않았다.
* main을 살펴보면 Car 객체를 생성해주고, 부모에 있는 메서드인 getSpeed를 직접 사용하였다. (getSpeed 메서드는 Vechle 클래스에만 존재하는 speed 변수를 사용한다. -> 즉, extends를 통해 메서드 뿐아니라 변수에 대한 접근도 가능해진다.)

> 중요한 것은 자바는 다중상속을 지원하지 않는다.
* 다중상속을 지원하지 않는다는 것은 부모가 여러명일 수 없다는 것이다.
* ```extends Parent1, Parent2, ... ParentN``` 이 불가능하다는 것
* 다중상속이 불가능한 것을 해결하기 위해서 ```인터페이스```가 등장하였다.

## implements

```java
interface TestInterface{
  public static int num = 8;
  public void fun1();
  public void fun2();
}

class InterfaceExam implements TestInterface{
  @Override
  public void fun1(){
    System.out.println(num);
  }
  
  @Override
  public void fun2() {
    
  }
}

public class InterfaceSample{
  public static void main(String args[]){
    InterfaceExam exam = new InterfaceExam();
    exam.fun1();
  }
}
```

위의 코드를 살펴보면 interfaceExam은 TestInterface를 상속받았다.
* extends와 달리 implements는 부모 클래스에 있는 메서드들을 오버라이딩 해주어야 한다는 점이다.
* 이렇게 재정의를 해주어야하는데 왜 인터페이스를 상속받을까? 의문이 생기게 된다.

## 인터페이스 사용 이유

### 1. 다중 상속의 이점

위에도 설명했다시피 extends를 이용한 클래스 상속은 다중상속이 불가능하다. 그래서 다중상속을 위해 인터페이스를 사용하게 된다.

```java
public class HashMap<K,V> extends AbstractMap<K,V>
  implements Map<K,V>, Cloneable, Serializable {
}
```
* HashMap 클래스의 경우 기본적으로 Map의 구조를 가지기 때문에 Map 인터페이스를 구현하였다.
* 또한 Cloneable로 인스턴스가 복제 가능하도록 하였고 Serializable로 직렬화할 수 있도록 하였다.
* 이처럼 필요한 인터페이스를 상속받아서 구현하면 여러 기능들을 구조적으로 포함할 수 있게 된다.

### 2. 협업의 이점

개발자A가 만들고 있는 A클래스는 개발자B가 만들고 있는 B클래스가 완성이 되어야지 개발 진행이 가능한 경우가 많다.
* 이러한 경우 개발자A는 클래스B의 더미(dummy) 클래스라는 가짜 혹은 임시 클래스를 만들어서 클래스B가 필요한 자리에 채워 넣은 뒤, 개발자B가 클래스B를 완성한 후에 바꿔주게 된다. 
* 이때 B클래스에는 여러 기능을 하는 메서드들이 존재할 텐데 이 메서드들의 틀 나타내는 것이 인터페이스이다.
* 인터페이스로 메서드들의 형식을 잡아놓지 않는다면, 개발자들 간의 형식이 맞지 않아서 많은 양을 리팩터링 해야 하는 경우가 발생할 수 있기 때문에 인터페이스를 사용하게 된다.


## 참고자료
* https://reinvestment.tistory.com/48
* https://velog.io/@hkoo9329/%EC%9E%90%EB%B0%94-extends-implements-%EC%B0%A8%EC%9D%B4
