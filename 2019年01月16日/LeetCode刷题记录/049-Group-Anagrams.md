> Given an array of strings, group anagrams together.
#### Example：
> Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
#### Note：
> All inputs will be in lowercase.
The order of your output does not matter.

#### 解释下题目：
> 给定一个字符串数组，把其中是相同字母异序词的放到一个list里面。


## 1. 那就一个一个遍历呗
#### 实际耗时：697ms
```
public List<List<String>> groupAnagrams(String[] strs) {
        boolean flag = true;
        List<List<String>> res = new ArrayList<>();
        if (strs.length == 0) {
            return res;
        }
        for (String s : strs) {
            flag = true;
            for (int i = 0; i < res.size(); i++) {
                List<String> tmp = res.get(i);
                if (isAnagram(tmp.get(0), s)) {
                    tmp.add(s);
                    flag = false;
                    break;
                }
            }
            if (flag) {
                List<String> tmp = new ArrayList<>();
                tmp.add(s);
                res.add(tmp);
            }
        }
        return res;
    }

    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            //长度都不相同还怎么可能是
            return false;
        }
        int[] arr = new int[26];

        for (int i = 0; i < s.length(); i++) {
            arr[s.charAt(i) - 'a']++;
            arr[t.charAt(i) - 'a']--;
        }
        for (int i = 0; i < s.length(); i++) {
            if (arr[s.charAt(i) - 'a'] != 0) {
                return false;
            }
        }

        return true;
    }
```
&emsp;&emsp;思路很简单，每取一个词，就去每一组的第一个对比看看，如果是就放进去，不是就下一个，如果全部都遍历完都没找到的话，另外给它起一个组。
###### 时间复杂度O(n^3)
###### 空间复杂度O(n)
---------
## 2. 网上找的解法
#### 实际耗时：19ms
```
public static List<List<String>> groupAnagrams2(String[] strs) {
        int[] prime = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103};
        //最多10609个z
        // prime.length = 26

        List<List<String>> res = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();
        for (String s : strs) {
            int key = 1;
            for (char c : s.toCharArray()) {
                key *= prime[c - 'a'];
            }
            List<String> t;
            if (map.containsKey(key)) {
                t = res.get(map.get(key));
            } else {
                t = new ArrayList<>();
                res.add(t);
                map.put(key, res.size() - 1);
            }
            t.add(s);
        }
        return res;
    }
```
&emsp;&emsp;思路很巧，这个解法是利用26个素数，这样你任何几个字母相乘都是独一无二的解，然后配合hashmap可以极大加快查找对应的组。
###### 时间复杂度O(n*m)  n是单词的个数，m是单词的长度
###### 空间复杂度O(n*m)
---------
