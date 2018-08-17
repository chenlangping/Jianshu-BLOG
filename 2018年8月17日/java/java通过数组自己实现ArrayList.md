# java ArrayList实现
主要靠数组实现，成员变量就一个数组elementData和一个size用来记录数组的当前长度。
因为需要让这个ArrayList存一切对象，所以设置成Object，size的长度int绰绰有余
#### 主要实现了以下方法：<br>
```
public myArrayList()
public myArrayList(int initialCapacity)
public void add(Object obj)
public void add(int index,Object obj)
public void remove(int index)
public void remove(Object obj)
public void set(int index,Object obj)
public Object get(int index)

private void checkRange(int index)
private void ensureCapacity()
public void print()
public int getSize()
public boolean isEmpty()
```
#### 详细代码
```
public class myArrayList {
    private Object elementData[];
    private int size;

    /**
     * 无参构造器
     */
    public myArrayList() {
        this(10);
    }

    /**
     * 带参构造器
     *
     * @param initialCapacity 数组初始大小
     */
    public myArrayList(int initialCapacity) {
        if (initialCapacity < 0) {
            System.out.println("长度不可小于0");
            try {
                throw new Exception();
            } catch (Exception e) {
                e.printStackTrace();
            }
        } else {
            elementData = new Object[initialCapacity];
        }
    }

    /**
     * @param obj 在数组最后面增加一个元素
     */
    public void add(Object obj) {
        //可能会超过上限，先确保空间足够
        ensureCapacity();
        elementData[size] = obj;
        size++;
    }

    /**
     * 在数组指定位置加一个元素，之后的元素向后移动一位
     *
     * @param index 插入的位置，以0开始
     * @param obj
     */
    public void add(int index, Object obj) {
        //可能会超过上限，先确保空间足够
        ensureCapacity();
        //这里不可以使用checkRange()
        if (index < 0 || index > size) {
            System.out.println("下标错误");
            try {
                throw new Exception();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        System.arraycopy(elementData, index, elementData, index + 1, size - index);
        elementData[index] = obj;
        size++;
    }

    /**
     * 删除指定位置的元素
     *
     * @param index 数组下标
     */
    public void remove(int index) {
        checkRange(index);
        System.arraycopy(elementData, index + 1, elementData, index, size - index - 1);
        elementData[size - 1] = null;
        size--;

    }

    /**
     * 删除相同的元素 如果没有就不删除
     *
     * @param obj
     */
    public void remove(Object obj) {
        for (int i = 0; i < size; i++) {
            if (get(i).equals(obj)) {
                remove(i);
            }
        }

    }

    /**
     * 将指定位置的元素设置成指定的对象
     *
     * @param index 数组下标
     * @param obj   要设置的对象
     */
    public void set(int index, Object obj) {
        checkRange(index);
        elementData[index] = obj;
    }

    /**
     * 获取指定位置的元素
     *
     * @param index 数组下标
     * @return 指定位置的元素
     */
    public Object get(int index) {
        checkRange(index);
        return elementData[index];
    }

    /**
     * 确保数组的下标是否合法
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

    /**
     * 确保数组长度是否足够，不够则扩展数组
     */
    private void ensureCapacity() {
        if (size > elementData.length - 1) {
            Object[] newArray = new Object[size * 2 + 1];
            System.arraycopy(elementData, 0, newArray, 0, elementData.length);
            elementData = newArray;
        }
    }

    /**
     * 打印整个list
     */
    public void print() {
        for (int i = 0; i < size; i++) {
            System.out.println(get(i));
        }
    }

    /**
     * 获取当前的数组实际大小
     *
     * @return 返回数组实际大小
     */
    public int getSize() {
        return this.size;
    }

    /**
     * 返回数组是否是空
     *
     * @return 数组长度为0返回true
     */
    public boolean isEmpty() {
        return size == 0 ? true : false;
    }

}

```
第一部分是两个构造器+增删改查部分<br>
第二部分是一些上述方法用到的方法封装<br>
------------
`public myArrayList()`
> 无参构造器

<br>

`public myArrayList(int initialCapacity)`
> 带参构造器，其中会对`initialCapacity`进行判断，如果通过就生成数组

<br>
`public void add(Object obj)`
> 将obj加入到容器中，需要确保容量足够放得下新的这个对象，然后直接加入即可。由于是add，所以最后把size+1

<br>
`public void add(int index,Object obj)`
> 重载add，在指定位置放入对象。仍然是需要能够确保放得下元素，然后进行下标检查。最后调用系统的数组拷贝函数。由于是add，所以最后把size+1

<br>
`public void remove(int index)`
> 删除指定位置的对象，对index进行检查，删除其实就是把后面的元素覆盖到前面即可。记得处理完把数组最后一位置为Null，方便垃圾回收。由于是remove，所以最后把size-1

<br>
`public void remove(Object obj)`
> 删除容器中的对象，只删除第一个匹配的。调用的是equal方法，所以如果内容相同就认为是相同的对象。

<br>
`public void set(int index,Object obj)`
> 设置容器指定位置的对象。没什么好说的，确认index合法，然后把数组对应的位置设置成obj即可。

<br>
`public Object get(int index)`
> 获取指定位置的对象。确认index合法，返回数组对应下标的元素即可。

`private void checkRange(int index)`
> 对传入的index进行合法性检查。小于0肯定是非法的，大于size也肯定是非法的。等于的话因为size是从1开始计算的，所以`size-1==index`的时候其实是数组最后一位。

<br>
` private void ensureCapacity()`
> 确保数组能够容纳得下，在add时候肯定需要。主要思路是当`size == elementData.length`的时候需要扩容。

<br>
`public void print()`
> 打印数组(容器)中的每个元素

<br>
`public int getSize()`
`public boolean isEmpty()`
> 这俩不解释。
