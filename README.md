# FAANG Algorithms Interview Using Java Stream API and HashMap/HashSet

## 1. Two Sum 
### Description: Return indices of two numbers that add up to target.
```
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) return new int[] { map.get(complement), i };
        map.put(nums[i], i);
    }
    return new int[0];
}
```

## 2. Group Anagrams
### Description: Group all strings that are anagrams of each other.  
```
public List<List<String>> groupAnagrams(String[] strs) {
    return new ArrayList<>(Arrays.stream(strs)
        .collect(Collectors.groupingBy(s -> {
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            return new String(chars);
        }))
        .values());
}
```

## 3. Top K Frequent Elements 
### Description: Return the k most frequent elements in the array.   
```
public int[] topKFrequent(int[] nums, int k) {
    return Arrays.stream(nums)
        .boxed()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
        .entrySet().stream()
        .sorted(Map.Entry.<Integer, Long>comparingByValue().reversed())
        .limit(k)
        .mapToInt(Map.Entry::getKey)
        .toArray();
}
```

## 4. First Unique Character in a String 
### Description: Return the index of the first character that appears only once (-1 if none).  
```
public int firstUniqChar(String s) {
    Map<Character, Integer> counts = new HashMap<>();
    for (char c : s.toCharArray()) counts.put(c, counts.getOrDefault(c, 0) + 1);
    return IntStream.range(0, s.length())
        .filter(i -> counts.get(s.charAt(i)) == 1)
        .findFirst().orElse(-1);
}
```

## 5. Contains Duplicate 
### Description: Return true if any value appears at least twice in the array.  
```
public boolean containsDuplicate(int[] nums) {
    return Arrays.stream(nums)
        .distinct()
        .count() != nums.length;
}
```

## 6. Valid Anagram 
### Description: Return true if t is an anagram of s.
```
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    Map<Character, Long> sm = s.chars()
        .mapToObj(c -> (char)c)
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    Map<Character, Long> tm = t.chars()
        .mapToObj(c -> (char)c)
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    return sm.equals(tm);
}
```

## 7. Subarray Sum Equals K 
### Description: Return the total number of subarrays whose sum equals k.  
```
public int subarraySum(int[] nums, int k) {
    int count = 0, sum = 0;
    Map<Integer, Integer> preSum = new HashMap<>();
    preSum.put(0, 1);
    for (int n : nums) {
        sum += n;
        if (preSum.containsKey(sum - k)) count += preSum.get(sum - k);
        preSum.put(sum, preSum.getOrDefault(sum, 0) + 1);
    }
    return count;
}
```

## 8. Intersection of Two Arrays 
### Description: Return an array of elements that appear in both input arrays (any order).  
```
public int[] intersection(int[] nums1, int[] nums2) {
    Set<Integer> set = Arrays.stream(nums1)
        .boxed()
        .collect(Collectors.toSet());
    return Arrays.stream(nums2)
        .distinct()
        .filter(set::contains)
        .toArray();
}
```

## 9. Word Pattern 
### Description: Return true if the string follows the same pattern as the given pattern.  
```
public boolean wordPattern(String pattern, String s) {
    String[] words = s.split(" ");
    if (words.length != pattern.length()) return false;
    Map index = new HashMap();
    for (Integer i = 0; i < words.length; ++i)
        if (index.put(pattern.charAt(i), i) != index.put(words[i], i)) return false;
    return true;
}
```

## 10. Majority Element 
### Description: Return the element that appears more than n/2 times.
```
public int majorityElement(int[] nums) {
    return Arrays.stream(nums).boxed()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
        .entrySet().stream()
        .max(Map.Entry.comparingByValue())
        .get()
        .getKey();
}
```

## 11. Sort Characters By Frequency
### Description: Return the string with characters sorted by descending frequency. 
```
public String frequencySort(String s) {
    Map<Character, Long> map = s.chars()
        .mapToObj(c -> (char)c)
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    return map.entrySet().stream()
        .sorted(Map.Entry.<Character, Long>comparingByValue().reversed())
        .map(e -> String.valueOf(e.getKey()).repeat(e.getValue().intValue()))
        .collect(Collectors.joining());
}
```

## 12. Longest Substring Without Repeating Characters 
### Description: Return the length of the longest substring without repeating characters. 
```
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> map = new HashMap<>();
    int max = 0;
    for (int j = 0, i = 0; j < s.length(); j++) {
        if (map.containsKey(s.charAt(j))) i = Math.max(map.get(s.charAt(j)), i);
        max = Math.max(max, j - i + 1);
        map.put(s.charAt(j), j + 1);
    }
    return max;
}
```

## 13. Jewels and Stones 
### Description: Return how many stones are also jewels (count characters in jewels are unique). 
```
public int numJewelsInStones(String jewels, String stones) {
    Set<Character> jewelSet = jewels.chars()
        .mapToObj(c -> (char)c)
        .collect(Collectors.toSet());
    return (int) stones.chars()
        .filter(c -> jewelSet.contains((char)c))
        .count();
}
```

## 14. All Anagrams in a String 
### Description: Return starting indices of all anagrams of p in s.  
```
public List<Integer> findAnagrams(String s, String p) {
    Map<Character, Long> pMap = p.chars()
        .mapToObj(c -> (char)c)
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    List<Integer> result = new ArrayList<>();
    for (int i = 0; i <= s.length() - p.length(); i++) {
        String sub = s.substring(i, i + p.length());
        Map<Character, Long> sMap = sub.chars()
            .mapToObj(c -> (char)c)
            .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
        if (pMap.equals(sMap)) result.add(i);
    }
    return result;
}
```

## 15. Roman to Integer 
### Description: Convert a Roman numeral string to an integer.       
```
public int romanToInt(String s) {
    Map<Character, Integer> map = Map.of('I', 1, 'V', 5, 'X', 10, 'L', 50, 'C', 100, 'D', 500, 'M', 1000);
    int res = 0, prev = 0;
    for (int i = s.length() - 1; i >= 0; i--) {
        int curr = map.get(s.charAt(i));
        res += (curr < prev) ? -curr : curr;
        prev = curr;
    }
    return res;
}
```

## 16. Valid Sudoku
### Description: Return true if the 9×9 Sudoku board is valid (no conflicts in rows/cols/boxes).
```
public boolean isValidSudoku(char[][] board) {
    Set<String> seen = new HashSet<>();
    return IntStream.range(0, 9)
        .allMatch(i ->IntStream.range(0, 9)
            .allMatch(j -> {
                char c = board[i][j];
                if (c == '.') return true;
                return seen.add(c + "row" + i) &&
                       seen.add(c + "col" + j) &&
                       seen.add(c + "block" + i/3 + j/3);
            })
    );
}
```

## 17. Contiguous Array
### Description: Return the longest subarray length with equal number of 0s and 1s. 
```
public int findMaxLength(int[] nums) {
    Map<Integer, Integer> firstIndex = new HashMap<>();
    firstIndex.put(0, -1);
    int balance = 0;
    return IntStream.range(0, nums.length)
            .reduce(0, (max, i) -> {
                balance += nums[i] == 0 ? -1 : 1;
                firstIndex.putIfAbsent(balance, i);
                return Math.max(max, i - firstIndex.get(balance));
            });
}
```

## 18. 4 Sum 
### Description: Return all unique quadruplets in the array that sum to target. 
```
public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
    Map<Integer, Long> sum12 = Arrays.stream(nums1)
            .boxed()
            .flatMap(a -> Arrays.stream(nums2)
                .mapToObj(b -> a + b))
            .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

    return (int) Arrays.stream(nums3)
            .boxed()
            .flatMap(c -> Arrays.stream(nums4)
                .mapToObj(d -> -(c + d)))
            .mapToLong(need -> sum12.getOrDefault(need, 0L))
            .sum();
}
```

## 19. Minimum Window Substring 
### Description: Return the smallest substring of s that contains all characters of t (including duplicates).
```
public String minWindow(String s, String t) {
    Map<Character, Long> targetCount = t.chars()
            .mapToObj(c -> (char) c)
            .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

    Map<Character, Long> windowCount = new HashMap<>();
    int[] best = {-1, 0, 0}; 

    IntStream.range(0, s.length())
        .forEach(right -> {
            char c = s.charAt(right);
            windowCount.merge(c, 1L, Long::sum);
            while (isValid(windowCount, targetCount)) {
                if (best[0] == -1 || right - best[1] + 1 < best[0]) {
                    best[0] = right - best[1] + 1;
                    best[1] = left;
                    best[2] = right;
                }
                char leftChar = s.charAt(left++);
                windowCount.merge(leftChar, -1L, Long::sum);
                windowCount.remove(leftChar, 0L);
            }
    });

    return best[0] == -1
        ? ""
        : s.substring(best[1], best[2] + 1);
}

private boolean isValid(Map<Character, Long> window, Map<Character, Long> target) {
    return target.entrySet().stream()
            .allMatch(e -> window.getOrDefault(e.getKey(), 0L) >= e.getValue());
} 
```
