## Comparator와 Comparable

```java
public interface Comparator {
    int compare(Object o1, Object o2);
    boolean equals(Object obj);
}

public interface Comparable {
    public int compareTo(Object o);
}
```
* [Java API - Comparable]([docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#method.summary](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#method.summary)) & [Java API - Comparator](docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#method.summary)

> Comparable과 Comparator는 모두 ```인터페이스```로 컬렉션을 ```정렬```하는데 필요한 메서드들을 정의하고 있다.
* 두 인터페이스를 사용하고자 한다면 인터페이스 내에 선언된 메소드를 **반드시 구현**해야한다.
* ```공통점```
  * compare()과 compareTo()는 선언형태와 이름만 다를 뿐 두 객체를 비교한다는 점에서는 동일하다.
* ```차이점```
  * Comparable은 **자기 자신과 매개변수 객체를 비교**하는 것이고, Comparator는 **두 매개변수 객체를 비교**한다.
  * Comparable은 lang패키지에 있기 때문에 import를 해줄 필요가 없지만, Comparator는 util패키지에 있다.
   
``` 
  여기서 의문 !! Java API 문서를 보면 여러개의 메서드들이 존재하는데 왜 compareTo(), compare()만 구현하는 것이지?
  - 문서를 잘 살펴보면 default, static이 붙은 메서드들은 { ... } 블럭 내부가 구현 되어있는 것을 볼 수 있다. 
  - default나 static으로 선언된 메소드가 아니면 이는 추상메소드라는 의미로 반드시 재정의를 해주어야 한다는 것이다.
  - default와 static의 차이라면 default로 선언된 메소드는 재정의(Override)를 할 수 있고, static은 재정의를 할 수 없다는 차이다. 
  ```
   
## Comparable

> 자기 자신과 매개변수 객체를 비교

* Comparable을 사용하는것은 객체의 ```정렬기준```을 만들기 위해서다.
* 만약 String 자료의 정렬을 위해 배열을 만들고 Arrays.sort()메서드를 사용하면 사전순서대로 정렬할 수 있다.
* 하지만 객체는 정렬기준이 없기 때문에 Comparable 인터페이스를 객체의 클래스에서 구현하여 정렬기준을 만들어주어야 한다.


```java
public class ClassName implements Comparable<T> {
    /* 필수적으로 구현*/
    @Override
    public int compareTo(Type o) {
        // 비교를 위한 구현 필요
    }
}
```
* Comparable<T>에는 하나의 객체 타입을 지정해주어야 한다.
* compareTo() 메서드는 우리가 객체를 비교한 기준을 정의해주는 부분이다.

```java
class Student implements Comparable<Student> {
    int age;                 // 나이
    int classNumber; // 학급
   
   Student(int age, int classNumber) {
	this.age = age;
	this.classNumber = classNumber;
    }

    @Override
    public int compareTo(Student o) {
        if(this.age > o.age) {
            return 1;
        }
	
        else if(this.age == o.age) {
	    return 0;
	}
	
        else {
	    return -1;
	}
    }
}
```
Student 클래스 간의 나이를 비교하는 예시를 보자.
* compareTo()는 자기자신과 파라미터로 들어온 객체를 비교하여 상대방과의 차이 값을 비교하여 반환한다.
* compareTo()는 int 값을 반환하도록 되어있는데, 다음과 같은 기준으로 반환한다.
    * 두 객체가 같으면 0
    * 비교하는 값보다 작으면 음수 (일반적으로 -1 리턴)
    * 비교하는 값보다 크면 양수 (일반적으로 1 리턴)
* 이렇게 정렬의 대상이 되는 객체의 클래스에 Comparable 인터페이스를 구현하면, 객체들의 정렬기준을 정할 수 있다.

## Comparator

> 두 매개변수 객체를 비교

* 정렬 가능한 클래스(Comparable 인터페이스를 구현한 클래스)들의 기본 정렬 기준과 다르게 정렬 하고 싶을 때 사용하는 인터페이스

```java
public class ClassName implements Comparator<T> {
    /* 필수적으로 구현*/
    @Override
    public int compareTo(Type o) {
        // 비교를 위한 구현 필요
    }
}
```
* compare() 메서드는 매개변수로 받은 2개의 객체가 가진 값을 비교한다.

```java
class Student implements Compartor<Student> {
    int age;                 // 나이
    int classNumber; // 학급
   
   Student(int age, int classNumber) {
	this.age = age;
	this.classNumber = classNumber;
    }

    @Override
    public int compareTo(Student s1, Student s2) {
        if(s1.age > s2.age) {
            return 1;
        }
	
        else if(s1.age == s2.age) {
	    return 0;
	}
	
        else {
	    return -1;
	}
    }
}

class AgeDesc implements Comparator<Student> {
    @Override
    public int compare(Student s1, Student s2) {
        return s1.compareTo(s2) * -1;
    }
}

class ClassNumDesc implements Comparator<Student> {
    @Override
    public int compare(Student s1, Student s2) {
        return s1.compareTo(s2) * -1;
    }
}
```
* 이미 오름차순으로 정렬기준이 잡혀있는 comepareTo() 메서드의 결과에 -1을 곱하면 내림차순으로 쉽게 구현할 수 있다.
* 이처럼 이미 정해진 정렬기준 외 다른 정렬기준을 사용하고 싶다면 Comparator의 구현체를 사용하여 여러가지 정렬기준을 사용할 수 있다.
  * Collections.sort(list, new AgeDesc());처럼 매개변수로 정렬기준을 넘겨주어야 한다.



## 참고자료
* [Comparable 과 Comparator의 이해](https://st-lab.tistory.com/243)
* [Comparable 과 Comparator 차이](https://velog.io/@ovan/Comparable-Comparator)
