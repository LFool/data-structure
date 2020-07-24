# 希尔排序

## 1. 算法思想

对于大规模乱序数组插入排序很慢，因为它只会交换相邻的元素，因此元素只能一点一点地从数组的一端移动到另一端。例如如果主键最小的元素正好在数组的尽头，要将它挪到正确的位置就需要 $$N - 1$$ 次移动。希尔排序为了加快速度简单地改进了插入排序，交换不相邻的元素以对数组的局部进行排序，并最终用插入排序将局部有序的数组排序。

对于插入排序，每次交换仅仅在相邻的元素之间，所以最多减少一个逆序对。而对于希尔排序，不相邻的元素交换一次，可能改变多个逆序对。

希尔排序的思想是使数组中任意间隔为 h 的元素都是有序的。

## 2. 增量选择

不同的增量，可导致运行速度的不同

## 3. 算法特点

$$D_k$$ 间隔有序的序列，在执行 $$D_{k - 1}$$ 间隔排序后，仍然是 $$D_k$$ 间隔有序的

## 4. 算法实现

```java
import edu.princeton.cs.algs4.StdIn;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

/**
 * @author LFool
 * @create 2020-07-24 20:20
 */
@SuppressWarnings({"rawtypes"})
public class Shell {

    private Shell() {
    }

    /**
     * Rearranges the array in ascending order, using the natural order
     * @param arr the array to be sorted
     */
    public static void sort(Comparable[] arr) {
        int n = arr.length;

        // 3x+1 increment sequence:  1, 4, 13, 40, 121, 364, 1093, ...
        int h = 1;
        while (h < n / 3) {
            h = 3 * h + 1;
        }

        while (h >= 1) {
            // h-sort the array
            for (int i = h; i < n; i++) {
                for (int j = i; j >= h && SortUtil.less(arr[j], arr[j - 1]); j--) {
                    SortUtil.exch(arr, j, j - 1);
                }
            }
            assert SortUtil.isHSorted(arr, h);
            h /= 3;
        }
        assert SortUtil.isSorted(arr);
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
        Shell.sort(arr);
        SortUtil.show(arr);
    }
}

```

