# 比较选择排序和插入排序

```java
import edu.princeton.cs.algs4.StdOut;
import edu.princeton.cs.algs4.StdRandom;
import edu.princeton.cs.algs4.Stopwatch;

/**
 * @author LFool
 * @create 2020-07-24 18:13
 */
@SuppressWarnings({"rawtypes"})
public class SortCompare {

    public static double time(String alg, Comparable[] arr) {
        Stopwatch timer = new Stopwatch();
        String[] sortType = {"Insertion", "Selection"};
        if (sortType[0].equals(alg)) {
            Insertion.sort(arr);
        }
        if (sortType[1].equals(alg)) {
            Selection.sort(arr);
        }
        return timer.elapsedTime();
    }

    public static double timeRandomInput(String alg, int n, int t) {
        double total = 0;
        Double[] arr = new Double[n];
        for (int i = 0; i < t; i++) {
            for (int j = 0; j < n; j++) {
                arr[j] = StdRandom.uniform();
            }
            total += time(alg, arr);
        }
        return total;
    }

    public static void main(String[] args) {
        String alg1 = "Insertion";
        String alg2 = "Selection";
        int n = 1000;
        int t = 100;
        double t1 = timeRandomInput(alg1, n, t);
        double t2 = timeRandomInput(alg2, n, t);
        StdOut.printf("For %d random Doubles\n   %s is", n, alg1);
        StdOut.printf(" %.1f times faster than %s\n", t2 / t1, alg2);
    }

    /**
     * Result:
     * For 1000 random Doubles
     *    Insertion is 1.1 times faster than Selection
     */

}

```

