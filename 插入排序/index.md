# 插入排序


## 插入排序  O(n²)

插入算法把要排序的数组分成两部分：第一部I分包含了这个数组的所有元素，但将最后一个元素除外（让数组多一个空间才有插入的位置），而第二部分就只包含这一个元素（即待插入元素）。在第一部分排序完成后，再将这个最后元素插入到已排好序的第一部分中。

**插入排序的基本思想是：每步将一个待排序的记录，按其临时值的大小插入前面已经排序的文件中适当位置上，直到全部插入完为止。** 

```java
import java.util.Arrays;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class InsertSort {
    public static void main(String[] args) {
        int[] arr = new int[]{1, 6, 3, 7, 9, 2, 0};
        insertSort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void insertSort(int[] arr) {
        int j = 0;
        int temp;
        //默认认为只有一个元素时，数组有序，所有从数组的第二个元素开始遍历，每一趟排序就会有一个元素有序
        for (int i = 1; i < arr.length; i++) {
            //如果当前数字比前一个数字小，就要把它向前放
            if (arr[i] < arr[i - 1]) {
                //取出当前的数作为临时数，保存并即将做插入操作
                temp = arr[i];
                //从当前数字前面一个元素开始遍历,因为是向前遍历，所以条件是j>=0和临时数字小于前面的数字才做操作，否则目前就是顺序排列
                for (j = i - 1; j >= 0 && temp < arr[j]; j--) {
                    //每次都把当前数给后一个位置，为临时值的插入腾出空间
                    arr[j + 1] = arr[j];
                }
                //把temp放在当前元素的前一个位置上
                arr[j + 1] = temp;
            }
        }
    }
}
```


