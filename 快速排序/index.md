# 快速排序


## 快速排序  O(nlogn) 

**快速排序的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。**

快速排序运用了递归的思想，快速排序的结束条件是排序队列内只剩下一个元素，此时不需要再排序。

快速排序算法通过多次比较和交换来实现排序，其排序流程如下： 

1. 首先设定一个分界标准值，通常这个标准值用每次排序的第一个元素，通过该分界值将数组分成左右两部分。
2.  将大于或等于分界值的数据集中到数组右边，小于分界值的数据集中到数组的左边。此时，左边部分中各元素都小于或等于分界值，而右边部分中各元素都大于或等于分界值。
3.  然后，左边和右边的数据可以独立排序。对于左侧的数组数据，又可以取一个分界值，将该部分数据分成左右两部分，同样在左边放置较小值，右边放置较大值。右侧的数组数据也可以做类似处理。 
4. 重复上述过程，可以看出，这是一个递归定义。通过递归将左侧部分排好序后，再递归排好右侧部分的顺序。当左、右两个部分各数据排序完成后，整个数组的排序也就完成了。

```java
import java.util.Arrays;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class QuickSort {
    public static void main(String[] args) {
        int[] arr = {1, 23, 54, 21, 2, 4, 5, 8, 10};
        quickSort(arr, 0, arr.length - 1);
        System.out.println(Arrays.toString(arr));
    }

    public static void quickSort(int arr[], int start, int end) {
        //把数组的起始位置和最后一个位置标记
        int low = start;
        int high = end;
        //标记标准数为要排序数组的第一个数
        int standard = arr[start];
        if (start < end) {
            //当前后指针未重合，重合表示排序结束
            while (low < high) {
                //当指针没有重合，而且标准数小于等于后指针指向的数，后指针前移
                while (low < high && standard <= arr[high]) {
                    high--;
                }
                //如果后指针指向的数小于标准数，那么就用当前数替换前指针指向的数
                arr[low] = arr[high];
                //如果前指针指向的数小于标准数，就让前指针后移
                while (low < high && standard >= arr[low]) {
                    low++;
                }
                //如果前指针指向的数大于标准数，就用当前数替换后指针指向的数
                arr[high] = arr[low];
            }
            //此时前指针和后指针重合，去low和high代表同一个位置，把标准数放置在重合位置，因为前后指针来回替换，总有一个多出来的重复的数字，现在用标准数将它替换掉
            arr[low] = standard;
            //递归排序，将数组分为前后两部分，前一部分全部是小于标准数的，后一部分全部是大于的，分隔线就是low和high重合的位置，取low或high都可
            //对前一部分快速排序
            quickSort(arr, start, low);
            //对后一部分快速排序
            quickSort(arr, low + 1, end);
        }
    }
}
```


