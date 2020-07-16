# 冒泡排序


## 冒泡排序  O(n²)

**冒泡排序的思想是对一组元素进行数次遍历，判断前后元素的大小，做交换，将大的数向后排，每一次遍历都将此次排序的最大数移到队列的末尾，所以每整体遍历一次就会有一个元素有序。**

冒泡排序算法的原理如下： 

- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
-  对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。 
- 针对所有的元素重复以上的步骤，除了最后一个。 
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。



```java
import java.util.Arrays;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class BubbleSort {
    public static void main(String[] args) {
        int[] arr = {1, 3, 6, 8, 10, 2};
        bubblesort(arr);
        System.out.println(Arrays.toString(arr));

    }

    public static void bubblesort(int[] arr) {
        int temp = 0;
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }
}
```


