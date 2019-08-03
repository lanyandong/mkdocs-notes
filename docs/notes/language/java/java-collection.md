### Java 容器类库

Collection 和Map 是Java容器类库的两种主要类型，最主要的区别在于Collection保存的是单个元素，而Map保存的是一个键值对。

下面例子展示了一些基本类型的容器，第一个 fill() 可以用于所用类型的Collection，这些类型都实现了用来添加新元素的 add() 方法。而第二个 fill()  使用与Map，它们都实现了添加键值对的 put()  方法。

```java
import java.util.*;
// 容器的打印
public class PrintingContainers {

    static Collection fill(Collection<String> collection){
        collection.add("rat");
        collection.add("cat");
        collection.add("dog");
        collection.add("dog");

        return collection;
    }

    static Map fill(Map<String, String> map){
        map.put("rat", "Fuzzy");
        map.put("cat", "Rags");
        map.put("dog", "Bosco");
        map.put("dog", "Spot");

        return map;
    }


    public static void main(String args[]){
        // List 类型
        System.out.println(fill(new ArrayList<String>()));
        System.out.println(fill(new LinkedList<String>()));

        // Set 类型
        System.out.println(fill(new HashSet<String>()));
        System.out.println(fill(new TreeSet<String>()));
        System.out.println(fill(new LinkedHashSet<String>()));

        // Map 类型
        System.out.println(fill(new HashMap<String, String>()));
        System.out.println(fill(new TreeMap<String, String>()));
        System.out.println(fill(new LinkedHashMap<String, String>()));

    }
}
```

`ArrayList` 和 `LinkedList` 都是List类型，都是按照插入了顺序保存元素，`ArrayList `可以理解为动态数组，`LinkedList`可以理解为链表，两者最主要的区别在于执行某些类型的操作时的性能。

`HashSet`，`TreeSet` 和`LinkedHashSet`都是Set类型，在Set类型中每个元素只保留一次，不会出现相同的项，它们的一个主要区别是存储元素的方式不同，`HashSet`是以散列的形式保存元素，所以是无序的，而`TreeSet`是以树的形式存储数据，是按照一定的顺序要求保存的。而LinkedHashSet则是按照添加的顺序保存对象的。

`HashMap`，`TreeMap`和`LinkedHashMap`都是Map类型，保存都是键值对（key—value），`HashMap`没有按照任何明显的顺序来保存其元素，但查找效率是比较快的，它有自己的算法来控制顺序，`TreeMap`则是按照某种特定顺序来保存键的，`LinkedHashMap`则是按照插入顺序保存键，并且保存了`HashMap`的查询速度。

