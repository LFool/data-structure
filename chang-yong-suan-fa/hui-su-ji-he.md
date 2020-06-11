# 回溯集合

## 1. Subsets

{% embed url="https://leetcode.com/problems/subsets/" %}

```java
public List<List<Integer>> subsets(int[] nums) {
    
    List<List<Integer>> res = new ArrayList<>();
    
    backtrack(res, new ArrayList<>(), nums, 0);
    
    return res;
}

private void backtrack (List<List<Integer>> list, 
                        List<Integer> track, int[] nums, int start) {
    
    list.add(new ArrayList<>(track));
    
    for (int i = start; i < nums.length; i++) {
        
        if (track.contains(nums[i])) continue;
        
        track.add(nums[i]);
        backtrack(list, track, nums, i + 1);
        track.remove(track.size() - 1);
    }
}
```



