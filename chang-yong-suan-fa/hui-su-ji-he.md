# 回溯集合

## 1. Subsets

{% embed url="https://leetcode.com/problems/subsets/" %}

```java
public List<List<Integer>> subsets(int[] nums) {
    
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(res, new ArrayList<>(), nums, 0);
    
    return res;
}

private void backtrack (List<List<Integer>> list, 
                        List<Integer> track, int[] nums, int start) {
    
    list.add(new ArrayList<>(track));
    
    for (int i = start; i < nums.length; i++) {
        track.add(nums[i]);
        backtrack(list, track, nums, i + 1);
        track.remove(track.size() - 1);
    }
}
```

## 2. Subsets II \(contains duplicates\)

{% embed url="https://leetcode.com/problems/subsets-ii/" %}

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {

    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(res, new ArrayList<>(), nums, 0);

    return res;
}

private void backtrack (List<List<Integer>> list, 
                        List<Integer> track, int[] nums, int start) {

    list.add(new ArrayList<>(track));

    for (int i = start; i < nums.length; i++) {

        if (i > start && nums[i] == nums[i - 1]) continue;

        track.add(nums[i]);
        backtrack(list, track, nums, i + 1);
        track.remove(track.size() - 1);
    }
}
```

## 3. Permutations

{% embed url="https://leetcode.com/problems/permutations/" %}

```java
public List<List<Integer>> permute(int[] nums) {

    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(res, new ArrayList<>(), nums);

    return res;
}

private void backtrack (List<List<Integer>> list, 
                        List<Integer> track, int[] nums) {

    if (track.size() == nums.length) list.add(new ArrayList<>(track));

    for (int i = 0; i < nums.length; i++) {

        if (track.contains(nums[i])) continue;

        track.add(nums[i]);
        backtrack(list, track, nums);
        track.remove(track.size() - 1);
    }
}
```

