## 栈和队列

栈和队列是程序中最常使用的数据结构，它们的实现最常用机制是基于数组，相对于其它数据结构而言是最简单也是最基础的数据结构。

**栈(Stack)**

一种特殊的线性表，插入和删除在同一端，该端叫栈顶（top），另一端叫栈底（bottom）。后进先出（LIFO），故也称为后进先出的线性表。

```java
public class StackApp {

    public static void main(String[] args) {
        StackX stackX = new StackX(10);

        System.out.print("进栈：");
        for (int i = 0; i < 12 ; i++) {
            if(!stackX.isFull()){
                System.out.print(i + " ");
                stackX.push(i);
            }
        }

        System.out.println("\n栈顶数据项：" + stackX.peek());
        System.out.print("\n出栈：");
        for (int j = 0; j < 10 ; j++) {
            if(!stackX.isEmpty()){
                System.out.print(stackX.pop() + " ");
            }
        }
    }
}


// 定义栈结构
class StackX{
    private int maxSize;
    private int[] data;
    private int top;

    public StackX(int size){
        maxSize = size;
        data = new int[maxSize];
        top = -1;
    }

    public void push(int p){
        data[++top] = p;
    }

    public int pop(){
        return data[top--];
    }

    public int peek(){
        return data[top];
    }

    public boolean isEmpty(){
        return (top == -1);
    }

    public boolean isFull(){
        return (top == maxSize -1);
    }

}

```



**队列(Queue)**

一种操作受限的线性表，只允许在表的一端进行插入，而在表的另一端进行删除。先进先出(FIFO)，故又称先进先出的线性表。

队列可以实现为循环队列，将数组下标从数组末端绕回到数组的开始位置。

```java

public class QueueApp {

    public static void main(String[] args) {

        Queue queue = new Queue(10);

        System.out.print("插入队列：");
        for (int i = 0; i < 10; i++) {
            if (!queue.isFull()){
                System.out.print(i + " ");
                queue.insert(i);
            }
        }

        System.out.println("\n队列长度：" + queue.size());
        System.out.println("队列头元素： " + queue.peekFront());

        System.out.print("\n移除队列：");
        while (!queue.isEmpty()){
            System.out.print(queue.remove() + " ");
        }

        System.out.println("\n队列长度：" + queue.size());
    }
}


class Queue{
    private int maxSize;
    private int[] data;
    private int front;
    private int rear;
    private int index;

    public Queue(int size){
        maxSize = size;
        data = new int[maxSize];
        front = 0;
        rear = -1;
        index = 0;
    }

    public void insert(int value){
        if(rear == maxSize -1){
            rear = -1;
        }
        data[++rear] = value; // 尾位置填数据
        index++;
    }

    public int remove(){
        int temp = data[front++]; // 头位置取数据
        if(front == maxSize){
            front = 0;
        }

        index--;
        return temp;
    }

    public int peekFront(){
        return data[front];
    }

    public boolean isEmpty(){
        return (index == 0);
    }

    public boolean isFull(){
        return (index == maxSize);

    }

    public int size(){
        return index;
    }
}
```



**算术表达式**

算术表达式最常用的是中缀表达式，另外还有前缀表达式和后缀表达式。和以后要讲的树的中序，前序和后序遍历差不多是一个概念。下表给出几个中缀表达式对应的后缀表达式的例子，举一反三，就能推广到其他情况了。

| 中缀表达式  | 后缀表达式 |
| ----------- | ---------- |
| A+B-C       | AB+C-      |
| A*B/C       | AB*C/      |
| A+B*C       | ABC*+      |
| A*(B+C)     | ABC+*      |
| (A+B)*(C-D) | AB+CD-*    |

算术表达式求值通常是先装换成后缀表达式，然后在求后缀表达式的值，在这个过程中栈都是很好用的工具。











