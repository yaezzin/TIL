## instanceof 연산자

참조변수가 참조하고 있는 인스턴스의 실제 타입을 알기 위해 instanceof 연산자를 사용한다.

```java
void doWork() {
    if (c instanceof FireEngine) {
        FireEngine fe = (FireEngine) c;
        fe.water();
    }

    if (c instanceof Ambulance) {
        Ambulance a = (Ambulance) c;
        a.water();
    }
}
```

* 위의 코드는 Car타입의 참조변수 c를 매개변수로 하는 메서드이다.
* 이메서드가 호출될 때 Car클래스 또는 그 자손 클래스의 인스턴스를 넘겨받겠지만 메서드 내에서는 어떤 인스턴스인지 정확히 알 수 없다.
* 그러므로 instanceof 연산자를 사용하여 ```참조변수가 가리키고 있는 인스턴스 타입을 체크```하고 적절히 형변환을 한 후에 작업을 해야한다.

```java
Tv      t = new CaptionTv(); // 조상 클래스 타입으로 자손 클래스의 인스턴스를 참조
Caption c = new CaptionTv();
```
* 위와 같이 조상 타입의 참조변수로 자손타입의 인스턴스를 참조할 수 있기 때문에 참조변수의 타입과 인스턴스의 타입이 항상 일치하지 않는다.
* 조상 타입의 참조변수로는 실제 인스턴스의 멤버들을 모두 사용할 수 없기 때문에 실제 인스턴스와 같은 타입의 참조변수로 형변환을 해주어야 인스턴스의 모든 멤버들을 사용할 수 있다!
