# HashMap은 어떻게 동작하는가!
[참고](https://d2.naver.com/helloworld/831311)
HashMap은 Java Collections FrameWork에 속한 구현체 클래스

어떤 방식으로 HashMap 구현체의 성능을 향상시켰는지 소개할 예정이며 구체적인 내용은
Amortized Constant Time을 위해 어떻게 해시 충돌 가능성을 줄이고 있는가에 대한 것이다.

## HashMap과 HashTable

* 해시맵 : 해시맵(HashMap)은 키(key)와 값(value)을 쌍으로 저장하는 자료구조
Java에서는 java.util.HashMap 클래스를 통해 제공
* 해시맵은 내부적으로 해시 테이블(hash table)을 사용하여 데이터를 관리하며, 이 때문에 키에 대한 빠른 검색 속도를 제공
  (chat GPT)


해시맵과 해시테이블은 Java의 API의 이름이다. 

HashTable 또한 Map 인터페이스를 구현하고 있어 HashMap과 HashTable이 제공하는 기능은 같다고 
할 수 있다. 
HashMap은 보조 해시함수(Additional Hash Function)을 사용하기 때문에 이를 사용하지 않는 HashTable
보다 Hash 충돌이 덜 발생할 수 있어 성능상 이점이 있다.

**정의**
- 키에 대한 해시값을 사용하여 값을 저장하고 조회하며, 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 associate array
라고 할 수 있다. associate Array는 다른말로 Map, Dictionary, Symbol Table이 있다. 

HashTable과 HashMap의 선언

```java
public class 8ccce55530bc3477c678dd9921b60f3e.gifHashtable<K,V> extends Dictionary<K,V> implements Map<K,V>, Cloneable, java.io.Serializable { 
    
    public class 928b3cc3fe40d69cd06cbe7f5f3767f8.gifHashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {
```

associative array를 지칭하기 위하여 HashTable에서는 Dictionary라는 이름을 사용하고, HashMap에서는 Map을 사용한다. 

* Map이란 수학 함수에서의 대응 관계를 지칭하는 용어로, 경우에 따라서는 함수자체를 의미하기도 함.
* 키 집합인 정의역과 값 집합인 공역의 대응에 해시 함수를 이용하는 것. 

## 해시 분포와 해시 충돌 

동일하지 않은 객체 X, Y 가 있을때, 즉 X.equals(Y)가 거짓일 때, X.hasCode()! = Y.hashCode()가 같지않다면, 이때 사용하는 해시 함수는 완전한 해시 함수(perfect hash functions) 
라고 한다. 

boolean 같이 서로 구별되는 객체의 종류가 적거나, Integer, Long, Double 같은 넘버객체는 객체가 나타내려는 값 자체를 해시값으로 사용할 수 있기 때문에 완전한 해시 함수 대상으로 삼을 수 있다.
하지만, String이나 POJO(plain old java object)에 대하여 완전한 해시 함수를 제작하는 것은 사실상 불가능하다. 

적은 연산만으로 빠르게 동작할 수 있는 완전한 해시 함수가 있더라도, 그것을 HashMap에서 사용할 수 있는 것은 아니다. 
HashMap은 기본적으로 각 객체의 hashCode() 메서드가 반환하는 값을 사용하는 데, 결과 자료형은 int 이다.
32비트 정수 자료형으로는 완전한 자료형으로는 완전한 자료해시 함수를 만들 수 없다. 논리적으로 생성 가능한 객체의 수가 2^32 보다 많을 수 있기 때문이며, 모든 HashMap 객체에서
O(1) 을 보장하기 위해 랜덤 접근이 가능하게 하려면 원소가 2^32인 배열을 모든 HashMap이 가지고 있어야 하기 때문이다. 

따라서 HashMap을 비롯한 많은 해시 함수를 이용하는 association array 구현체 에서는 메모리를 절약하기 위해 실제 해시 함수의 표현 정수 범위 N 보다 작은 M개의 원소가 있는 배열만을 사용한다.

다음과 같이 객체에대한 해시코드의 나머지 값을 해시 버킷 인덱스 값으로 사용한다.

```java
int index = x.hashCode() % M;
```

위 코드와 같은 방식 사용시 서로다른 해시 코드를 가지는 서로다른 객체가 1/M의 확률로 같은 해시 버킷을 사용하게 된다. 
이는 해시 함수가 해시 충돌을 얼마나 회피하도록 잘 구현되었냐에 따라  상관없이 발생할 수 있다.

이렇게 해시 충돌이 발생하더라도 키-값 쌍 데이터를 잘 저장히고 조회할 수 있게 하는 방식에는 대표적으로 두가지가 있다

1. Open Adressing
2. Separate Chaining


```java
int index = X.hashCode % M;

A, B, C, D 순서로 HashMap에 삽입될 때
A, B, C, D애 대한 index 값이 차례로 0,1, 2M+1, M+1이라고 할 때,
```

<img src="https://ibb.co/TWSf9pn">

Open Addressing은 데이터를 삽입하려는 해시 버킷이 이미 사용중인 경우 다른 해시 버킷에 해당 데이터를
삽입하는 방식이며, 데이터를 저장하고 조회할 해시 버킷을 찾을 때에는 Linear Probing, Quadratic Probing등의 
방법을 사용한다.

Separate Chaining에서 각 배열의 인자는 인덱스가 같은 해시 버킷을 연결한 링크드 리스트의 첫부분(head) 이다.

두 방법 모두 워스트 case O(M)이다. 하지만, OpenAddressing은 연속된 공간에 데이터를 저장하기 때문에 Separate Chaining에 비하여
캐시 효율이 높다. 따라서 데이터 개수가 충분히 적다면 Open Addressing이 Separate Chaining보다 더 성능이 좋다. 하지만 배열의 크기가
커질수록 (M 값이 커질수록) 캐시 효율이라는 Open Addressing의 장점은 사라진다. 배열의 크기가 커지면, L1, L2캐시 적중률(hit ratio)이 낮아지기 
때문이다. 


Java HashMap에서 사용하는 방식은 Separate Chaining이다. 

Open Addressing은 데이터를 삭제할떼 처리가 효율적이기 어렵고, HashMap에서 remove() 메소드는 매우 빈번하게 호출될 수 있기 때문이다. HashMap에
저장된 키-값 쌍 개수가 일정 개수이상으로 많아지면, 일반적으로 Open Addressing은 Separate Chaining 보다 느리다. 

Open Addressing의 경우 해시버킷을 채운 밀도가 높아질수록 Worst Case 발생 빈도가 더 높아지기 때문이다. 반번 Separate Chaining 방식의 경우 
해시 충돌이 잘 발생하지 않도록 조정할 수 있다면 Worst Case 또는 Worst Case에 가까운 일이 발생하는 것을 줄일 수 있다. 

```java
transient Entry<K,V>[] table = (Entry<K,V>[]) EMPTY_TABLE;

static class Entry<K,V> implements Map.Entry<K,V> {
    final K key;
    V value;
    Entry<K,V> next;
    int hash;
    
    Entry(int h, K k, V v, Entry<K,V> n) {
        value = v;
        next = n;
        key = k;
        hash = h;
    }
    public final getKey(){...}
    public final V getValue(){...}
    public final boolean equals(Object o){...}
    public final int hashCode(){...}
    public final String toString(){...}
    
    void recordAcess(HashMap<K,V> m) {...}
    
    void recordRemoval(HashMap<K,V> m) {...}
    }
}
```

Separate Chaining 방식을 사용하기 때문에 put() 메서드 구현은

```java
public V put(K key, V value) {
        if (table == EMPTY_TABLE) {
        inflateTable(threshold); // 테이블 배열 생성
        }

        // HashMap에서는 null을 키로 사용
        if (key == null) {
        return putForNullKey(value);
        }

        // 해시 값을 계산
        int hash = hash(key);

        // i 값은 해시 버킷의 인덱스
        int i = indexFor(hash, table.length);

        // 해시 버킷에 있는 링크드 리스트를 순회
        // 같은 키가 이미 저장되어 있다면 값을 교체.
        for (Entry<K, V> e = table[i]; e != null; e = e.next) {
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
        V oldValue = e.value;
        e.value = value;
        e.recordAccess(this);
        return oldValue;
        }
        }

        // 해당 키-값 쌍 데이터가 아직 삽입되지 않았다면 새 Entry를 생성
        addEntry(hash, key, value, i);

        // 수정 횟수(modification)를 추적하기 위한 코드
        // ConcurrentModificationException을 발생시켜야 하는지 판단할 때 사용
        modCount++;

        return null;
        }
```
## 해시맵을 이용해 알고리즘 문제 해결





















