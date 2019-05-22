> Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:
> 1. Only one letter can be changed at a time.
> 2. Each transformed word must exist in the word list. Note that beginWord is not a transformed word.
#### Example：
> Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
#### Note：
> - Return 0 if there is no such transformation sequence.
> - All words have the same length.
> - All words contain only lowercase alphabetic characters.
> - You may assume no duplicates in the word list.
> - You may assume beginWord and endWord are non-empty and are not the same.

#### 解释下题目：
> 给定一个开始的单词，一个结束的单词，然后在给定一个列表里面有一堆单词，变化规则是只能从里面找。


## 1. 广度优先算法，队列实现
#### 实际耗时：577ms
```
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (beginWord == null || beginWord.length() == 0 || endWord == null || endWord.length() == 0) {
            return 0;
        }

        if (!wordList.contains(endWord)) {
            return 0;
        }

        if (beginWord.length() != endWord.length()) {
            return 0;
        }


        int length = beginWord.length();

        //正式开始算法，首先需要对list进行一个预处理
        //创建一个字典，其中的每个key就是wordList中的词经过一个字母变换能够得到的值，而value就是对应的key所能够到达的
        //打个比方 "*og"=[dog, log, cog]
        HashMap<String, ArrayList<String>> allComboDict = new HashMap<>(0);

        for (String word : wordList) {
            for (int i = 0; i < length; i++) {
                String newWord = word.substring(0, i) + "*" + word.substring(i + 1);
                if (allComboDict.containsKey(newWord)) {
                    allComboDict.get(newWord).add(word);
                } else {
                    ArrayList<String> tmp = new ArrayList<>();
                    tmp.add(word);
                    allComboDict.put(newWord, tmp);
                }
            }
        }
        //预处理完毕

        //下面的pair就是一个键值对的类
        Queue<Pair<String, Integer>> queue = new LinkedList<>();
        queue.add(new Pair<>(beginWord, 1));

        //这个列表存放word是否访问过
        ArrayList<String> visited = new ArrayList<>();
        visited.add(beginWord);

        //BFS的核心算法
        while (!queue.isEmpty()) {
            Pair<String, Integer> node = queue.remove();
            String word = node.getKey();
            int level = node.getValue();
            for (int i = 0; i < length; i++) {
                String newWord = word.substring(0, i) + '*' + word.substring(i + 1, length);
                for (String adjacentWord : allComboDict.getOrDefault(newWord, new ArrayList<>())) {
                    if (adjacentWord.equals(endWord)) {
                        return level + 1;
                    }

                    //下面是优化的部分，防止大规模循环或者形成环
                    if (!visited.contains(adjacentWord)) {
                        visited.add(adjacentWord);
                        queue.add(new Pair(adjacentWord, level + 1));
                    }
                }
            }
        }

        return 0;
    }
```
&emsp;&emsp;思路都在注释里了，巧妙的是通过*来进行跳转，广度优先算法没什么好说的
###### 时间复杂度O(nm) n是wordList中单词个数，m是单词的长度
###### 空间复杂度O(nm)
---------
