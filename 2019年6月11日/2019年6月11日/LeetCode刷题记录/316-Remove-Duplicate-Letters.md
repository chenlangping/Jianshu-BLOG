> Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.


#### Example：
> Input: "bcabc"
Output: "abc"

> Input: "cbacdcbc"
Output: "acdb"

#### 解释下题目：
> 题目其实很简单，就是把重复的删除，然后因为删除可能有很多种可以删除的选择，留下字典顺序最小的。举个例子，`babc`明显要删除一个b，所以如果选择删除第一个b，剩下的是`abc`，如果选择第二个b，那么留下的是`bac`，显然是`abc`<`bac`。

## 1. 错误的贪心算法
#### 实际耗时：错误的
```java
public String removeDuplicateLetters(String s) {
        int position = 0;
        HashMap<Integer, Character> result = new HashMap<>();
        for (char c = 'a'; c <= 'z'; c++) {
            if (s.indexOf(c) != -1) {
                //存在这个字符，那么先在position之后找有没有它的位子
                for (int i = position; i < s.length(); i++) {
                    if (s.charAt(i) == c) {
                        position = i;
                        break;
                    }
                }
                //如果后面没找到
                if (s.charAt(position) != c) {
                    position = s.indexOf(c);
                }

                result.put(position, c);
            } else {
                //不存在这个字符，什么都不用做
            }
        }

        for (Map.Entry<Integer, Character> entry : result.entrySet()) {
            System.out.println(entry.getKey() + " " + entry.getValue());
        }

        return "";
    }
```
&emsp;&emsp;思路，这里先假设一定有abc字符（因为换成其他的也成立），然后我首先要找a，肯定希望a越前面越好，这样后面所剩下的空间越大。OK现在有了第一个字符位置确定了，第二个b有几种可能：
1. b只有在a之后才有，这样的话肯定留下离a距离最近的，理由同选择a的时候一样。
2. b只有在a之前才有，那么就要使b在数组越前面越好。理由同选择a的时候一样。
3. b在a前后都有。我错就错在这里，其实这种情况是需要讨论的，我错误的解法就认为肯定要选择在a之后且离a最近的那个b，这就导致了可能会出现c的位置放不好了。解决方法是用一个数组记录下有没有出现过。
举个会发生错误的例子：`cbacdcbc`，肉眼看一下答案应该是`acdb`，但是算法的结果是`adbc`，判断过程简单写一下：首先a肯定没问题，然后b按照思路会选择倒数第二个b，这也没问题，问题在于c的选择。按照我的算法会选择在b之后且离b最近的那个c，这就导致了d会出现在bc之前，就是没考虑好，具体解决见方法2。
###### 时间复杂度O(n) n为string的长度
###### 空间复杂度O(1)
---------
## 2.利用堆栈实现
#### 实际耗时：3ms
```
 public String removeDuplicateLetters2(String s) {
        Stack<Character> stack = new Stack<>();

        //1.计算每个字母出现的次数
        int[] count = new int[26];
        char[] arr = s.toCharArray();
        for (char c : arr) {
            count[c - 'a']++;
        }

        boolean[] visited = new boolean[26];

        //2. 对每个字母进行处理
        for (char c : arr) {
            count[c - 'a']--;

            //已经处理过这个字符了，无需处理
            if (visited[c - 'a']) {
                continue;
            }

            //3. 如果顶端的那个字母比现在的这个小，如堆栈的顶端是c，而目前的是a，且c之后还会出现的话，那这个c我就不要了
            while (!stack.isEmpty() && stack.peek() > c && count[stack.peek() - 'a'] > 0) {
                visited[stack.peek() - 'a'] = false;
                stack.pop();
            }
            stack.push(c);
            visited[c - 'a'] = true;
        }

        StringBuilder sb = new StringBuilder();
        for (char c : stack) {
            sb.append(c);
        }
        return sb.toString();
    }
```

&emsp;&emsp;思路很简单，就跟捡东西一样，如果我之后还能捡到，那我现在就可以先不要，仔细理解一下3.的注释就可以了。
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
