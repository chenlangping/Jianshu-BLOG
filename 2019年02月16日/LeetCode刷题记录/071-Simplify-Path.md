> Given an **absolute path** for a file (Unix-style), simplify it. Or in other words, convert it to the **canonical path**.
In a UNIX-style file system, a period `.` refers to the current directory. Furthermore, a double period `..` moves the directory up a level. For more information, see: [Absolute path vs relative path in Linux/Unix](https://www.linuxnix.com/abslute-path-vs-relative-path-in-linuxunix/)
Note that the returned canonical path must always begin with a slash `/`, and there must be only a single slash `/` between two directory names. The last directory name (if it exists) **must not** end with a trailing `/`. Also, the canonical path must be the **shortest** string representing the absolute path.

#### Example：
> Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.

> Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.

> Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.

> Input: "/a/./b/../../c/"
Output: "/c"

> Input: "/a/../../b/../c//.//"
Output: "/c"

> Input: "/a//b////c/d//././/.."
Output: "/a/b/c"

#### 解释下题目：
> 给定一个决定路径，将其简化


## 1. 利用堆栈
#### 实际耗时：15ms
```
public String simplifyPath(String path) {
        Deque<String> stack = new LinkedList<>();
        Set<String> skip = new HashSet<>(Arrays.asList("..", ".", ""));
        for (String dir : path.split("/")) {
            //利用 "/"作为分隔符来分割
            if (dir.equals("..") && !stack.isEmpty()) {
                //如果是退回到上一级且栈中有东西，那么弹出
                stack.pop();
            } else if (!skip.contains(dir)) {
                //判断下，如果不是本层或者没内容，那么就加入
                stack.push(dir);
            }
        }
        StringBuilder sb = new StringBuilder();
        for (String dir : stack) {
            //还原操作
            sb.insert(0, dir);
            sb.insert(0, "/");

        }
        return sb.toString().isEmpty() ? "/" : sb.toString();
    }
```
###### 踩过的坑：一开始还原的时候弄反了
&emsp;&emsp;思路很简单，就是利用堆栈来进行处理。
###### 时间复杂度O(n)  n为一共多少层
###### 空间复杂度O(n)  n为一共多少层
