# 二叉树的前中后序遍历


## 构造二叉树

```java
/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class BinaryTree {
    //二叉树里只需要包含根节点，通过方法调用来使用其他子节点
    TreeNode root;
	//空树构造函数
    public BinaryTree() {
    }

    public void setRoot(TreeNode root) {
        this.root = root;
    }

   	public void preorder() {
        if (root != null) {
            root.preorder();
        }
    }

    public void inorder() {
        if (root != null) {
            root.inorder();
        }
    }

    public void postorder() {
        if (root != null) {
            root.postorder();
        }
    }

    public TreeNode preorderSearch(int value) {
        return root.preorderSearch(value);
    }

    public TreeNode inorderSearch(int value) {
        return root.inorderSearch(value);
    }

    public TreeNode postorderSearch(int value) {
        return root.postorderSearch(value);
    }
}
```

## 构造树节点

```java
import java.time.temporal.Temporal;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class TreeNode {
    int value;
    TreeNode leftNode;
    TreeNode rightNode;

    public TreeNode(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }

    public void setValue(int value) {
        this.value = value;
    }

    public TreeNode getLeftNode() {
        return leftNode;
    }

    public void setLeftNode(TreeNode leftNode) {
        this.leftNode = leftNode;
    }

    public TreeNode getRightNode() {
        return rightNode;
    }

    public void setRightNode(TreeNode rightNode) {
        this.rightNode = rightNode;
    }


    //前序遍历
    public void preorder() {
        System.out.println(this.value);
        if (this.leftNode != null) {
            this.leftNode.preorder();
        }
        if (this.rightNode != null) {
            this.rightNode.preorder();
        }
    }

    //中序遍历
    public void inorder() {
        if (this.leftNode != null) {
            this.leftNode.inorder();
        }
        System.out.println(this.value);
        if (this.rightNode != null) {
            this.rightNode.inorder();
        }
    }

    //后序遍历
    public void postorder() {
        if (this.leftNode != null) {
            this.leftNode.postorder();
        }
        if (this.rightNode != null) {
            this.rightNode.postorder();
        }
        System.out.println(this.value);
    }

    //使用前序遍历查找节点
    public TreeNode preorderSearch(int value) {
        TreeNode target = null;
        //如果当前节点的值等于目标值，返回当前节点
        if (this.value == value) {
            return this;
        }
        //从左子树中查找目标值
        if (this.leftNode != null) {
            target = this.leftNode.preorderSearch(value);
        }
        //判断右子树中书否查找到了目标值，如果找到则返回target
        if (this.rightNode != null) {
            target = this.rightNode.preorderSearch(value);
        }
        //如果不存在该目标值，就直接返回target，此时target值为null
        return target;
    }

    //使用中序遍历查找目标值
    public TreeNode inorderSearch(int value) {
        TreeNode target = null;
        if (this.leftNode != null) {
            target = this.leftNode.inorderSearch(value);
        }
        if (this.value == value) {
            return this;
        }
        if (this.rightNode != null) {
            target = this.rightNode.inorderSearch(value);
        }
        return target;
    }

    public TreeNode postorderSearch(int value) {
        TreeNode target = null;
        if (this.leftNode != null) {
            target = this.leftNode.postorderSearch(value);
        }
        if (this.rightNode != null) {
            target = this.rightNode.postorderSearch(value);
        }
        if (this.value == value) {
            return this;
        }
        return target;
    }
}
```

### 测试类

```java

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class BinaryTreeTest {
    public static void main(String[] args) {
        //建立一个空树
        BinaryTree tree = new BinaryTree();
        //建立一个根节点
        TreeNode root = new TreeNode(1);
        //把根节点赋值给新创建的空树
        tree.setRoot(root);
        //创建左右节点
        TreeNode rootleft = new TreeNode(2);
        TreeNode rootright = new TreeNode(3);
        //把左右节点分别赋值给根节点的左右节点
        root.setLeftNode(rootleft);
        root.setRightNode(rootright);
        //为根节点的左子节点创建左右子节点
        rootleft.setRightNode(new TreeNode(5));
        rootleft.setLeftNode(new TreeNode(4));
        //为根节点的右子节点创建左右子节点
        rootright.setLeftNode(new TreeNode(6));
        rootright.setRightNode(new TreeNode(7));
        //调用前序遍历
        tree.preorder();
        System.out.println("-----");
        //调用中序遍历
        tree.inorder();
        System.out.println("-----");
        //调用后序遍历
        tree.postorder();
        //调用查找方法
        TreeNode result = tree.preorderSearch(3);
        //已经知道value是3的节点对象是rootright，判断两者是否是同一对象，结果返回true
        System.out.println(result == rootright);
    }
}
```



### **二叉树的前中后序遍历都是运用了递归的思想。**

## 前序遍历

前序遍历的顺序：根节点-左孩子-右孩子

```java
//前序遍历
    public void preorder() {
        System.out.println(this.value);
        if (this.leftNode != null) {
            this.leftNode.preorder();
        }
        if (this.rightNode != null) {
            this.rightNode.preorder();
        }
    }
//使用前序遍历查找节点
    public TreeNode preorderSearch(int value) {
        TreeNode target = null;
        //如果当前节点的值等于目标值，返回当前节点
        if (this.value == value) {
            return this;
        }
        //从左子树中查找目标值
        if (this.leftNode != null) {
            target = this.leftNode.preorderSearch(value);
        }
        //判断右子树中书否查找到了目标值，如果找到则返回target
        if (this.rightNode != null) {
            target = this.rightNode.preorderSearch(value);
        }
        //如果不存在该目标值，就直接返回target，此时target值为null
        return target;
    }
```



## 中序遍历

中序遍历的顺序：左孩子-根节点-右孩子

```java
//中序遍历
    public void inorder() {
        if (this.leftNode != null) {
            this.leftNode.inorder();
        }
        System.out.println(this.value);
        if (this.rightNode != null) {
            this.rightNode.inorder();
        }
    }
//使用中序遍历查找目标值
    public TreeNode inorderSearch(int value) {
        TreeNode target = null;
        if (this.leftNode != null) {
            target = this.leftNode.inorderSearch(value);
        }
        if (this.value == value) {
            return this;
        }
        if (this.rightNode != null) {
            target = this.rightNode.inorderSearch(value);
        }
        return target;
    }
```



## 后序遍历

后序遍历的顺序：左孩子-右孩子-根节点

```java
//后序遍历
    public void postorder() {
        if (this.leftNode != null) {
            this.leftNode.postorder();
        }
        if (this.rightNode != null) {
            this.rightNode.postorder();
        }
        System.out.println(this.value);
    }
public TreeNode postorderSearch(int value) {
        TreeNode target = null;
        if (this.leftNode != null) {
            target = this.leftNode.postorderSearch(value);
        }
        if (this.rightNode != null) {
            target = this.rightNode.postorderSearch(value);
        }
        if (this.value == value) {
            return this;
        }
        return target;
    }
```


