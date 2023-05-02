# this()

this()는 클래스의 생성자 **오버로딩시 반복되는 소스를 줄이기 위해서** 사용한다.
* this()는 같은 클래스 내의 다른 생성자를 호출하며, 생성자 내에서만 사용이 가능하다.
* 한 생성자에서 다른 생성자를 호출할 때는 반드시 첫줄에서만 호출이 가능하다.
    > 생성자 내에서 초기화 작업 도중 다른 생성자를 호출하게 되면 호출된 다른 생성자 내에서도 멤버 변수의 값을 초기화 해버리므로 
    > 다른 생성자를 호출하기 전의 작업이 무의미해지기 때문
 
* 주의할 점은 생성자 코드블록 내부의 this() 위에 다른 소스코드가 존재해서는 안된다는 것이다.

```text
# 오버로딩
매개변수의 개수 또는 타입을 다르게 하여 같은 이름의 메소드 및 생성자를 중복해서 정의하는 것
```

## 예제

```java
public class Car {
    String company = "현대";
    String model;
    String color;
    int maxSpeed;

    public Car() {

    }

    public Car(String model) {
        this(model, "검정색", 10);
    }

    public Car(String model, String color) {
        this(model, color, 10);
    }

    public Car(String model, String color, int maxSpeed) {
        this.model = model;
        this.color = color;
        this.maxSpeed = maxSpeed;
    }

    public String printCar() {
        return company + ", " + model + ", " + color + ", " + maxSpeed;
    }
}

public class CarExam {

    public static void main(String[] args) {
        Car c1 = new Car();
        Car c2 = new Car("제네시스");
        Car c3 = new Car("그렌져", "흰색");
        Car c4 = new Car("제네시스", "흰색", 20);
        
        System.out.println(c1.printCar());
        System.out.println(c2.printCar());
        System.out.println(c3.printCar());
        System.out.println(c4.printCar());
    }
}
```

<img width="200" alt="스크린샷 2022-08-17 오후 5 30 14" src="https://user-images.githubusercontent.com/97823928/185072481-a2f16fb4-4153-4fbf-b18b-41a186ede1f8.png">

# this 

this는 객체 내부에서 **객체 자신을 지칭**하고 싶을 때 사용한다.
* ```지역변수(메서드에 선언된 변수)```와 ```멤버변수(클래스 내에 선언된 변수)```를 구별해야할 때 사용한다.
* 매개변수와 객체 자신이 가지고 있는 변수의 이름이 같은 경우 이를 구분하기 위해 멤버변수에 this를 사용

```java
class Example {

    public int a;
    public int b;

    public Example() {
        a = 100;
        b = 200;
    }

    public Example(int a) {
        a = a; #여기를 this.a = a;로 변경해주어야 한다.
    }
}
```
a = a 는 a를 a를 대입하는 것이니까 맞지 않나? 의문이 들 수 있다.
하지만 파라미터로 받은 a와 클래스에 존재하는 멤버변수 a와 이름이 서로 같기 때문에 에러가 발생하게 된다.
이 에러를 해결하려면 this를 사용하여 클래스에 존재하는 멤버변수를 가리키면 된다. 
이로써 멤버변수 a와 파라미터 a를 구별 가능하게 되어진다.

결국 this.a = a에서
* 대입연산자 왼쪽의 this.a는 클래스에 선언된 public int a와 
* 대입연산자 오른쪽의 a는 메서드의 파라미터와 같다고 생각하면 된다. 


## 참고자료
* https://dustink.tistory.com/41

