> Given a string *s1*, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.
#### Example：
> Input: s1 = "great", s2 = "rgeat"
Output: true

#### 解释下题目：
> 给定一个单词，可以把这个单词在任意的地方一分为二，然后构成两颗子树，然后对子树继续如此操作，就可以得到一棵树。选择这棵树的任意一个非叶子节点，交换这个节点的两个子树，最后会得到一个新的单词，这个单词就叫原单词的Scramble String，然后可以继续这么操作下去得到一个单词的所有Scramble String。


## 1. 递归
#### 实际耗时：2ms
```
public boolean isScramble(String s1, String s2) {
        if (s1.equals(s2)) return true;

        int[] letters = new int[26];

        //确保每个字母出现的频率一致
        for (int i = 0; i < s1.length(); i++) {
            letters[s1.charAt(i) - 'a']++;
            letters[s2.charAt(i) - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (letters[i] != 0) {
                return false;
            }
        }

        for (int i = 1; i < s1.length(); i++) {
            if (isScramble(s1.substring(0, i), s2.substring(0, i))
                    && isScramble(s1.substring(i), s2.substring(i))) return true;
            if (isScramble(s1.substring(0, i), s2.substring(s2.length() - i))
                    && isScramble(s1.substring(i), s2.substring(0, s2.length() - i))) return true;
        }
        return false;
    }
```
&emsp;&emsp;一开始拿到这道题的时候，我一开始想的是按照题目的意思，对一个单词构造二叉树，然后进行交换操作，老老实实得到一个单词的所有解。但是后来发现复杂度太高。最后没办法去网上找了上面的解。它的思路其实很简单，就是如果s1和s2如果是Scramble String，那么将它们在相同的地方一分为二，必须满足以下的两个条件之一：
1. s1的前半部分和s2的前半部分必须出现的单词频率一致且它们的后半部分的单词出现频率也一致
2. s1的前半部分和s2的后半部分必须出现的单词频率一致且s1的后半部分和s2的前半部分的单词出现频率一致
数学证明.....这题如果这么做其实就没意思了。
---------
