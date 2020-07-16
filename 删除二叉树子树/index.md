# 删除二叉树子树


## 删除二叉树的子树

```java
//二叉树类BinaryTree.java中的删除子树方法
    
public void delete(int value) {
    //判断根节点是否是要删除的节点
        if(root.value == value){
            root = null;
        }else{
            //调用TreeNode中的delete()方法
            root.delete(value);
        }
    }
```

```java
//二叉树节点类TreeNode.java中的删除子树方法

 public void delete(int value) {
        TreeNode parent = this;
        //判断左儿子，这里的parent.leftNode != null必须写在&&之前，避免空指针异常
        if (parent.leftNode != null && parent.leftNode.value == value) {
            parent.leftNode = null;
            return;
        }
        //判断右儿子，同左儿子
        if (parent.rightNode != null && parent.rightNode.value == value) {
            parent.rightNode = null;
            return;
        }
        //左子树递归查找
     	//把当前节点的左节点作为根节点继续查找它的左右子节点，直到为空
        parent = this.leftNode;
        if (parent != null) {
            parent.delete(value);
        }
        //右子树递归查找
        parent = this.rightNode;
        if (parent != null) {
            parent.delete(value);
        }
    }
```

```java
//测试类BinaryTreeTest.java
		//删除权值为7的节点
		tree.delete(7);
		//使用前序遍历来查看删除后的结果
        tree.preorder();
		//删除权值为1的节点，即根节点
		tree.delete(1);
		tree.preorder();
```
