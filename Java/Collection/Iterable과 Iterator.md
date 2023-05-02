## Iterable

> Collection의 상위 인터페이스

```java
public interface Collection<E> extends Iterable<E> {
    ...
}
```

자바의 컬렉션 프레임워크는 컬렉션의 저장된 요소들을 읽어오는 방법으로 Iterable 인터페이스를 표준화하고있다.

```java
public interface Iterable<T> {    
    Iterator<T> iterator();

    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }

    default Spliterator<T> spliterator() {
        return Spliterators.spliteratorUnknownSize(iterator(), 0);
    }
}
```

* Iterable 인터페이스는 ```Iterator```와 ```for-each loop```을 제공한다.
* Iterable 인터페이스의 역할은 iterator() 메소드를 하위 클래스에서 구현하도록 강제하기 위함이다.

## Iterator
