## 散列

散列又称Hash，在计算机中经常使用，相对于链表，树等数据结构，哈希表提供了快速的插入删除操作，且无论数据量有多大，时间复杂度都为O(1)。但Hash也存在一些问题，比如无法知道数据量而导致的数据空间浪费，或者是因为hash碰撞造成的效率损失。所以如何设计hash函数和采用何种扩容方法，是影响哈希表效率的重要因素。

解决hash冲突常用的方法是开放定址法和链地址法。开发地址法常用的策略又包括线性探测，平方探测，再哈希法等。链地址法则是将映射到同一地址的数据元素分别保存到散列表以外的各自线性表中。

散列函数的设计也特别关键，在选择散列函数时需要考虑的因数有计算散函数所需时间，关键字长度，散列表长度（散列表地址范围），记录的查找频率等因数。

`装填因子：a=n/m  其中n 为关键字个数，m为表长`装填因子是影响哈希表性能的一个重要指标。



实例代码实现了基于线性探测的hashTable。

```java
import static java.lang.Math.random;

// 基于线性探测的hashTable的实现
// 没有hashTable是否满的情况(在测试时避免)
// 装填因子：a=n/m  其中n 为关键字个数，m为表长。

public class HashApp {

    public static void main(String[] args) {

        DataItems aDataItem;
        int size = 20;   // hashtable的大小
        int n = 8;       // 数据项数目
        int key;

        HashTable theHashTable = new HashTable(size);

        for (int i = 0; i < n; i++) {
            key = (int)(random() * 99);
            aDataItem = new DataItems(key); // 随机生成数据项

            theHashTable.insert(aDataItem);
        }

        theHashTable.displayTable();

        // 插入数据项
        theHashTable.insert(new DataItems(666));
        theHashTable.displayTable();

        // 查找数据
        DataItems find = theHashTable.find(666);
        if(find != null){
            System.out.println("Find with key");
        }
        else
            System.out.println("Can't find");

        // 删除数据项
        theHashTable.delete(666);
        theHashTable.displayTable();


    }
}


class DataItems{
    private int iData;

    public DataItems(int id){
        iData = id;
    }

    public int getKey(){
        return iData;
    }
}

class HashTable{

    private DataItems[] hashArray;
    private int arraySize;
    private DataItems noItems;

    public HashTable(int size){
        arraySize = size;
        hashArray = new DataItems[arraySize];
        noItems = new DataItems(-1);
    }

    public void displayTable(){
        System.out.print("Table: ");
        for (int i = 0; i < arraySize; i++) {

            if(hashArray[i] != null){
                System.out.print(hashArray[i].getKey() + " ");
            }
            else
                System.out.print("** ");
        }
        System.out.println();
    }


    public int hashFunc(int key){

        return key % arraySize;  // hash 函数

    }

    public void insert(DataItems item){
        int key = item.getKey();
        int hashVal = hashFunc(key); //关键值的hash映射结果

        while (hashArray[hashVal] != null && hashArray[hashVal].getKey() != -1){
            ++hashVal;
            hashVal %= arraySize;
        }

        hashArray[hashVal] = item;

    }

    public DataItems delete(int key){
        int hashVal = hashFunc(key);

        while(hashArray[hashVal] != null){
            if (hashArray[hashVal].getKey() == key){

                DataItems temp = hashArray[hashVal];
                hashArray[hashVal] = noItems;  // 删除置为-1

                return temp;

            }
            ++hashVal;
            hashVal %= arraySize;
        }

        return null;
    }

    public DataItems find(int key){
        int hashVal = hashFunc(key);

        while (hashArray[hashVal] != null){

            if(hashArray[hashVal].getKey() == key){
                return hashArray[hashVal];
            }

            ++hashVal;
            hashVal %= arraySize;
        }

        return null;

    }
}

```

