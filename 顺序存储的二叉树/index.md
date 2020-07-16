# 顺序存储的二叉树


## 顺序存储的二叉树

**顺序存储的二叉树通常情况 只考虑完全二叉树。**

常用基本性质：

1. **第n个元素的左子节点是2n+1，它的右子节点是2n+2**
2. **第n个元素的父节点是(n-1)/2**

### 遍历

**数组二叉树使用的遍历方法与正常二叉树的遍历方法相同，分为前中后序遍历。**

构造一棵数组二叉树：

```java
/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class ArrayBinaryTree {
    int[] data;

    public ArrayBinaryTree(int[] data) {
        //构造函数，参数是一个数组，使用这个数组构造一棵完全二叉树
        this.data = data;
    }

    //前序遍历,参数是从第几个元素开始遍历
    public void preorder(int index) {
        //数组为空或者数组长度为0，直接返回
        if (data == null || data.length == 0) {
            return;
        } else {
            //遍历当前节点，
            System.out.println(data[index]);
            //判断当前节点的左子节点是否在数组长度内，可能没有左子节点
            //判断完成后前序递归查找节点
            if (index * 2 + 1 < data.length) {
                preorder(2 * index + 1);
            }
            if (index * 2 + 2 < data.length) {
                preorder(2 * index + 2);
            }
        }
    }

    //中序遍历
    public void inorder(int index) {
        if (data.length == 0 || data == null) {
            return;
        } else {
            if (2 * index + 1 < data.length) {
                inorder(2 * index + 1);
            }
            System.out.println(data[index]);
            if (2 * index + 2 < data.length) {
                inorder(2 * index + 2);
            }
        }
    }

    //后序遍历
    public void postorder(int index) {
        if (data == null || data.length == 0) {
            return;
        } else {
            if (2 * index + 1 < data.length) {
                postorder(2 * index + 1);
            }
            if (2 * index + 2 < data.length) {
                postorder(2 * index + 2);
            }
            System.out.println(data[index]);
        }
    }
}
```



测试类：

```java
/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class ArrayBinaryTreeTest {
    public static void main(String[] args) {
        //使用数组构造一棵完全二叉树
        int[] arr = new int[]{1, 2, 3, 4, 5, 6, 7};
        ArrayBinaryTree atree = new ArrayBinaryTree(arr);
        System.out.println("-------------");
        atree.preorder(0);
        System.out.println("-------------");
        atree.inorder(0);
        System.out.println("-------------");
        atree.postorder(0);
    }
}
```


