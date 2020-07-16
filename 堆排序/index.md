# 堆排序


## 堆排序   O (nlgn) 

**堆排序**是指利用**堆**这种数据结构所设计的一种排序算法。堆是一个近似**完全二叉树**的结构，并同时满足**堆积的性质**：即子结点的键值或索引总是小于（或者大于）它的父节点。

堆中定义以下几种操作：  

- 最大堆调整：将堆的末端子节点作调整，使得子节点永远小于父节点
- 创建最大堆：将堆中的所有数据重新排序
- 堆排序：移除位在第一个数据的根节点，并做最大堆调整的递归运算



**Java实现：**

```java
import java.util.Arrays;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class HeapSort {
    public static void main(String[] args) {
        //使用数组定义一棵完全二叉树
        int[] arr = new int[]{1,2,5,4,6};

    }

    //构造部分大顶堆函数,参数size是节点个数，index是节点位置
    public static void MaxHeap(int[] arr, int size, int index) {
        //左子节点
        int leftNode = 2 * index + 1;
        //右子节点
        int rightNode = 2 * index + 2;
        //三个节点相比取最大且保存
        int max = index;
        if (leftNode < size && arr[leftNode] > arr[max]) {
            max = leftNode;
        }
        if (rightNode < size && arr[rightNode] > arr[max]) {
            max = rightNode;
        }
        //如果max里面的默认值index没有改变，说明index已经是最大，否则交换
        if (max != index) {
            int temp = arr[index];
            arr[index] = arr[max];
            arr[max] = temp;
            //交换位置以后跟可能会破坏之前排好的堆，所以要重新调整，相当于是拿三个节点中的最大值节点与他们的子节点进行比较，如此递归下去
            //max表示调整之后的位置,这个操作会把权值较小的元素放置在合适的位置上
            MaxHeap(arr, size, max);
        }
    }


    //堆排序
    public static void heapSort(int[] arr) {
        //start是最后一个叶子节点的父节点
        int start = (arr.length - 1) / 2;
        //从最后一个节点的父节点依次向前遍历，直到根节点
        for (int i = start; i >= 0; i--) {
            //循环加构造部分大顶堆函数构造成了一个完整的大顶堆
            MaxHeap(arr, arr.length, i);
        }//此时大顶堆已经成型
        //进行排序，排序基本操作是每次有一个元素有序，因为大顶堆的最大元素在根，所以拿根与数组最后一个元素交换，每换一次再重新构造一次大顶堆，使得每一次将要排序的最大元素都在根节点
        for (int i = arr.length - 1; i > 0; i--) {
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;
            //再将有序元素外的其他元素构造成大顶堆
            MaxHeap(arr, i, 0);
        }
    }
}
```


