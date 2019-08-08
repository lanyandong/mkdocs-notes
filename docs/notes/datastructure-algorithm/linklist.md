## 链表

链表主要还是基于数组，实现起来也并不是很难，但链表的结构有很多，包括单链表，双端链表，以及双链表。这取决于构成链的节点结构。单链表只允许在表头插入数据项，而双端链表可以在表头和表尾插入数据项，但它们对于数据的变量访问都只能顺序遍历，但双向链表允许反向遍历。

在链表中，数据的插入和删除操作都很快，只需要改变一些引用的值就行了，时间复杂度为O(1), 但是查找数据并不容易，需要一次访问链表中的每一项数据（平均N/2），时间复杂度为O(N)。

注：有序数组查找(利用二分查找)数据所需时间为O(logN)，插入删除数据项时，数据需要移动，这是很费时的。

实例代码实现了单链表结构，并定义了查找删除方法。

```java
package datastructure;

public class LinkListApp {

    public static void main(String[] args) {

        LinkList theList = new LinkList();
        theList.insertFirst(1, 10);
        theList.insertFirst(2, 11);
        theList.insertFirst(3, 12.0);
        theList.insertFirst(4, 13.3);

        theList.displayLink();


        Link f = theList.find(2);
        if(f != null){
            System.out.println("find link with key:" + f.kData);

            System.out.println("delete link with key");
            theList.delete(2);
        }
        else
            System.out.println("cant find the link");


        theList.displayLink();


        while(!theList.isEmpty()){
            Link alink = theList.deleteFirst();
            System.out.print("delete:");
            alink.displayLink();
        }
    }
}


// 链节点
class Link{

    public int kData;    // 头指针
    public double dData; // 数据项
    public Link next;    // 下个节点

    public Link(int kd, double dd){
        kData = kd;
        dData = dd;
    }

    public void displayLink(){
        System.out.println("{" + kData + "," + dData+ "}");
    }
}

// 单端链表，只往头插入节点
class LinkList{
    private Link first;

    public LinkList(){
        first = null;
    }

    public boolean isEmpty(){
        return (first == null);
    }

    public void insertFirst(int kd, double dd){

        Link newlink = new Link(kd,dd);
        newlink.next = first;   // newlink--->old first
        first = newlink;        // first--->newlink
    }

    public Link deleteFirst(){
        Link temp = first;
        first = first.next;

        return temp;
    }


    // 查找链节点
    public Link find(int key){
        Link current = first;

        while (current.kData != key){
            if(current.next == null)
                return null;
            else
                current = current.next;
        }

        return current;
    }

    public Link delete(int key){
        Link current = first;
        Link previous = first;

        while(current.kData != key){
            if(current.next == null){
                return null;
            }
            else{
                previous = current;
                current = current.next;
            }
        }

        // 如果是第一个，则first-->first.next;
        // 否则的话，就是前一个的后一个指向当前的下一个
        if(current == first){
            first = first.next;
        }
        else {
            previous.next = current.next;
        }

        return current;

    }


    public void displayLink(){

        System.out.println("Link(first--->last)");
        Link current = first;
        while(current != null){
            current.displayLink();
            current = current.next;
        }
        System.out.println();
    }
}

```

