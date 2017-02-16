### Top K Frequent Elements

Given a non-empty array of integers, return the k most frequent elements.

For example,
Given [1,1,1,2,2,3] and k = 2, return [1,2].

Note:
* You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
* Your algorithm's time complexity must be better than O(n log n), where n is the array's size.


#### Solution 1

Use a frequency bucket to sort it out. Brilliant!

```java
public class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();
        for (int n: nums) {
            map.put(n, map.getOrDefault(n, 0)+1);
        }

        List<Integer>[] bucket = new List[nums.length+1];
        for (int n: map.keySet()) {
            int freq = map.get(n);
            if (bucket[freq] == null)
                bucket[freq] = new ArrayList<>();
            bucket[freq].add(n);
        }

        ArrayList<Integer> result = new ArrayList<>();
        for (int i=bucket.length-1; i>0 && k>0; i--) {
            if (bucket[i] != null) {
                result.addAll(bucket[i]);
                k -= bucket[i].size();
            }
        }
        return result;
    }
}
```

#### Solution 2

Use maxHeap. Put entry into maxHeap so we can always poll a number with largest frequency.

```java
public class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int n: nums){
            map.put(n, map.getOrDefault(n,0)+1);
        }

        PriorityQueue<Map.Entry<Integer, Integer>> maxHeap =
                         new PriorityQueue<>((a,b)->(b.getValue()-a.getValue()));
        for(Map.Entry<Integer,Integer> entry: map.entrySet()){
            maxHeap.add(entry);
        }

        List<Integer> res = new ArrayList<>();
        while(res.size()<k){
            Map.Entry<Integer, Integer> entry = maxHeap.poll();
            res.add(entry.getKey());
        }
        return res;
    }
}
```
