> You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring(s) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.
#### Example：
> Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoor" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.

> Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []


#### 解释下题目：
> 这题目我看了半天才看懂！就是给定一个字符串s，还有若干个单词的数组，每个单词都是相同的长度，然后它们有个全排列，找出这个全排列中的任何一种可能在字符串s中出现的位置。就是如果我的单词数组是["a","b","c"]，那么如果s中有abc,acb,bca,bac,cab,cba这六种可能中的任何一种符合，都要输出。（PS：肯定不能全排列的）


## 1. 利用hashmap
#### 实际耗时：225ms
```
public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res = new ArrayList<>();

        //有多少单词
        int numOfWord = words.length;

        if (numOfWord == 0) {
            return res;
        }

        //每个单词的长度 #题中说了每个单词长度一样
        int wordLen = words[0].length();

        //把所有单词放进map1里面
        Map<String, Integer> map1 = stringToMap(words, numOfWord);

        for (int i = 0; i < s.length() - wordLen * numOfWord + 1; i++) {
            Map<String, Integer> map2 = getMap(i, wordLen, numOfWord, s);
            if (compareMap(map1, map2)) {
                res.add(i);
            }
        }
        return res;
    }

    //用所给的字符串，指定开始，并且指定每个单词的长度，来构造map
    private Map<String, Integer> getMap(int start, int wordLen, int numOfWord, String s) {
        String words[] = new String[numOfWord];
        int j = 0;
        for (int i = start; i < start + numOfWord * wordLen; i = i + wordLen) {
            words[j] = s.substring(i, i + wordLen);
            j++;
        }
        return stringToMap(words, numOfWord);
    }

    private Map<String, Integer> stringToMap(String words[], int numOfWord) {
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < numOfWord; i++) {
            if (map.containsKey(words[i])) {
                int count = map.get(words[i]);
                count++;
                map.put(words[i], count);
            } else {
                map.put(words[i], 1);
            }
        }
        return map;
    }

    private boolean compareMap(Map<String, Integer> m1, Map<String, Integer> m2) {

        Iterator<Map.Entry<String, Integer>> iter1 = m1.entrySet().iterator();
        while (iter1.hasNext()) {
            Map.Entry<String, Integer> entry1 = iter1.next();
            int m1value = entry1.getValue() == null ? -1 : entry1.getValue();
            int m2value = m2.get(entry1.getKey()) == null ? -2 : m2.get(entry1.getKey());
            if (m1value != m2value) {
                return false;
            }
        }
        return true;
    }
```
&emsp;&emsp;思路是这样的，全排列是绝对不可取的！那就用hashmap来存呗，上面的abc就可以存成`{'a':1,'b':1,'c':1}`，然后去比较两个hashmap是否相同。[这篇文章](https://leetcode.windliang.cc/leetCode-30-Substring-with-Concatenation-of-All-Words.html)写得更详细，而且还有优化。
###### 时间复杂度O(n*m) n是s的长度，m是单词个数
###### 空间复杂度O(2*m) m是单词个数
---------
