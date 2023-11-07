# JAVA 8에서는 더 발전된 방식을 사용한다

## JAVA 8 HashMap에서의 Separate Chaining

* 만약 객체의 함수 함수 값이 균등 분포(uniform distribution) 상태라고 할떼, get() 메서드 호풀에 대한 기댓값은 E(N/M)이다. 
그러나, JAVA 8에서는 이보다 더 나은 E(log(N/m)) 을 보장한다. 이유는 데이터의 개수가 많아지면, Separate Chaining에서 링크드 리스트 대신 트리를 사용하기 때문이다.

--> 데이터의 개수가 일정 개수 이상일 때는 링크드 리스트보단 트리를 이용하는 것이 성능상 이점이 있다.

##### 링크드리스트를 사용할 것인가 트리를 사용할 것인가?
- 기준은 하나의 해시 버킷에 할당된 키 -  값 쌍의 개수이다.
- 자바 8에서는 상수 형태로 기준을 정하고 있으며, 즉 하나의 해시 버킷에 8개의 키 값-쌍이 모이면 링크드리스트를 트리로 변경한다.
- 만약 해당 버킷에 있는 데이터를 삭제하여 개수가 6개에 이르면 다시 링크드 리스트로 변경한다. 
 트리는 잉크드 리스트 보다 메모리 사용량이 많고, 데이터의 개수가 적을때,트리와 킹크드 리스트의 Worst Case 수행 시간 차이 비교는 의미가 없기
 때문이다.

*8과 6으로 2이상의 차이를 둔 이유*
-> 만약 차이가 1이라면, 어떤 한 키-값 쌍이 반복되어 삽입/삭제 되는 경우 불필요하게 트리와 링크드 리스트를 변경하는 일이 반복되어 성능저하가 
발생할 수 있기 때문.

```java
static final int TREEIFY_THRESHOLD = 8;

static final int UNTREEIFY_THRESHOLD = 6;
```

##### 자바 8의 HashMap에서는 Entry 클래스 대신 Node 클래스를 사용한다.
 Node 클래스 자체는 자실상 java 7의 Entry 클래스와 내용이 같지만 링크드 리스트 대신 트리를 사용할 수 있도록 하의 클래스인 TreeNode가 있다는 것이
 java 7의 HashMap과 다르다.
 
이때 사용하는 트리는 Red-Black Tree인데 트리 순회 시 사용하는 대소 판단 기준은 해시 함수값이다. 해시 값을 대소 판단 기준으로 사용하면 Total Ordering
에 문제가 생기는데, 이를 java 8 HashMap 이를 tieBreakOrder() 메서드로 해결한다.

```java
transient Node<K,V>[] table;


static class Node<K,V> implements Map.Entry<K,V> {  
  // 클래스 이름은 다르지만, Java 7의 Entry 클래스와 구현 내용은 같다. 
}


// LinkedHashMap.Entry는 HashMap.Node를 상속한 클래스다.
// 따라서 TreeNode 객체를 table 배열에 저장할 수 있다.
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {


        TreeNode<K,V> parent;  
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;   

        // Red Black Tree에서 노드는 Red이거나 Black이다.
        boolean red;

        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }

        final TreeNode<K,V> root() {
        // Tree 노드의 root를 반환한다. 
        }

        static <K,V> void moveRootToFront(Node<K,V>[] tab, TreeNode<K,V> root) {
        // 해시 버킷에 트리를 저장할 때에는, root 노드에 가장 먼저 접근해야 한다.
        }


        // 순회하며 트리 노드 조회 
        final TreeNode<K,V> find(int h, Object k, Class<?> kc) {}
        final TreeNode<K,V> getTreeNode(int h, Object k) {}


        static int tieBreakOrder(Object a, Object b) {
         // TreeNode에서 어떤 두 키의comparator 값이 같다면 서로 동등하게 취급된다.
         // 그런데 어떤 두 개의 키의 hash 값이 서로 같아도 이 둘은 서로 동등하지 
         // 않을 수 있다. 따라서 어떤 두 개의 키에 대한 해시 함수 값이 같을 경우, 
         // 임의로 대소 관계를 지정할 필요가 있는 경우가 있다. 
        }


        final void treeify(Node<K,V>[] tab) {
          // 링크드 리스트를 트리로 변환한다.
        }



        final Node<K,V> untreeify(HashMap<K,V> map) {
          // 트리를 링크드 리스트로 변환한다.
        }

        // 다음 두 개 메서드의 역할은 메서드 이름만 읽어도 알 수 있다.
        final TreeNode<K,V> putTreeVal(HashMap<K,V> map, Node<K,V>[] tab,
                                       int h, K k, V v) {}
        final void removeTreeNode(HashMap<K,V> map, Node<K,V>[] tab,
                                  boolean movable) {}


        // Red Black 구성 규칙에 따라 균형을 유지하기 위한 것이다.
        final void split (…)
        static <K,V> TreeNode<K,V> rotateLeft(…)
        static <K,V> TreeNode<K,V> rotateRight(…)
        static <K,V> TreeNode<K,V> balanceInsertion(…)
        static <K,V> TreeNode<K,V> balanceDeletion(…)



        static <K,V> boolean checkInvariants(TreeNode<K,V> t) {
        // Tree가 규칙에 맞게 잘 생성된 것인지 판단하는 메서드다.
        }
    }
```

## 해시 버킷 동적 확장

해시 버킷의 개수가 적다면, 메모리 사용을 아낄 수 있지만 해시 충돌로 인해 성능상 손실이 발생
--> 그래서 HashMap은 키 - 값 쌍 데이터 개수가 일정 개수 이상이 되면 해시 버킷의 수를 두배로 늘린다. 
이렇게 해시 버킷 수를 늘리면 (N/M) 값도 작아져 해시 충돌로 인한 성능 손실 문제를 어느정도 해결할 수 있다. 

해시 버킷 수의 기본값은 16이고, 데이터의 개수가 임계점에 이를때마다 해시 버킷의 크기를 두 배 씩 등가 시킨다. 버킷의 최대 수 는 2^30개다.

그런데 버킷 개수가 2배로 증가할 때마다 모든 키-값 데이터를 읽어 새로운 Separate Chaining을 구성해야 하는 문제가 있다. HashMap 생성자의 인자로
초기 해시 버킷개수를 지정할 수 있으므로, 해당 HashMap 객체에 저장될 데이터의 개수가 어느정도인지 예측 가능한 경우에는 이를 생성자의 인자로 저장하면 
불필요하게 Separate Chaining 을 재구성하지 않게 할 수 있다.
```java
// 인자로 사용하는 newCapacity는 언제나 2a이다.
void resize(int newCapacity) {  
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;

        // MAXIMIM_CAPACITY는 230이다.
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }

        Entry[] newTable = new Entry[newCapacity];


        // 새 해시 버킷을 생성한 다음 기존의 모든 키-값 데이터들을
        // 새 해시 버킷에 저장한다.
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        table = newTable;
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
    }


    void transfer(Entry[] newTable, boolean rehash) {
        int newCapacity = newTable.length;
        // 모든 해시 버킷을 순회하면서
        for (Entry<K,V> e : table) {
            // 각 해시 버킷에 있는 링크드 리스트를 순회하면서
            while(null != e) {
                Entry<K,V> next = e.next;
                if (rehash) {
                    e.hash = null == e.key ? 0 : hash(e.key);
                }
                // 해시 버킷 개수가 변경되었기 때문에
                // index 값(hashCode % M)을 다시 계산해야 한다. 
                int i = indexFor(e.hash, newCapacity);
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            }
        }
    }
```


해시 버킷 크기를 두배로 확장하는 임계점은 현재의 데이터 개수가 ***'load factor * 현재의 해시 버킷 개수'*** 에 이를때 이고, load factor는 0.75이다. 
이 load factor 또한 HashMap의 생성자에서 지정할 수 있다. 임계값에 이르면 항상 해시 버킷 크기를 두 배로 확장하기 때문에, N개의 데이터를 삽입했을때
의 키-값 쌍 접근 횟수는

<img src="https://d2.naver.com/content/images/2015/06/helloworld-831311-9.png">
로 분석된다.

즉, 기본생성자로 생성한 HashMap을 이용하여 많은 양의 데이터를 삽입시, 최적의 해시 버킷 개수를 지정한 것보다 2.5배 ㅁ많이 키-값 쌍에 접근해야 한다. 
이는 곧 수행시간이 2.5배 늘어남을 의미하고, 따라서 성능 향상을 위해서는 HashMap 겍체를 생성할때, 적정한 버킷 개수를 지정해야 한다. 

이렇게 해시 버킷 크기를 두배로 확장하는 것에는 결정적인 문제가 있다. 해시 버킷의 개수 M이 2^a 형태가 되기 때문에 
index = X.hashCode() % M을 계산할 때 X.hashCode()의 하위 a개의 비트만 사용하게 된다는 것이다. 즉 해시 함수가 32비트 영역을 고르게
사용하도록 만들었다 하더라도 해시 값을 2의 승수로 나누면 해시 충돌 발생 가능성이 높다. 

이 때문에 보조 해시 함수가 필요하다.

 