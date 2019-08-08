## 快速排序

快速排序应该是排序算法中比较复杂的一种，也是性能相对来说做好的一种，平均时间复杂度为O(n logn)。快速排序实现起来比较困难的是枢纽(哨兵)的选择和数组的划分。快速排序同样使用了递归，划分的两部分数组分别调用自身。

数组划分主要是根据选取的枢纽，使得数组前半部分不大于枢纽值，后半部分不小于枢纽值。枢纽的选择是影响算法的效率的一个重要因数，简单版本中的快熟排序常将子数组最右端的数据项作为枢纽。但这也可能造成算法效率为O(n^2)的情况(逆序排序)。

高级快速排序中使用三数据取中(子数组中第一个，最后一个和中间一个数据的中值)选取枢纽，有效的解决了算法效率为O(n^2)的问题。由于比较实现相对比较困难，所以实例代码仅实现了简单版本的快速排序。

```java
package algorithm;

import static java.lang.Math.random;

//实现快速排序
public class QuickSort {

    public static void main(String args[]){

        System.out.println("快速排序例子");
        int maxSize = 16;
        QuickSortArr arr = new QuickSortArr(maxSize);

        for(int i = 0; i < maxSize; i++){
            long value = (int)(random() * 99);
            arr.insert(value);
        }

        arr.display();
        arr.quickSort();
        arr.display();
    }
}


class QuickSortArr{

    private long theArray[];
    private int index;

    public QuickSortArr(int size){
        theArray = new long[size];
        index = 0;
    }

    public void insert(long v){
        theArray[index] = v;
        index++;
    }

    public void display(){

        System.out.print("the array: ");
        for(int i = 0; i < theArray.length; i++){
            System.out.print(theArray[i] + " ");
        }
        System.out.println();
    }

    public void quickSort(){
        recQuickSort(0, index - 1);
    }

    private void recQuickSort(int left, int right) {
        if(right - left <= 0){
            return;
        }
        else {
            long flag = theArray[right]; // 选最右的为flag
            int ptt = partitionIt(left, right, flag);

            recQuickSort(left, ptt - 1 );
            recQuickSort(ptt + 1, right);
        }
    }

    private int partitionIt(int left, int right, long flag) {
        int leftPtr = left - 1;  // 数组下标指针
        int rightPtr = right;

        while(true){
            // 左移/右移，找到不满足条件的(先运算在比较)
            while (theArray[++leftPtr] < flag);

            while (theArray[--rightPtr] > flag && rightPtr > 0);
            // 交换
            if(leftPtr < rightPtr){
                swap(leftPtr,rightPtr);

            }
            else
                break;
        }

        swap(leftPtr, right); // 将flag值移动到leftPtr的位置,并返回下标
        return leftPtr;
    }

    private void swap(int index1, int index2) {
        long temp = theArray[index1];
        theArray[index1] = theArray[index2];
        theArray[index2] = temp;
    }
}
```

