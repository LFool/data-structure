# N Sum

## 1. Two Sum

{% embed url="https://leetcode.com/problems/two-sum/" %}

{% tabs %}
{% tab title="结果唯一" %}
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(target - nums[i])) {
            return new int[]{map.get(target - nums[i]), i};
        }
        map.put(nums[i], i);
    }
    return null;
}
```
{% endtab %}

{% tab title="结果不唯一" %}
```java
public List<int[]> twoSum(int[] nums, int target) {

    List<int[]> res = new ArrayList<>();

    Arrays.sort(nums);

    int lo = 0, hi = nums.length - 1;
    while (lo < hi) {
        int sum = nums[lo] + nums[hi];
        int left = nums[lo], right = nums[hi];
        if (sum < target) {
            while (lo < hi && nums[lo] == left) lo++;
        } else if (sum > target) {
            while (lo < hi && nums[hi] == right) hi--;
        } else {
            res.add(new int[]{left, right});
            while (lo < hi && nums[lo] == left) lo++;
            while (lo < hi && nums[hi] == right) hi--;
        }
    }
    return res;

```
{% endtab %}
{% endtabs %}

## 2. 3Sum

{% embed url="https://leetcode.com/problems/3sum/" %}

```java
public List<List<Integer>> threeSum(int[] nums) {

    List<List<Integer>> res = new ArrayList<>();

    Arrays.sort(nums);
    for (int i = 0; i < nums.length; i++) {
        List<List<Integer>> temp = twoSum(nums, i + 1, 0 - nums[i]);
        for (List<Integer> t : temp) {
            t.add(0, nums[i]);
            res.add(t);
        }

        while (i < nums.length - 1 && nums[i] == nums[i + 1]) i++;
    }

    return res;

}

private List<List<Integer>> twoSum(int[] nums, int start, int target) {

    List<List<Integer>> res = new ArrayList<>();

    int lo = start, hi = nums.length - 1;
    while (lo < hi) {
        int sum = nums[lo] + nums[hi];
        int left = nums[lo], right = nums[hi];
        if (sum < target) {
            while (lo < hi && nums[lo] == left) lo++;
        } else if (sum > target) {
            while (lo < hi && nums[hi] == right) hi--;
        } else {
            List<Integer> temp = new ArrayList<>();
            temp.add(left);
            temp.add(right);
            res.add(temp);
            while (lo < hi && nums[lo] == left) lo++;
            while (lo < hi && nums[hi] == right) hi--;
        }
    }
    return res;
}
```

## 3. 4Sum

{% embed url="https://leetcode.com/problems/4sum/" %}

```java
public List<List<Integer>> fourSum(int[] nums, int target) {

    List<List<Integer>> res = new ArrayList<>();

    Arrays.sort(nums);

    for (int i = 0; i < nums.length; i++) {
        List<List<Integer>> temp = threeSum(nums, i + 1, target - nums[i]);
        for (List<Integer> t : temp) {
            t.add(0, nums[i]);
            res.add(t);
        }

        while (i < nums.length - 1 && nums[i] == nums[i + 1]) i++;
    }

    return res;

}

private List<List<Integer>> threeSum(int[] nums, int start, int target) {

    List<List<Integer>> res = new ArrayList<>();

    for (int i = start; i < nums.length; i++) {
        List<List<Integer>> temp = twoSum(nums, i + 1, target - nums[i]);
        for (List<Integer> t : temp) {
            t.add(0, nums[i]);
            res.add(t);
        }

        while (i < nums.length - 1 && nums[i] == nums[i + 1]) i++;
    }

    return res;

}

private List<List<Integer>> twoSum(int[] nums, int start, int target) {

    List<List<Integer>> res = new ArrayList<>();

    int lo = start, hi = nums.length - 1;
    while (lo < hi) {
        int sum = nums[lo] + nums[hi];
        int left = nums[lo], right = nums[hi];
        if (sum < target) {
            while (lo < hi && nums[lo] == left) lo++;
        } else if (sum > target) {
            while (lo < hi && nums[hi] == right) hi--;
        } else {
            List<Integer> temp = new ArrayList<>();
            temp.add(left);
            temp.add(right);
            res.add(temp);
            while (lo < hi && nums[lo] == left) lo++;
            while (lo < hi && nums[hi] == right) hi--;
        }
    }
    return res;
}
```

## 4. nSum

```java
private List<List<Integer>> nSum(int[] nums, int n, int start, int target) {
    int size = nums.length;
    List<List<Integer>> res = new ArrayList<>();

    if (n < 2 || size < n) return res;
    if (n == 2) {
        int lo = start, hi = size - 1;
        while (lo < hi) {
            int sum = nums[lo] + nums[hi];
            int left = nums[lo], right = nums[hi];
            if (sum < target) {
                while (lo < hi && nums[lo] == left) lo++;
            } else if (sum > target) {
                while (lo < hi && nums[hi] == right) hi--;
            } else {
                List<Integer> temp = new ArrayList<>();
                temp.add(left);
                temp.add(right);
                res.add(temp);
                while (lo < hi && nums[lo] == left) lo++;
                while (lo < hi && nums[hi] == right) hi--;
            }
        }
    } else {
        for (int i = start; i < size; i++) {
            List<List<Integer>> sub = nSum(nums, n - 1, i + 1, target - nums[i]);
            for (List<Integer> t : sub) {
                t.add(0, nums[i]);
                res.add(t);
            }
            while (i < size - 1 && nums[i] == nums[i + 1]) i++;
        }
    }
    return res;
}
```

