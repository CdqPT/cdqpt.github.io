---
title: Algorithm算法库
author: 陈德强
date: 2019-09-28 14:49:00
categories:
- 字典
tags: CPlus
toc: true
top: true
img: /medias/paperimg/c.jpg
summary: Algorithm算法库简介。
---

algorithm 是C++标准程式库中的一个头文件，定义了C++ STL标准中的基础性的算法（均为函数模板）。在C++98中，共计有70个算法模板函数；在C++11中，增加了20个算法模板函数。其中有5个算法模板函数定义在头文件numeric中。

下文所称的“序列”（sequence），是指可以用迭代器顺序访问的容器。

```c
#include <algorithm>
```

# 一、对序列的每个元素执行函数调用
|命令|功能|
|:---:|:---:|
|for_each(inIterBegin, inIterEnd,ufunc)|用函数对象ufunc调用容器中每一项元素。
|transform (InputIterator first1, InputIterator last1, OutputIterator result, UnaryOperation op)|对容器中每一个元素，执行一元操作op，结果写入另一容器中。
|transform (InputIterator1 first1, InputIterator1 last1,InputIterator2 first2, OutputIterator result,BinaryOperation binary_op)|对两个容器中对应的每一对元素，执行二元操作binary_op，结果写入另一容器中。

# 二、测试序列的性质
|命令|功能|
|:---:|:---:|
|all_of (InputIterator first, InputIterator last, UnaryPredicate pred)|C11算法。如果序列所有元素均满足谓词pred，则返回true
|any_of (InputIterator first, InputIterator last, UnaryPredicate pred)|C11算法。如果序列存在元素满足谓词pred，则返回true
|none_of (InputIterator first, InputIterator last, UnaryPredicate pred)|C11版。如果容器中所有元素不满足为此pred，则返回true

# 三、有序容器中的边界查找
|命令|功能|
|:---:|:---:|
|equal_range(forIterBegin, forIterEnd, targetVal)|在已排序的容器中查找目标值的位置范围；返回范围的下界与上界。对于随机迭代器，用二分查找；否则线性查找。返回pair<ForwardIterator,ForwardIterator>
|equal_range(forIterBegin, forIterEnd, targetVal, binPred)|重载版本，其中binPred是给定的序关系函数。
|lower_bound(forIterBegin, forIterEnd, targetVal)|在升序的容器中查找第一个不小于targetVal的元素。实际上是二分查找。
|lower_bound(forIterBegin, forIterEnd, targetVal, binPred)|重载版本，其中binPred是给定的序关系函数。
|upper_bound(forIterBegin, forIterEnd, val)|在已排序的容器中查找目标值val出现的上界（即第一个大于目标值val的元素的位置）。
|upper_bound(forIterBegin, forIterEnd, val, binPred)|重载版本，其中binPred是给定的序关系函数。

# 四、比较
|命令|功能|
|:---:|:---:|
|equal(inIter1Begin, inIter1End, inIter2Begin)|比较两个序列的对应元素是否相等
|equal(inIter1Begin, inIter1End, inIter2Begin, binPred)|重载版本，其中binPred是给定的相等比较函数。
|lexicographical_compare(inIter1Begin, inIter1End, inIter2Begin, inIter2End)|对两个序列做词典比较。两个序列的对应元素用 < 运算符比较。如果第一个序列在词典序下小于第二个序列，返回true
|lexicographical_compare(inIter1Begin, inIter1End, inIter2Begin, inIter2End, binPred)|重载版本，其中binPred是给定的“小于”比较函数。
|mismatch (InputIterator1 first1, InputIterator1 last1,InputIterator2 first2)|比较两个序列的对应元素，返回用std::pair表示的第一处不匹配在两个序列的位置，比较时使用==运算符。
|mismatch (InputIterator1 first1, InputIterator1 last1,InputIterator2 first2, BinaryPredicate pred)|重载版本，其中binPred是给定的“相等”比较函数。

# 五、复制
|命令|功能|
|:---:|:---:|
|copy (InputIterator first, InputIterator last, OutputIterator result)|复制一个序列到另一个序列
|copy_backward (BidirectionalIterator1 first,BidirectionalIterator1 last,BidirectionalIterator2 result)|把一个序列复制到另一个序列，按照由尾到头顺序依次复制元素。
|copy_if (InputIterator first, InputIterator last,OutputIterator result, UnaryPredicate pred)|C11算法。复制容器中所有满足谓词pred的元素
|copy_n (InputIterator first, Size n, OutputIterator result)|C11算法。复制容器中前n个元素

# 六、计数
|命令|功能|
|:---:|:---:|
|count (InputIterator first, InputIterator last, const T& val)|容器中等于给定值的元素的计数
|count_if (InputIterator first, InputIterator last, UnaryPredicate pred)|容器中满足给定谓词的元素的计数

# 七、填充
|命令|功能|
|:---:|:---:|
|fill (ForwardIterator first, ForwardIterator last, const T& val)|用给定值填充容器中每个元素
|fill_n (OutputIterator first, Size n, const T& val)|用给定值填充序列的n个元素

# 八、单值过滤
|命令|功能|
|:---:|:---:|
|unique (ForwardIterator first, ForwardIterator last)|对容器中一群连续的相等的元素，仅保留第一个元素，该群其他元素被这群元素之后的其他值的元素替换。该函数不改变这些值的相互顺序，不改变容器的size。函数返回值为最后一个保留元素的下一个位置（past-the-end element）。建议函数返回后resize容器对象。
|unique (ForwardIterator first, ForwardIterator last,BinaryPredicate pred)|重载版本，用二元谓词pred替换operator==算符。
|unique_copy (InputIterator first, InputIterator last,OutputIterator result)|把一个容器中的元素拷贝到另一个序列；对于一群连续的相等的元素，仅拷贝第一个元素。
|unique_copy (InputIterator first, InputIterator last,OutputIterator result, BinaryPredicate pred)|重载版本，用二元谓词pred替换operator==算符。

# 九、生成
|命令|功能|
|:---:|:---:|
|generate (ForwardIterator first, ForwardIterator last, Generator gen)|对容器中每个元素，依次调用函数gen的返回值赋值。
|generate_n (OutputIterator first, Size n, Generator gen)|对容器中的n个元素，依次调用指定函数的返回值赋值。

# 十、堆操作

这里的堆是指b:二叉堆，默认为b:最大堆。用数组或std::vector等顺序结构按照广度优先顺序存储堆的元素。数组的第一个元素是堆的根节点元素。元素的比较使用operator< 或者comp函数（对于重载版本）。

|命令|功能|
|:---:|:---:|
|is_heap (RandomAccessIterator first, RandomAccessIterator last)|C11版，判断序列是否为二叉堆
|is_heap_until (RandomAccessIterator first, RandomAccessIterator last,Compare comp)|C11版，重载版本
|is_heap_until (RandomAccessIterator first, RandomAccessIterator last)|C11版，返回有效二叉堆的最末范围
|is_heap (RandomAccessIterator first, RandomAccessIterator last,Compare comp)|C11版，重载版本
|make_heap (RandomAccessIterator first, RandomAccessIterator last)|对于一个序列，其元素任意放置，原地(in-place)构造一颗最大树。
|make_heap (RandomAccessIterator first, RandomAccessIterator last,Compare comp )|重载版本
|pop_heap (RandomAccessIterator first, RandomAccessIterator last)|堆的根节点被移除到last-1位置，堆的元素数目减1并保持堆性质。
|pop_heap (RandomAccessIterator first, RandomAccessIterator last,Compare comp)|重载版本
|push_heap (RandomAccessIterator first, RandomAccessIterator last)|对一个范围为[first,last-1)的堆，增加一个新元素。新元素最初保存在last-1位置。
|push_heap(r,r,bpred)|重载版本
|sort_heap (RandomAccessIterator first, RandomAccessIterator last)|对一个堆，执行原地堆排序，得到一个升序结果。
|sort_heap (RandomAccessIterator first, RandomAccessIterator last,Compare comp)|重载版本

# 十一、合并
|命令|功能|
|:---:|:---:|
|inplace_merge (BidirectionalIterator first, BidirectionalIterator middle,BidirectionalIterator last)|对两个升序的序列[first,middle) 与[middle,last)，执行原地合并，合并后的序列仍保持升序。空间复杂度O(1)，时间复杂度O(N)
|inplace_merge (BidirectionalIterator first, BidirectionalIterator middle,BidirectionalIterator last, Compare comp)|重载版本,operator< 被函数对象comp代替。
|merge (InputIterator1 first1, InputIterator1 last1,InputIterator2 first2, InputIterator2 last2,OutputIterator result)|两个升序的序列合并，结果序列保持升序。
|merge (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp)|重载版本。operator< 被函数对象comp代替。

# 十二、最小最大
|命令|功能|
|:---:|:---:|
|min (const T& a, const T& b)|两个值中的最小值。
|min (const T& a, const T& b, Compare comp)|重载版本
|max (const T& a, const T& b)|两个值中的最大值。
|max (const T& a, const T& b, Compare comp)|重载版本
|min_element (ForwardIterator first, ForwardIterator last)|返回容器中最小值的迭代器
|min_element (ForwardIterator first, ForwardIterator last,Compare comp) |重载版本
|max_element (ForwardIterator first, ForwardIterator last)|返回容器中最大值的迭代器
|max_element (ForwardIterator first, ForwardIterator last,Compare comp)|重载版本
|max (initializer_list<T> il, Compare comp)|C11版本
|minmax (const T& a, const T& b)|C11版，返回由最小值与最大值构成的std:pair
|minmax (const T& a, const T& b, Compare comp)|重载版本
|minmax (initializer_list<T> il)|C11版，返回由初始化列表中最小元素与最大元素构成的std:pair
|minmax (initializer_list<T> il, Compare comp)|重载版本
|minmax_element (ForwardIterator first, ForwardIterator last)|C11版，返回由容器中最小元素与最大元素构成的std:pair
|minmax_element (ForwardIterator first, ForwardIterator last, Compare comp)|重载版本

# 十三、移动语义
|命令|功能|
|:---:|:---:|
|move (InputIterator first, InputIterator last, OutputIterator result)|C11版，把输入容器中的逐个元素移动到结果序列。需要注意的是，把一个对象移动给另一个对象的std::move定义在头文件`<utility>`中。
|move_backward (BidirectionalIterator1 first,BidirectionalIterator1 last,BidirectionalIterator2 result)|C11版，把输入容器中的逐个元素自尾到头移动到结果序列

# 十四、划分
|命令|功能|
|:---:|:---:|
|nth_element (RandomAccessIterator first, RandomAccessIterator nth,RandomAccessIterator last)|对序列重排，使得指定的位置出现的元素就是有序情况下应该在该位置出现的那个元素，且在指定位置之前的元素都小于指定位置元素，在指定位置之后的元素都大于指定位置元素。
|nth_element (RandomAccessIterator first, RandomAccessIterator nth,RandomAccessIterator last, Compare comp)|重载版本
|is_partitioned (InputIterator first, InputIterator last, UnaryPredicate pred)|C11版，判断容器中满足为此pred的元素是否都在头部
|partition (ForwardIterator first,ForwardIterator last, UnaryPredicate pred)|对序列重排，使得满足谓词pred的元素位于最前，返回值为指向不满足谓词pred的第一个元素的迭代器。
|stable_partition (BidirectionalIterator first,BidirectionalIterator last,UnaryPredicate pred)|对序列重排，使得满足谓词pred的元素在前，不满足谓词pred的元素在后，且两组元素内部的相对顺序不变。一般是用临时缓冲区实现本函数。
|partition_copy (InputIterator first, InputIterator last, OutputIterator1 result_true, OutputIterator2 result_false, UnaryPredicate pred)|C11版，输入容器中满足谓词pred的元素复制到result_true，其他元素复制到result_false
|partition_point (ForwardIterator first, ForwardIterator last, UnaryPredicate pred)|C11版，输入队列已经是partition，折半查找到分界点。

# 十五、排列
|命令|功能|
|:---:|:---:|
|is_permutation (ForwardIterator1 first1, ForwardIterator1 last1,ForwardIterator2 first2)|C11版本，判断两个序列是否为同一元素集的两个排列
|is_permutation (ForwardIterator1 first1, ForwardIterator1 last1, ForwardIterator2 first2, BinaryPredicate pred)|C11版本，重载版本
|next_permutation (BidirectionalIterator first, BidirectionalIterator last)|n个元素有n!种排列。这些排列中，规定升序序列为最小排列，降序序列为最大的排列，任意两个排列按照字典序分出大小。该函数返回当前序列作为一个排列按字典序的下一个排列。
|next_permutation (BidirectionalIterator first, BidirectionalIterator last, Compare comp)|重载版本
|prev_permutation (BidirectionalIterator first, BidirectionalIterator last )|返回当前序列作为一个排列按字典序的上一个排列
|prev_permutation (BidirectionalIterator first, BidirectionalIterator last, Compare comp)|重载版本

# 十六、随机洗牌
|命令|功能|
|:---:|:---:|
|random_shuffle (RandomAccessIterator first, RandomAccessIterator last)|n个元素有n!个排列。该函数给出随机选择的一个排列。
|random_shuffle (RandomAccessIterator first, RandomAccessIterator last, RandomNumberGenerator& gen)|使用给定的随机数发生器来产生随机选择的一个排列。
|shuffle (RandomAccessIterator first, RandomAccessIterator last, URNG&& g)|C11版，其中参数g为随机数发生器（uniform random number generator），如在<random>中定义的标准随机数发生器。

# 十七、删除
|命令|功能|
|:---:|:---:|
|remove (ForwardIterator first, ForwardIterator last, const T& val)|删除容器中等于给定值的所有元素，保留的元素存放在容器的前部且相对次序不变。容器的size不变。
|remove_if (ForwardIterator first, ForwardIterator last,UnaryPredicate pred)|删除容器中满足给定谓词pred的元素，保留的元素存放在容器的前部且相对次序不变。容器的size不变。
|remove_copy (InputIterator first, InputIterator last, OutputIterator result, const T& val)|把一个容器中不等于给定值的元素复制到另一个容器中。
|remove_copy_if (InputIterator first, InputIterator last, OutputIterator result, UnaryPredicate pred)|把一个容器中不满足给定谓词pred的元素复制到另一个容器中。

# 十八、替换
|命令|功能|
|:---:|:---:|
|replace (ForwardIterator first, ForwardIterator last, const T& old_value, const T& new_value)|把容器中为给定值的元素替换为新值
|replace_if (ForwardIterator first, ForwardIterator last, UnaryPredicate pred, const T& new_value )|把容器中满足给定谓词pred的元素替换为新值
|replace_copy (InputIterator first, InputIterator last, OutputIterator result, const T& old_value, const T& new_value)|复制序列，对于等于老值的元素复制时使用新值
|replace_copy_if (InputIterator first, InputIterator last, OutputIterator result, UnaryPredicate pred, const T& new_value)|复制序列，对于满足给定谓词pred的元素复制新值。

# 十九、逆序
|命令|功能|
|:---:|:---:|
|reverse (BidirectionalIterator first, BidirectionalIterator last)|把容器中的元素逆序
|reverse_copy (BidirectionalIterator first, BidirectionalIterator last, OutputIterator result)|复制序列的逆序

# 二十、旋转
|命令|功能|
|:---:|:---:|
|rotate (ForwardIterator first, ForwardIterator middle,ForwardIterator last)|等效于循环左移序列，使得迭代器middle所指的元素成为首元素。
|rotate_copy (ForwardIterator first, ForwardIterator middle, ForwardIterator last, OutputIterator result)|等效于循环左移序列并复制到新的存储空间，使得迭代器middle所指的元素成为首元素。

# 二十一、搜索
|命令|功能|
|:---:|:---:|
|ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last)|在容器中发现第一对相邻且值相等的元素
|ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last, BinaryPredicate pred)|重载版本。用给定谓词pred代替了operator ==
|binary_search (ForwardIterator first, ForwardIterator last,const T& val)|对一个升序序列做b:二分搜索，判定容器中是否有给定值val。
|binary_search (ForwardIterator first, ForwardIterator last,const T& val, Compare comp)|重载版本。用给定谓词pred代替了operator ==
|find (InputIterator first, InputIterator last, const T& val)|对一个输入序列，找到第一个等于给定值的元素
|ForwardIterator1 find_end (ForwardIterator1 first1, ForwardIterator1 last1, ForwardIterator2 first2, ForwardIterator2 last2)|在序列[first1,last1)中，找到序列[first2,last2)最后出现的位置
|ForwardIterator1 find_end (ForwardIterator1 first1, ForwardIterator1 last1, ForwardIterator2 first2, ForwardIterator2 last2, BinaryPredicate pred)|重载版本，用给定谓词pred代替operator==
|find_first_of (InputIterator first1, InputIterator last1, ForwardIterator first2, ForwardIterator last2)|在序列[first1, last1)中，找到集合[first2,last2)中任何一个元素的第一次出现
|find_first_of (InputIterator first1, InputIterator last1, ForwardIterator first2, ForwardIterator last2, BinaryPredicate pred)|重载版本，用给定谓词pred代替operator==
|find_if (InputIterator first, InputIterator last, UnaryPredicate pred)|在容器中返回满足谓词pred的第一个元素
|find_if_not (InputIterator first, InputIterator last, UnaryPredicate pred)|C11算法，在容器中返回不满足谓词pred的第一个元素
|search (ForwardIterator1 first1, ForwardIterator1 last1, ForwardIterator2 first2, ForwardIterator2 last2)|在序列[first1,last1)中，找到序列[first2,last2)首次出现的位置
|search (ForwardIterator1 first1, ForwardIterator1 last1, ForwardIterator2 first2, ForwardIterator2 last2, BinaryPredicate pred)|重载版本，用给定谓词pred代替operator==
|search_n (ForwardIterator first, ForwardIterator last, Size count, const T& val)|给定容器中，搜索给定值val连续出现n次的位置
|search_n ( ForwardIterator first, ForwardIterator last, Size count, const T& val, BinaryPredicate pred )|重载版本，用给定谓词pred代替operator==

# 二十二、集合操作
|命令|功能|
|:---:|:---:|
|includes ( InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2 )|判断第2个已为升序的序列的元素是否都出现在第1个升序容器中
|includes ( InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, Compare comp )|重载版本，用给定谓词pred代替operator<
|set_difference (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result)|两个升序序列之差
|set_difference (InputIterator1 first1, InputIterator1 last1,InputIterator2 first2, InputIterator2 last2,OutputIterator result, Compare comp)|重载版本
|set_intersection (InputIterator1 first1, InputIterator1 last1,InputIterator2 first2, InputIterator2 last2,OutputIterator result)|两个升序序列（集合）的交
|set_intersection (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp)|重载版本
|set_symmetric_difference (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result)|两个升序序列的b:对称差
|set_symmetric_difference (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp)|重载版本
|set_union (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result)|两个升序序列的并
|set_union (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp)|重载版本

# 二十三、排序
|命令|功能|
|:---:|:---:|
|partial_sort (RandomAccessIterator first, RandomAccessIterator middle,RandomAccessIterator last)|使得[first, middle)为整个容器中最小的那些元素并为升序。[middle,last)的元素任意安排。
|partial_sort (RandomAccessIterator first, RandomAccessIterator middle,RandomAccessIterator last, Compare comp)|重载版本
|partial_sort_copy (InputIterator first,InputIterator last, RandomAccessIterator result_first, RandomAccessIterator result_last)|从输入序列复制最小的一批元素到结果容器中，并按升序排列
|partial_sort_copy (InputIterator first,InputIterator last, RandomAccessIterator result_first, RandomAccessIterator result_last, Compare comp)|重载版本
|is_sorted (ForwardIterator first, ForwardIterator last)|C11版本，判断序列是否为升序
|is_sorted (ForwardIterator first, ForwardIterator last, Compare comp)|C11版本，重载版本
|is_sorted_until (ForwardIterator first, ForwardIterator last)|C11版本，返回序列从头部开始为升序的子序列的结束处
|is_sorted_until (ForwardIterator first, ForwardIterator last, Compare comp)|C11版本，重载版本
|sort (RandomAccessIterator first, RandomAccessIterator last)|排序
|sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp)|重载版本
|stable_sort ( RandomAccessIterator first, RandomAccessIterator last )|稳定排序
|stable_sort ( RandomAccessIterator first, RandomAccessIterator last,Compare comp )|重载版本

# 二十四、交换
|命令|功能|
|:---:|:---:|
|iter_swap (ForwardIterator1 a, ForwardIterator2 b)|交换两个迭代器所指的元素对象
|swap (T& a, T& b)|交换两个对象。优先使用移动语义
|swap(T (&a)[N], T (&b)[N])|交换两个对象数组
|swap_ranges (ForwardIterator1 first1, ForwardIterator1 last1,ForwardIterator2 first2)|交换两个容器中对应元素



参考链接：
https://zh.wikibooks.org/wiki/C%2B%2B/STL/Algorithm