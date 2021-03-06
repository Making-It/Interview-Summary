STL

# 内存

## 1.STL内存池的实现

STL内存分配分为一级分配器和二级分配器，一级分配器就是采用malloc分配内存，二级分配器采用内存池。

二级分配器设计的非常巧妙，分别给8k，16k,…, 128k等比较小的内存片都维持一个空闲链表，每个链表的头节点由一个数组来维护。需要分配内存时从合适大小的链表中取一块下来。假设需要分配一块10K的内存，那么就找到最小的大于等于10k的块，也就是16K，从16K的空闲链表里取出一个用于分配。释放该块内存时，将内存节点归还给链表。

如果要分配的内存大于128K则直接调用一级分配器。 

为了节省维持链表的开销，采用了一个union结构体，分配器使用union里的next指针来指向下一个节点，而用户则使用union的空指针来表示该节点的地址。

# 容器

## 1.STL里set和map是基于什么实现的

> 1).STL里set和map是基于什么实现的

- set和map都是基于红黑树实现的。

> 2).红黑树的特点？

- 红黑树是一种平衡二叉查找树，与AVL树（自平衡二叉查找树）的区别是：AVL树是完全平衡的，红黑树基本上是平衡的，它不是严格控制左、右子树高度或节点数之差小于等于1。

> 3).为什么选用红黑数？

- 因为红黑数是平衡二叉树，其查找、插入、删除的效率都是O(logn)。与AVL相比红黑数插入和删除最多只需要3次旋转，而AVL树为了维持其完全平衡性，在坏的情况下要旋转的次数太多。

> 4).红黑树的定义

- 二叉平衡搜索树


- 节点是红色或者是黑色
- 根节点是黑色
- 每个叶节点（NIL或空节点）是黑色
- 每个红色节点的两个子节点都是黑色的（也就是说不存在两个连续的红色节点）
- 从根节点到每个叶子节点路径上黑色节点的数量相同

> 5).时间复杂度

- 查找、插入、删除的效率都是O(logn)


> 6).set和map的区别

- 对于set来说key和value合一，value就是key，map的元素是一个pair，包括key和value
- set不支持[]，map(不包括multimap)支持[]

> 7).set(map)和multiset(multimap)的区别

- set不允许key重复,其insert操作调用rb_tree的insert_unique函数
- multiset允许key重复,其insert操作调用rb_tree的insert_equal函数

> 8).set(multiset)和map(multimap)的迭代器

- 由于set(multiset)key和value合一，迭代器不允许修改key
- map(multimap)除了key有data，迭代器允许修改data不允许修改key

> 9).map与unordered_map的区别？

[unordered_map 与 map 的对比](https://www.cnblogs.com/NeilZhang/p/5724996.html)

unordered_map和map类似，都是存储的key-value的值，可以通过key快速索引到value。不同的是unordered_map不会根据key的大小进行排序，

存储时是根据key的hash值判断元素是否相同，即unordered_map内部元素是无序的，而map中的元素是按照二叉搜索树存储，进行中序遍历会得到有序遍历。

所以使用时map的key需要定义operator<。而unordered_map需要定义hash_value函数并且重载operator==。但是很多系统内置的数据类型都自带这些，

那么如果是自定义类型，那么就需要自己重载operator<或者hash_value()了

## 2.vector底层的实现？insert具体做了哪些事？resize()调用的是什么？



