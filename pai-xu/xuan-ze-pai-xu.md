# 选择排序

## 1. 算法思想

* 首先，找到数组中最小的那个元素
* 其次，将它和数组的第一个元素交换位置（如果第一个元素就是最小的元素那么它就和自己交换）
* 再次，在剩下的元素中找到最小的元素，将它与数组的第二个元素交换位置
* 如此往复，直到将整个数组排序

## 2. 算法效率

根据算法的思想我们可以知道，**交换的总次数**是 $$N$$ ，所以算法的时间效率取决于**比较次数**（选出最小值的过程）

**命题：**对于长度为 $$N$$ 的数组，选择排序需要大约 $$N^2 / 2$$ 次**比较**和 $$N$$ 次**交换**

## 3. 算法特点

### 3.1 运行时间和输入无关

为了找到最小元素而扫描一遍数组 不能 为下一遍扫描提供任何信息。这一性质在某些情况下是缺点，因为使用选择排序可能惊讶的发现，一个已经有序的数组或主键全部相等的数组和一个元素随机排列的数组所用的时间竟然一样长！

### 3.2 数据移动是最少的

任何情况下，选择排序交换元素的次数是线性， $$N$$ 次

其他排序算法都不具备这个特征

## 4. 算法实现

```java
import edu.princeton.cs.algs4.StdIn;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.util.Comparator;

/**
 * @author LFool
 * @create 2020-07-24 01:04
 */
@SuppressWarnings({"rawtypes"})
public class Selection {

    /**
     * Rearranges the array in ascending order, using the natural order
     * @param arr the array to be sorted
     */
    public static void sort(Comparable[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            int min = i;
            for (int j = i; j < n; j++) {
                if (SortUtil.less(arr[j], arr[min])) {
                    min = j;
                }
            }
            SortUtil.exch(arr, i, min);
            if (!SortUtil.isSorted(arr, 0, i)) {
                throw new AssertionError();
            }
        }
        if (!SortUtil.isSorted(arr)) {
            throw new AssertionError();
        }
    }

    /**
     * Rearranges the array in ascending order, using a comparator
     * @param arr the array
     * @param comparator a comparator specifying the order
     */
    public static void sort(Object[] arr, Comparator comparator) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            int min = i;
            for (int j = i; j < n; j++) {
                if (SortUtil.less(comparator, arr[j], arr[min])) {
                    min = j;
                }
            }
            SortUtil.exch(arr, i, min);
            if (!SortUtil.isSorted(arr, comparator, 0, i)) {
                throw new AssertionError();
            }
        }
        if (!SortUtil.isSorted(arr, comparator)) {
            throw new AssertionError();
        }
    }

    /**
     * Reads in a sequence of strings from standard input; selection sorts them;
     * and prints them to standard output in ascending order.
     *
     * @param args the command-line arguments
     */
    public static void main(String[] args) throws FileNotFoundException {
        FileInputStream input = new FileInputStream("src/main/resources/" + "tiny.txt");
        System.setIn(input);
        String[] arr = StdIn.readAllStrings();
        Selection.sort(arr);
        SortUtil.show(arr);
    }

}

```

