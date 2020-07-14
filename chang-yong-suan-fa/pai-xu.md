# 排序

## 堆排序

```java
public class Heap {
    @SuppressWarnings("rawtypes")
    public static void sort(Comparable[] pq) {
        int n = pq.length;

        for (int k = n / 2; k >= 1; k--) {
            sink(pq, k, n);
        }

        int k = n;
        while (k > 1) {
            exch(pq, 1, k--);
            sink(pq, 1, k);
        }
    }

    /**
     * top to bottom
     * @param pq the array to be sorted
     * @param k the index of the moving element
     * @param n the size of array
     */
    @SuppressWarnings("rawtypes")
    private static void sink(Comparable[] pq, int k, int n) {

        while (2 * k <= n) {
            int child = 2 * k;
            if (child < n && less(pq, child, child + 1)) {
                child++;
            }
            if (!less(pq, k, child)) {
                break;
            }
            exch(pq, k, child);
            k = child;
        }

    }

    private static void exch(Object[] pq, int i, int j) {
        Object swap = pq[i - 1];
        pq[i - 1] = pq[j - 1];
        pq[j - 1] = swap;
    }

    @SuppressWarnings({"unchecked", "rawtypes"})
    private static boolean less(Comparable[] pq, int i, int j) {
        return pq[i - 1].compareTo(pq[j - 1]) < 0;
    }

    @SuppressWarnings("rawtypes")
    private static void show(Comparable[] a) {
        for (Comparable comparable : a) {
            System.out.print(comparable + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Integer[] a = new Integer[]{3, 4, 5, 6, 3, 4, 6, 7, 8, 1};
        Heap.sort(a);
        show(a);
    }
}

```

