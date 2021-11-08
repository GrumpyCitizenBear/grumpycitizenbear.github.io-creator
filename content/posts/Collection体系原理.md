---
title: "Collection体系原理"
date: 2021-10-21T21:29:50+08:00
draft: true
---

# Collection 体系原理

## 一、体系简介

### 1.为什么要有集合？
  - 数组不能实现动态扩容
  - 数组不便于添加、修改、删除
  - 数组可以储存重复元素


### 2.Collection接口主要有两种子类型集合：List、Set
  - Collection接口储存一组不唯一，无序的对象
  - List接口储存一组不唯一，有序（插入顺序）的对象
  - Set接口储存一组唯一，无序的对象
  - Map接口储存一组键值对象，提供key和value的映射

## 二、List

### 1.ArrayList：本质上就是一个数组

### 2.动态扩容的实现方式

创建一个更大的空间，把原先所有的元素拷贝过去

```Java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

## 三、Set

### 1.不允许有重复元素的集合

- 判断重复的方式：equals方法

### 2.HashCode
- Hash(散列函数)是什么？

Hash，一般翻译做散列、杂凑，或音译为哈希，是把任意长度的输入（又叫做预映射pre-image）通过散列算法变换成固定长度的输出，该输出就是散列值。

Hash是一个函数，该函数中的实现就是一种算法，通过一种算法得到一个哈希值

Hash表是所有Hash值组成的

- HashCode

对象在Hash表中对应的Hash桶中

- 作用

把一个东西映射成一个值，但这个值可能不唯一

单向的映射

用于快速定位一些东西

- 需要注意

同一个对象必须始终返回相同的hashcode
两个对象的equals返回true，必须返回相同的hashcode
两个对象不相等，也可能返回相同的hashcode

### 3.HashSet

- 最常用最高效的Set实现
- 使用HashSet给ArrayList去重
- HashSet是无序的，如果需要排序可以使用LinkedHashSet，和插入时的顺序相同

## 四、Map

### 1.概述

- 将键映射到值的对象
- 一个映射不能包含重复的键

### 2.HashMap
本质上，HashMap的key的set就是一个HashSet

##### （1）HashMap的扩容
- 创建一个更大的HashMap，将原先的拷贝过来

##### （2）线程不安全性
- HashMap在多线程环境下重新调整大小时会造成死循环
- 应该使用ConcurrentHashMap

##### （3）JDK7之后，HashMap由链表➡️红黑树
- 原因：有一种巧合，一大组数据的hashcode值相同时，他们将储存在同一个hash桶中
- 原本的hashset被迫变成了一个有序列表

## 五、TreeSet/TreeMap

### 1.比较
- HashSet：完全无序的
- LinkedHashSet：和插入时顺序相同
- TreeSet：有序的

### 2.原理：平衡二叉树
- 将时间复杂度从线性时间降到对数时间
- O（n）➡️ O（log n）

## 六、一些其他的方法

### 1.collections
- emptySet：等返回一个方便的空集合
- synchronizedCollection:将一个集合变成线程安全的
- unmodifiableCollection:将一个集合变成不可变的（也可以使用guava的Immutable）

### 2.Collection的其他实现
- Queue：队列 Last In Last Out
- Deque：双端队列
- LinkedList：链表
- ConcurrentHashMap:线程安全的HashMap
- PriorityQueue:优先级队列，堆的实现

### 3.Guava

- Lists/Sets/Maps
- ImmutableMap/ImmutableSet
- Multiset：可以统计重复的值
- Multimap：一个key可以对应多个值
- BiMap：创建双向的Map，从key搜索value，从value搜索key