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

    if (track.size() == nums.length) {
        list.add(new ArrayList<>(track));
        return ;
    }

    for (int i = 0; i < nums.length; i++) {

        if (track.contains(nums[i])) continue;

        track.add(nums[i]);
        backtrack(list, track, nums);
        track.remove(track.size() - 1);
    }
}
```

## 4. Permutations II \(contains duplicates\)

{% embed url="https://leetcode.com/problems/permutations-ii/" %}

```java
public List<List<Integer>> permuteUnique(int[] nums) {

    List<List<Integer>> res = new ArrayList<>();

    Arrays.sort(nums);

    backtrack(res, new ArrayList<>(), new boolean[nums.length], nums);

    return res;

}

private void backtrack(List<List<Integer>> list, List<Integer> track,
                      boolean[] used, int[] nums) {

    if (track.size() == nums.length) {
        list.add(new ArrayList<>(track));
        return ;
    }
    for (int i = 0; i < nums.length; i++) {

        // 如果当前元素已使用，跳过
        if (used[i]) continue;
        // 如果当前元素和上一个元素相等，上一个元素没有用，当前元素也不能用
        if(i > 0 && nums[i] == nums[i-1] && !used[i - 1]) continue;

        track.add(nums[i]);
        used[i] = true;

        backtrack(list, track, used, nums);

        track.remove(track.size() - 1);
        used[i] = false;
    }

}
```

## 5. Combination Sum

{% embed url="https://leetcode.com/problems/combination-sum/" %}

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();

    Arrays.sort(candidates);

    backtrack(res, new ArrayList<>(), candidates, target, 0);

    return res;

}

private void backtrack(List<List<Integer>> list, List<Integer> track, 
                       int[] candidates, int target, int start) {

    if (target == 0) {
        list.add(new ArrayList<>(track));
        return ;
    }
    if (target < 0) return ;

    for (int i = start; i < candidates.length; i++) {

        track.add(candidates[i]);

        backtrack(list, track, candidates, target - candidates[i], i);
        
        track.remove(track.size() - 1);
    }
}
```

## 6. Combination Sum II \(can't reuse same element\)

{% embed url="https://leetcode.com/problems/combination-sum-ii/" %}

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {

    List<List<Integer>> res = new ArrayList<>();

    Arrays.sort(candidates);

    backtrack(res, new ArrayList<>(), new boolean[candidates.length], candidates, target, 0);

    return res;

}

private void backtrack(List<List<Integer>> list, List<Integer> track, 
                       boolean[] used, int[] candidates, int target, int start) {
    if (target == 0) {
        list.add(new ArrayList<>(track));
        return ;
    }
    if (target < 0) return ;

    for (int i = start; i < candidates.length; i++) {

        if (used[i]) continue;
        if (i > start && candidates[i] == candidates[i - 1] && !used[i - 1]) continue;

        used[i] = true;
        track.add(candidates[i]);

        backtrack(list, track, used, candidates, target - candidates[i], i + 1);

        used[i] = false;
        track.remove(track.size() - 1);

    }
}
```

