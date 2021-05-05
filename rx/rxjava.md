<!-- TOC -->

- [RxJava 基本梳理](#rxjava-基本梳理)
- [学习](#学习)
- [学习的细节](#学习的细节)
  - [操作符](#操作符)
    - [快速创建操作符](#快速创建操作符)
      - [（1）create](#1create)
      - [（2）empty()](#2empty)
      - [（3）never()](#3never)
      - [（4）error()](#4error)
      - [（5）just()](#5just)
      - [（6）fromArray(T... items)](#6fromarrayt-items)
      - [（7）fromIterable()](#7fromiterable)
    - [延迟创建操作符](#延迟创建操作符)
      - [（1）defer](#1defer)
      - [（2）timer](#2timer)
      - [（3）interval](#3interval)
      - [（4）intervalRange](#4intervalrange)
      - [（5）range](#5range)
    - [变换操作符](#变换操作符)
      - [（1）map](#1map)
      - [（2）flatmap](#2flatmap)
      - [（3）concatmap](#3concatmap)
      - [（4）buffer](#4buffer)
    - [组合操作符](#组合操作符)
      - [（1）concat()和concatarray()](#1concat和concatarray)
      - [（2）merge与mergeArray()](#2merge与mergearray)

<!-- /TOC -->

# RxJava 基本梳理

# 学习

为什么会有这么一个东西，由浅到深来解决这种问题。
所有的东西都有其对应的背景以及要解决的问题。先整清楚这个，再谈论后续的操作。
之后在学习的过程中，前期不要去过分的纠结技术实现原理。就如下这几条：


1. 先会用。用的时候，看看是不是满足了我的所有需求。
2. 熟练使用。
3. 深究原理。
4. 与竞品进行对比。

一个用户如何快速的上手使用？


# 学习的细节

## 操作符

### 快速创建操作符
#### （1）create 
创建一个被观察者对象

#### （2）empty()
快速创建一个只发送onComplete的被观察者对象

#### （3）never()
快速创建一个什么都不发送的被观察者对象 

#### （4）error()
快速创建一个只发送error的被观察者对象

#### （5）just()
快速创建被观察者对象，最多只能发送十个事件。因为api中提供的just重载方法最多只支持十个参数，而且必须是同类型的。

其实内部也是调用了fromArray()

#### （6）fromArray(T... items)
快速创建被观察者对象，可以传入任意类型、任意数量的数据。
形参十个可变参数。

#### （7）fromIterable()
快速创建被观察者对象，可发送多个任意子类型的数据。

### 延迟创建操作符

#### （1）defer
直到有Observer观察者订阅的时候，才会通过Observable的工厂方法动态创建Observable，并且发送事件。

#### （2）timer
快速传建一个Observable的被观察者，延迟指定时间之后，发送一个类型为Long的事件。

#### （3）interval
快速创建Observable对象，每隔指定的时间，发送相应的事件，事件序列从0开始，无限增1。

#### （4）intervalRange
类似于interval，快速创建一个Observable对象，指定时间间隔发送事件，可以执行发送事件的数量，数据一次递增1。

#### （5）range
类似于intervalRange()，快速创建一个被观察者对象，指定事件起始值，执行发送事件的数量，但是区别在于range()不能延迟发送的时间。

### 变换操作符
变换操作符的主要是对事件序列中的事件进行处理变换，使其转变成不同的事件，再加以处理。这里列举几种常用的操作符

#### （1）map
map操作符把被观察者产生的结果通过一系列的映射规则转换成另一种结果集，并交给订阅者处理。简单来说就是对被观察者发送的每一个事件都通过指定函数处理，从而转换成另外一种事件。

#### （2）flatmap
flatmap操作符是将Observable产生的结果拆分和单独转化变成多个Observable，然后把多个Observable的结果扁平整合成一个Observable，并依次提交产生的结果给订阅者Observer。

flatmap在合并observable的时候，可能存在交叉的情况，即新合并的序列是无序的。


#### （3）concatmap
与flatmap类似，但是concatmap能保证书序。

#### （4）buffer
buffer()操作符将Observable(被观察者)需要发送的事件周期性收集到列表中，并把这列表提交给Observer(观察者)，观察者处理后，清空buffer列表，同时接收下一次的结果交给订阅者，周而复始。

### 组合操作符

#### （1）concat()和concatarray()
concat()与concatArray()都是组合多个被观察者的一起发送数据，合并后按照先后顺序执行。

区别是：concat()只能是4个参数，但是concatArray()可以是多个参数。concat内部调用的就是concatArray()。

#### （2）merge与mergeArray()
