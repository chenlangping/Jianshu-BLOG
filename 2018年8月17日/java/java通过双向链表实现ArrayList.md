##### 主要通过双向链表实现ArrayList
首先是封装了链表的节点：
```
public class Node {
    private Node previous;
    private Object object;
    private Node next;

    public Node(Object object) {
        this.object = object;
    }

    public Node() {

    }

    public Node getPrevious() {
        return previous;
    }

    public Object getObject() {
        return object;
    }

    public Node getNext() {
        return next;
    }

    public void setPrevious(Node previous) {
        this.previous = previous;
    }

    public void setObject(Object object) {
        this.object = object;
    }

    public void setNext(Node next) {
        this.next = next;
    }
}
```
很简单，每个Node节点包括指向前一个和后一个的“指针”以及内部存的数据Object，并且有一个带参构造器和一个无参构造器。以及一堆set和get方法。

---------
#### 双向链表部分：
主要实现了以下方法：
```
public void add(Object object)
public void add(int index, Object object)
public boolean remove(int index)
public boolean remove(Object object)
public void set(int index, Object object)
public Object get(int index)
public Node getNode(int index)
public void print()
public int getSize()
private void checkRange(int index)
```

#### 详细代码：
```
public class myLinkedList {
    private Node first;
    private Node last;
    private int size;

    /**
     * 新增一个节点
     *
     * @param object 新增节点所带的对象
     */
    public void add(Object object) {
        Node node = new Node();
        if (null == first) {
            //如果是第一个节点
            node.setPrevious(null);
            node.setObject(object);
            node.setNext(null);
            first = node;
            last = node;
        } else {
            node.setPrevious(last);
            node.setObject(object);
            node.setNext(null);
            last.setNext(node);
            last = node;
        }
        size++;
    }

    /**
     * 在指定位置插入
     *
     * @param index  数组下标，最大可等于当前链表长度
     * @param object 插入的对象
     */
    public void add(int index, Object object) {
        if (index < 0 || size < index) {
            System.out.println("下标错误！");
        }
        if (index == size) {
            Node newNode = new Node(object);
            last.setNext(newNode);
            newNode.setPrevious(last);
            last = newNode;
            return;
        }
        Node node = getNode(index);
        Node newNode = new Node(object);
        if (node.getPrevious() == null) {
            //此时Node是第一个，也就是first
            first.setPrevious(newNode);
            newNode.setNext(first);
            first = newNode;
            first.setPrevious(null);
            return;
        }
        node.getPrevious().setNext(newNode);
        newNode.setNext(node);

    }

    /**
     * 删除指定下标的节点
     *
     * @param index 下标
     * @return 成功删除返回true
     */
    public boolean remove(int index) {
        checkRange(index);
        Node node = first;
        int count = 0;
        if (node == null) {
            return false;
        } else {
            if (index == 0) {
                first = first.getNext();
                size--;
                return true;
            }
            while (node.getNext() != null) {
                count++;
                node = node.getNext();
                if (count == index) {
                    //删除操作
                    node.getPrevious().setNext(node.getNext());
                    size--;
                    return true;
                }
            }
        }
        return false;
    }

    /**
     * 删除链表中第一个指定的对象
     *
     * @param object
     * @return true=删除成功；false=删除失败
     */
    public boolean remove(Object object) {
        Node node = new Node();
        node = first;
        while (node.getNext() != null) {
            if (node.getObject().equals(object)) {
                //进行删除操作
                node.getPrevious().setNext(node.getNext());
                size--;
                return true;
            }
            node = node.getNext();
        }
        return false;
    }

    /**
     * 将指定下标的元素修改成指定的对象
     *
     * @param index  下标
     * @param object 对象
     */
    public void set(int index, Object object) {
        checkRange(index);
        Node node = getNode(index);
        node.setObject(object);
    }

    /**
     * 返回指定位置的对象
     *
     * @param index 下标，从0开始
     * @return 对应位置的元素
     */
    public Object get(int index) {
        checkRange(index);
        int count = 0;
        if (first == null) {
            return null;
        } else {
            Node temp = first;
            if (index == 0) {
                return temp.getObject();
            }
            while (temp.getNext() != null) {
                temp = temp.getNext();
                count++;
                if (index == count) {
                    return temp.getObject();
                }
            }
            return null;
        }
    }

    /**
     * 返回指定位置处的节点
     *
     * @param index 下标
     * @return 如果存在就返回节点，否则返回空
     */
    public Node getNode(int index) {
        checkRange(index);
        int count = 0;
        Node node = first;
        if (index == 0) {
            return first;
        } else {
            while (node.getNext() != null) {
                node = node.getNext();
                count++;
                if (index == count) {
                    return node;
                }
            }

            return null;
        }
    }

    /**
     * 打印整个链表
     */
    public void print() {
        Node node = first;
        while (node != null) {
            System.out.println(node.getObject());
            node = node.getNext();
        }
    }

    /**
     * 返回当前链表长度
     *
     * @return 当前链表长度
     */
    public int getSize() {
        return size;
    }

    /**
     * 确认传入的下标是否合法
     *
     * @param index
     */
    private void checkRange(int index) {
        if (index < 0 || size <= index) {
            System.out.println("下标错误");
            try {
                throw new Exception();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    
}
```
#### 详解部分：
`public void add(Object object)`
> 将一个对象顺次加到链表最后面。
首先构造一个新的节点。
然后需要判断这个节点是不是整条链的第一个。如果是，那么就把first和last都指向它，然后这个节点的前一个和后一个都是Null。
如果它不是第一个，那么就简单了，只需要把它和last连在一起，最后把last指向它即可。
不要忘记因为是add方法，所以size++

<br>
`public void add(int index, Object object)`
> 在指定位置加入指定对象。
首先肯定需要判断index是否合法。但是这里无法使用之前的`checkRange()`，这是因为我认为一个长度为5的数组，你可以在5这个位置插入，意味着在最后加入，所以单独进行了判断。
之后就是照理根据index找到对应的节点，然后把之前的链表连接到这个新的节点，新的节点链接进去即可。
最后不要忘记size++

<br>
`public boolean remove(int index)`
> 下标判断，然后直接找到index位置的节点，把它之前的节点的下一个指向它之后的那个，它就会被删除。
不要忘记size--

<br>
`public boolean remove(Object object)`
> 只能遍历判断，用equals方法找到之后直接用之前的删除代码
不要忘记size--

<br>
`public void set(int index, Object object)`
> 判断index合法，之后找到那个节点，使用setObject方法设置即可。

<br>
`public Object get(int index)`
> 判断index合法之后，找到节点并返回它对应的值。

<br>
` public Node getNode(int index)`
> 返回指定index处的节点对象。

<br>
` public void print()`
> 打印整个链表

<br>
`public int getSize()`
`private void checkRange(int index)`
> 这俩不解释
