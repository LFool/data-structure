# 排序

整个排序部分，会用到 三个 公共方法，如下所示：

```java
import java.util.Comparator;

/**
 * @author LFool
 * @create 2020-07-24 01:09
 */
@SuppressWarnings({"unchecked", "rawtypes"})
public class SortUtil {

    /**
     * is v < w ?
     * @param v first element
     * @param w second element
     * @return if v < w return true; else return false
     */
    public static boolean less(Comparable v, Comparable w) {
        return v.compareTo(w) < 0;
    }

    /**
     * is v < w ?
     * @param comparator comparator
     * @param v first element
     * @param w second element
     * @return if v < w return true; else return false
     */
    public static boolean less(Comparator comparator, Object v, Object w) {
        return comparator.compare(v, w) < 0;
    }

    /**
     * exchange arr[i] and arr[j]
     * @param arr array
     * @param i index of exchange element
     * @param j index of exchange element
     */
    public static void exch(Object[] arr, int i, int j) {
        Object tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }

    /**
     * is the array arr[] sorted?
     * @param arr array
     * @return if is sorted return true; else return false
     */
    public static boolean isSorted(Comparable[] arr) {
        return isSorted(arr, 0, arr.length - 1);
    }

    /**
     * is the array sorted from arr[lo] to arr[hi]?
     * @param arr array
     * @param lo index of low element
     * @param hi index of high element
     * @return if is sorted return true; else return false
     */
    public static boolean isSorted(Comparable[] arr, int lo, int hi) {
        for (int i = lo; i <= hi; i++) {
            if (less(arr[i], arr[i - 1])) {
                return false;
            }
        }
        return true;
    }

    /**
     * is the array arr[] sorted?
     * @param arr array
     * @param comparator comparator
     * @return if is sorted return true; else return false
     */
    public static boolean isSorted(Object[] arr, Comparator comparator) {
        return isSorted(arr, comparator, 0, arr.length - 1);
    }

    /**
     * is the array sorted from arr[lo] to arr[hi]?
     * @param arr array
     * @param comparator comparator
     * @param lo index of low element
     * @param hi index of high element
     * @return if is sorted return true; else return false
     */
    public static boolean isSorted(Object[] arr, Comparator comparator, int lo, int hi) {
        for (int i = lo; i <= hi; i++) {
            if (less(comparator, arr[i], arr[i - 1])) {
                return false;
            }
        }
        return true;
    }

}

```



