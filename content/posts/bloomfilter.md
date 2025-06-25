---
title: An introduction to bloom filter
tag: data structure
date: 2019-05-09 09:04:01
---

## Introducation of bloomfilter
详细的介绍和公式推倒可以参考 [wikipedia](https://en.wikipedia.org/wiki/Bloom_filter)
[中文介绍](https://blog.csdn.net/jiaomeng/article/details/1495500)

BloomFilter 是 一个时间和空间都非常高效的存储结构，它的基本用途是检查任意一个 key（字符串）是不是在一个给定的集合（bit 数组）里。基本原理是对于给定的 key 计算一系列 hash 的值，将每一个 hash 的值作为索引到 bit 数组里去寻找相应的 bit 是不是为 1。只有当一个 key 对于所有的 hash 值检索的结果都 是 1，这个 key 才被认为在 BloomFilter 集合里面；反之，只要有一个 hash 值检索的结果为 0，则表明该 key 一定不在集合中。

BloomFilter 有以下特性：
1. 只能查询给定的 key 是否在给定的集合中， 或者在该集合中有多少个 key。但不能列举出这些 key。
2. 只 能被用于检索某个 key 是否在给定的集合中，而不能判断这个 key 在集合中出现几次，也不能象 hash 表一样给每个 key 关联一个 value。一些扩展算 法，例如 Bloomier Filter、Counting Bloom Filter等，可以用时间和空间代价换取这些特性。
3. 无法删除。一旦一个 key 被加入 Bloom Filter 后，就无法将它从中删除。Counting Bloom Filter (其基本的思路是存储bit的地方不再仅存储0、1，而是存储一个counter，当删除时只需要把counter减1就可以) 可以以时间和空间代价实现删除操作。
4. 存在一定的可能性，BloomFilter 对于不存在于集合中的 key 可能返回不正确的结果，即报告其存在于集合中。这个被称之为 False Positive。但是，BloomFilter 报告为不存在于集合中的 key 则一定不存在。即它的 False Negative 为 0。

由于以上特性的存在：需要计算其错误率(false positive rate)：

设存在`S={x1, x2,…,xn}` 这样具有`n` 个元素的集合，Bloom Filter 使用 `k` 个相互独立的哈希函数（Hash Function），它们分别将集合中的每个元素映射到 `{1,…,m}` 的范围中。
假设各个哈希函数是完全随机的。当集合 `S={x1, x2,…,xn}` 的所有元素都被 `k` 个哈希函数映射到 `m` 位的位数组中时，这个位数组中某一位还是 `0`的概率是：

$$ (1-\frac{1}{m})^{kn} $$

根据自然常数$e$的定义， $\lim\limits_{x\to\infty} (1-\frac{1}{x})^{-x}=e$，则有

$$ f_p=(1-(1-\frac{1}{m})^{kn})^k = (1-e^{-kn/m})^k $$

参数计算：

当哈希函数的个数多，那么在对一个不属于集合的元素进行查询时得到 0 的概率就大；但另一方面，如果哈希函数的个数少，那么位数组中的 0 就多。所以需要计算一个最优的哈希函数个数。
首先通过求导等可以计算出使得 $f_p$ 最小的$k$为

$$ k = \frac{m}{n} \ln{2} $$

然后将 $k$代入可以得到结果：

$$ m = -\frac{n \ln{f_p}}{(\ln{2})^2} $$


## hash
hash32是根据 [http://burtleburtle.net/bob/c/lookup3.c](http://burtleburtle.net/bob/c/lookup3.c) 修改得到。

这里有一个[不同hash对比的文章](http://www.burtleburtle.net/bob/hash/doobs.html)。
[hash compare](http://blog.sina.com.cn/s/blog_b36b1ed90102v8ti.html)
[hash method](http://www.cnblogs.com/zhoug2020/p/6984177.html)


> [呼呼呼山](http://code4fun.me)
> 2019-05-09 09:04:01
