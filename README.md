# algorithms faang java stream api

## 1. Two Sum 
```public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) return new int[] { map.get(complement), i };
        map.put(nums[i], i);
    }
    return new int[0];
}```

## 2. Group Anagrams
```public List<List<String>> groupAnagrams(String[] strs) {
    return new ArrayList<>(Arrays.stream(strs)
        .collect(Collectors.groupingBy(s -> {
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            return new String(chars);
        })).values());
}```

## 3. Top K Frequent Elements 
```public int[] topKFrequent(int[] nums, int k) {
    return Arrays.stream(nums)
        .boxed()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
        .entrySet().stream()
        .sorted(Map.Entry.<Integer, Long>comparingByValue().reversed())
        .limit(k)
        .mapToInt(Map.Entry::getKey)
        .toArray();
}```

## 4. First Unique Character in a String 
```public int firstUniqChar(String s) {
    Map<Character, Integer> counts = new HashMap<>();
    for (char c : s.toCharArray()) counts.put(c, counts.getOrDefault(c, 0) + 1);
    return IntStream.range(0, s.length())
        .filter(i -> counts.get(s.charAt(i)) == 1)
        .findFirst().orElse(-1);
}```

## 5. Contains Duplicate 
```public boolean containsDuplicate(int[] nums) {
    return Arrays.stream(nums).distinct().count() != nums.length;
}```

## 6. Valid Anagram 
```public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    Map<Character, Long> sm = s.chars().mapToObj(c -> (char)c).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    Map<Character, Long> tm = t.chars().mapToObj(c -> (char)c).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    return sm.equals(tm);
}```

## 7. Subarray Sum Equals K 
```public int subarraySum(int[] nums, int k) {
    int count = 0, sum = 0;
    Map<Integer, Integer> preSum = new HashMap<>();
    preSum.put(0, 1);
    for (int n : nums) {
        sum += n;
        if (preSum.containsKey(sum - k)) count += preSum.get(sum - k);
        preSum.put(sum, preSum.getOrDefault(sum, 0) + 1);
    }
    return count;
}```

## 8. Intersection of Two Arrays 
```public int[] intersection(int[] nums1, int[] nums2) {
    Set<Integer> set = Arrays.stream(nums1).boxed().collect(Collectors.toSet());
    return Arrays.stream(nums2).distinct().filter(set::contains).toArray();
}```

## 9. Word Pattern 
```public boolean wordPattern(String pattern, String s) {
    String[] words = s.split(" ");
    if (words.length != pattern.length()) return false;
    Map index = new HashMap();
    for (Integer i = 0; i < words.length; ++i)
        if (index.put(pattern.charAt(i), i) != index.put(words[i], i)) return false;
    return true;
}```

## 10. Majority Element 
```public int majorityElement(int[] nums) {
    return Arrays.stream(nums).boxed()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
        .entrySet().stream()
        .max(Map.Entry.comparingByValue())
        .get().getKey();
}```

## 11. Sort Characters By Frequency
```public String frequencySort(String s) {
    Map<Character, Long> map = s.chars().mapToObj(c -> (char)c).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    return map.entrySet().stream()
        .sorted(Map.Entry.<Character, Long>comparingByValue().reversed())
        .map(e -> String.valueOf(e.getKey()).repeat(e.getValue().intValue()))
        .collect(Collectors.joining());
}```

## 12. Longest Substring Without Repeating Characters 
```public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> map = new HashMap<>();
    int max = 0;
    for (int j = 0, i = 0; j < s.length(); j++) {
        if (map.containsKey(s.charAt(j))) i = Math.max(map.get(s.charAt(j)), i);
        max = Math.max(max, j - i + 1);
        map.put(s.charAt(j), j + 1);
    }
    return max;
}```

## 13. Jewels and Stones 
```public int numJewelsInStones(String jewels, String stones) {
    Set<Character> jewelSet = jewels.chars().mapToObj(c -> (char)c).collect(Collectors.toSet());
    return (int) stones.chars().filter(c -> jewelSet.contains((char)c)).count();
}```

## 14. Find All Anagrams in a String 
```public List<Integer> findAnagrams(String s, String p) {
    Map<Character, Long> pMap = p.chars().mapToObj(c -> (char)c).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    List<Integer> result = new ArrayList<>();
    for (int i = 0; i <= s.length() - p.length(); i++) {
        String sub = s.substring(i, i + p.length());
        Map<Character, Long> sMap = sub.chars().mapToObj(c -> (char)c).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
        if (pMap.equals(sMap)) result.add(i);
    }
    return result;
}```

## 15. Roman to Integer 
```public int romanToInt(String s) {
    Map<Character, Integer> map = Map.of('I', 1, 'V', 5, 'X', 10, 'L', 50, 'C', 100, 'D', 500, 'M', 1000);
    int res = 0, prev = 0;
    for (int i = s.length() - 1; i >= 0; i--) {
        int curr = map.get(s.charAt(i));
        res += (curr < prev) ? -curr : curr;
        prev = curr;
    }
    return res;
}```
