# 归并排序


## 归并排序

归并排序利用了分治和递归的思想,把一些元素分成若干份并把它们分别排序,有序后再合并。

- 归并排序基于的思想
  - 递归
  - 分治

### 归并排序的核心

通常来说归并排序被分为两个模块：

- 排序模块 `sort()`
- 归并模块 `merge()`

### merge

此模块是为了解决合并问题而设计的。<br>
它的主要思想是将前后两个**有序数组**合并成一个有序数组。<br>

核心思想：<br>

- i,j,k 三个指针,用来表示数组下标。i 指向前一个有序数组的第一个元素，j 指向后一个有序数组的第一个元素，k 指向临时数组 temp 的第一个元素。
- 前后有序数组的元素逐个比较，较小的放入临时数组 temp，直到其中一个数组完全放入临时数组。
- 将有剩余元素未加入临时数组 temp 的数组按顺序加入临时数组 temp。
- 将临时数组 temp 中的元素写回 arr 中。

```java
    public static void merge(int[] arr, int leftPtr, int rightPtr, int bound) {
            int i = leftPtr;
            int j = rightPtr;
            int k = 0;
            int mid = rightPtr - 1;
            int[] temp = new int[bound - leftPtr + 1];
            while (i <= mid && j <= bound) {
                temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
            }
            while (i <= mid) {
                temp[k++] = arr[i++];
            }
            while (j <= bound) {
                temp[k++] = arr[j++];
            }
            for (int l = 0; l < temp.length; l++) {
                arr[leftPtr + l] = temp[l];
            }
        }
```

### sort

此模块的核心思想是**递归**，递归条件是 left < right。<br>

- 如果 left > right 就报错，left==right 就直接返回。
- 将待排序数组分成两半分别排序。
- 排序后将两半的有序数组合并，这就用到了刚才的`merge()`。
- mid 变量由于 left、right 是 int 类型，防止+运算溢出就使用-，同时使用位运算除以 2，提升执行效率。

```java
    public static void sort(int[] arr, int left, int right) {
            if (left > right) {
                throw new IllegalArgumentException("left必须小于等于right");
            }
            if (left == right) return;
            //先分成两半
            int mid = left + ((right - left) >> 1);
            //左边排序
            sort(arr, left, mid);
            //右遍排序
            sort(arr, mid + 1, right);
            //合并
            merge(arr, left, mid + 1, right);
        }
```

