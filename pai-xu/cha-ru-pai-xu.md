# 插入排序

## 1. 算法思想

通常人们整理桥牌的方法是一张一张的来，将每一张牌插入到其他已经有序的牌中的适当位置。

在计算机的实现中，为了给要插入的元素腾出空间，我们需要将其余所有元素在插入之前都想有移动一位。

## 2. 算法效率

对于随机排列的长度为 $$N$$ 且主键不重复的数组

* 平均情况下需要 $$N^2 / 4$$ 次比较 以及 $$N^2 / 4$$ 次交换
* 最坏情况下需要 $$N^2 / 2$$ 次比较 以及 $$N^2 / 2$$ 次交换
* 最好情况下需要 $$N - 1$$ 次比较 以及 $$0$$ 次交换

## 3. 算法特点

当前索引左边的所有元素都是有序的，但是最终位置不确定，可能后来还需要在其中插入新的元素

对于一个很大且其中元素已经有序（或接近有序）的数组进行排序将会比随机顺序的数组或是逆序数组进行排序快得多

当倒置的数量很少时，插入排序很可能比其他任何算法都要快

## 4. 算法实现

```java
import edu.princeton.cs.algs4.StdIn;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.util.Comparator;

/**
 * @author LFool
 * @create 2020-07-24 16:39
 */
@SuppressWarnings({"rawtypes"})
public class Insertion {

    /**
     * Rearranges the array in ascending order, using the natural order
     * @param arr the array to be sorted
     */
    public static void sort(Comparable[] arr) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            for (int j = i; j > 0 && SortUtil.less(arr[j], arr[j - 1]); j--) {
                SortUtil.exch(arr, j, j - 1);
            }
            assert SortUtil.isSorted(arr, 0, i);
        }
        assert SortUtil.isSorted(arr);
    }

    /**
     * Rearranges the subarray(a[lo...hi]) in ascending order, using the natural order
     * @param arr the array to be sorted
     * @param lo left endpoint (inclusive)
     * @param hi right endpoint (inclusive)
     */
    public static void sort(Comparable[] arr, int lo, int hi) {
        for (int i = lo + 1; i <= hi; i++) {
            for (int j = i; j > lo && SortUtil.less(arr[j], arr[j - 1]); j--) {
                SortUtil.exch(arr, j, j - 1);
            }
            assert SortUtil.isSorted(arr, lo, i);
        }
        assert SortUtil.isSorted(arr);
    }

    /**
     * Rearranges the array in ascending order, using a comparator
     * @param arr the array
     * @param comparator a comparator specifying the order
     */
    public static void sort(Object[] arr, Comparator comparator) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            for (int j = i; j > 0 && SortUtil.less(comparator, arr[j], arr[j - 1]); j--) {
                SortUtil.exch(arr, j, j - 1);
            }
            assert SortUtil.isSorted(arr, comparator, 0, i);
        }
        assert SortUtil.isSorted(arr, comparator);
    }

    /**
     * Rearranges the subarray(a[lo...hi]) in ascending order, using the natural order
     * @param arr the array to be sorted
     * @param comparator a comparator specifying the order
     * @param lo left endpoint (inclusive)
     * @param hi right endpoint (inclusive)
     */
    public static void sort(Object[] arr, Comparator comparator, int lo, int hi) {
        for (int i = lo; i <= hi; i++) {
            for (int j = i; j > lo && SortUtil.less(comparator, arr[j], arr[j - 1]); j--) {
                SortUtil.exch(arr, j, j - 1);
            }
            assert SortUtil.isSorted(arr, comparator, lo, i);
        }
        assert SortUtil.isSorted(arr, comparator, lo, hi);
    }

    public static void main(String[] args) throws FileNotFoundException {
        FileInputStream input = new FileInputStream("src/main/resources/" + "tiny.txt");
        System.setIn(input);
        String[] arr = StdIn.readAllStrings();
        Insertion.sort(arr, (Comparator<String>) (o1, o2) -> Character.compare(o2.charAt(0), o1.charAt(0)));
        SortUtil.show(arr);
    }

}

```

