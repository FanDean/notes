《疯狂Java讲义》学习笔记
=====================================

## 第8章：Java集合


### 8.1 Java 集合概述

  Java提供了集合类，所有的集合类都位于`java.util`包下。Java集合类主要由两个接口派生而出：Collection 和 
Map，他们是Java集合框架的根接口，这两个接口又包含了子接口或实现类。

Set和List接口是Collection接口派生的两个子接口，分别代表了无序集合和有序集合；  
Queue是Java提供的队列实现。

List集合中的元素，可以直接根据元素的索引来访问；     
Set集合中的元素，只能根据元素本身来访问（这也是Set集合里的元素不允许重复的原因）。  

Collection接口定义了如下操作集合元素的方法：   

| 方法		 |				描述      |
|------------|------------------------|
|boolean add(Object o) | 向集合添加元素,成功返回true|
|
