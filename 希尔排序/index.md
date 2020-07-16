# 希尔排序


## 希尔排序   O(nlog2n) 

```java
import java.util.Arrays;

/**
 * @author: YunTaoChen
 * @description:
 * @Date: Create in
 * @Modified by:
 */
public class ShellSort {
    public static void main(String[] args) {
        int[] arr = new int[]{2, 3, 7, 3, 0, 8, 1, 4};
        shellsort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void shellsort(int[] arr) {
        //这里的i是步长，每多少个步长分组，每次循环步长变为原来的一半，直到步长为0
        for (int i = arr.length / 2; i > 0; i = i / 2) {
            //遍历每一个元素
            for (int j = i; j < arr.length; j++) {
                //从每组的第一个元素开始遍历
                for (int k = j - i; k >= 0; k = k - i) {
                    if (arr[k] > arr[k + i]) {
                        //按照顺序交换
                        int temp = arr[k];
                        arr[k] = arr[k + 1];
                        arr[k + 1] = temp;
                    }
                }
            }
        }
    }
}
```


