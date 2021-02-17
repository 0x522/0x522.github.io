# 计数排序


## 计数排序

计数排序是桶排序的一种特殊情况，用于解决大量重复元素的排序，是一种非比较排序<br>
它的核心思想是：<br>
利用桶数组的下标映射待排序数组元素值，重复元素就把桶数组元素累加来记录个数。

### countSort

**计数排序的核心是构造桶数组，用桶数组的下标映射待排序数组的元素。**<br>
特别要注意的是：**计数排序的待排序数组如果指定排序区间，将会比从 0 开始的排序数组复杂**<br>
构造排序函数的步骤：<br>

- 构造结果数组 result
- 构造桶数组 count
- 遍历 arr 数组，把其中元素值当作桶数组的下标。**为了使桶数组 count 做到最节省空间，则使用 arr[i]-start 对齐。**
- 桶数组累加。
  ![](计数排序-1.png)

```java
import java.util.Arrays;

    /**
     * @param arr   待排序数组
     * @param start 数组元素区间最小值
     * @param end   数组元素区间最大值
     */
    public static void countSort(int[] arr, int start, int end) {
        int[] result = new int[arr.length];
        int[] count = new int[end - start];
        for (int i = 0; i < arr.length; i++) {
            //count数组下标代表待排序数组中的元素值,count中的元素代表数组下标在待排序数组中出现的次数
            count[arr[i] - start]++;
        }
        //此时把count变为累加数组,元素内容-1为下标元素在原数组中对应的位置,也在结果数组中确定了位置,直接放置即可
        for (int i = 1; i < count.length; i++) {
            count[i] = count[i] + count[i - 1];
        }
//        for (int i = 0, j = 0; i < count.length; i++) {
//            while (count[i]-- > 0) {
//                result[j++] = i + start;
//            }
//        }
        for (int i = arr.length - 1; i >= 0; i--) {
            result[--count[arr[i] - start]] = arr[i];
        }
        for (int i = 0; i < result.length; i++) {
            arr[i] = result[i];
        }
    }
```

