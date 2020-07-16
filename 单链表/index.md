# 单链表


## 数据结构单链表

#### 链表

链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。每个结点包括两个部分：一个是存储数据元素的数据域，另一个是存储下一个结点地址的指针域。 相比于线性表顺序结构，操作复杂。

**实现类：**

```java
/**
  *
  *@author: YuntaoChen
  *@description:
  *@Date:  Create in ${Time} ${Date}
  *@Modified by:
  *
  */
public class Node {
    private int data;
    private Node next;

    public Node(int data) {
        this.data = data;
    }

    //取出当前节点的下一个节点
    public Node getNext() {
        return this.next;
    }

    //追加节点
    public Node append(Node node) {
        /*this.next = node;*/
        Node currentNode = this;
        while (true) {
            Node nextNode = currentNode.next;
            //判断是否是最后一个节点
            if (nextNode == null) {
                currentNode.next = node;
                break;
            } else
                currentNode = nextNode;
        }
        return node;
    }

    //删除下一个节点,先找到当前节点的下下个节点，保存一下，然后将保存的节点赋值给当前节点的下一个节点
    public void removeNext() {
        Node Nextnext = this.next.next;
        this.next = Nextnext;
    }

    //插入节点，在当前节点的后面插入一个节点
    public void insert(Node node) {
        Node Nextnext = this.next;
        this.next = node;
        node.next = Nextnext;

    }

    //获取当前节点的data值
    public int getData() {
        return this.data;
    }

    //判断链表是否是空
    public boolean isEmpty() {
        //只需要判断头节点是否为空
        return this.next == null;
    }

    //判断当前节点是否是最后一个节点
    public boolean isLast() {
        return this.next == null;
    }

    //显示所有节点的信息
    public void show() {
        Node currentNode = this;
        while (true) {
            System.out.print(currentNode.data+" ");
            //一直取下一个节点
            currentNode = currentNode.next;
            //如果currentNode是空节点，结束循环
            if(currentNode == null){
                break;
            }
        }
    }
}
```



**测试类：**

```java
public class NodeTest {
    public static void main(String[] args) {
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        n1.append(n2);
        n1.append(n3);
        n1.append(n4);
        n2.insert(n5);
        n5.removeNext();
        System.out.println(n1.getNext().getData());
        System.out.println(n2.getNext().getData());
        System.out.println(n1.getNext().getNext().isLast());
        n1.show();
    }
}
```


