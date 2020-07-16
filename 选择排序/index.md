# 选择排序


## 选择排序  O(n²) 

选择排序（Selection sort）是一种简单直观的排序算法。

它的工作原理是：第一次从待排序的数据元素数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，然后再从剩余的未排序元素中寻找到最小（大）元素，然后放到已排序的序列的末尾。以此类推，直到全部待排序的数据元素的个数为零。选择排序是不稳定的排序方法。 

```java
import java.util.Arrays;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class SelectSort {
    public static void main(String[] args) {
        int[] arr = new int[]{2, 3, 5, 7, 1, 0, 10, 3, 1, 9};
        selectsort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void selectsort(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            //假设第一个元素为最小元素，记录它的位置
            int min = i;
            //从头遍历整个数组
            for (int j = i + 1; j < arr.length; j++) {
                //如果某元素比记录的元素小，就把下标指向比记录元素小的元素位置
                if (arr[min] > arr[j]) {
                    min = j;
                }
            }
            //如果第一个min的指向变化了，就说明记录的元素已经不是最小的元素，所以要做交换
            if (min != i) {
                int temp = arr[i];
                arr[i] = arr[min];
                arr[min] = temp;
            }
        }
    }
}
```


