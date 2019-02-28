---
layout:     post
title:      排序算法(Sort Algorithm)
subtitle:   Sort
date:       2019-02-28
author:     Galvin
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Algorithm
--- 

### recursion
* 终止条件；分解为子问题；子问题求解相同
* 警惕堆栈溢出（设置递归层数）
* 重复计算（通过散列表保存计算过的值）
* 过多的函数调用会耗时较多
* 所有的递归代码都可以改写为迭代循环的非递归写法。如何做？抽象出递推公式、初始值和边界条件，然后用迭代循环实现。

### sorting algorithm

| 排序算法 | 时间复杂度 | 是否基于比较 |
| --- | --- | --- |
| 冒泡、插入、选择 | O(n^2) | 是 |
| 快速、归并 | O(nlogn) | 是 |
| 桶、计数、基数 | O(n) | 否 |
* 稳定性：如果排序后相等的元素的相对位置没有改变，则此排序算法是稳定的（按照下单时间和金额排序）
* **Bubble Sort**
    * [BubbleSort-Golang](https://github.com/Galvin-wjw/Golang-study/blob/master/Algorithm/bubblesort.go)
    * 设置标志位flag，当某次内层循环没有交换数据，则说明已经完全有序，后面的已经不用比较了
    * 最优时间复杂度（已经是正序，O(n)）;最差时间复杂度（倒序，O(n^2)）；空间复杂度（O(1)）
* **Insertion Sort**
	* [InsertionSort-Golang](https://github.com/Galvin-wjw/Golang-study/blob/master/Algorithm/insert-sort.go)
    * 插入算法的核心思想是取未排序区间中的元素，在已排序区间中找到合适的插入位置将其插入，并保证已排序区间数据一直有序。重复这个过程，直到未排序区间中元素为空，算法结束。
    * 稳定的，最优时间复杂度（已经是正序，O(n)）;最差时间复杂度（倒序，O(n^2)）；空间复杂度（O(1)）
* **Merge Sort**
	* [MergeSort-Golang](https://github.com/Galvin-wjw/Golang-study/blob/master/Algorithm/merge-sort.go) 
    * 先分解，再合并；稳定的排序算法；非原地排序
    * 时间复杂度O(nlogn),空间复杂度O(n)
* **Quick Sort**
	* [QuickSort-Golang](https://github.com/Galvin-wjw/Golang-study/blob/master/Algorithm/quick-sort2.go)
    * 确定pivot，左边小于pivot，右边大于pivot，然后递归；不稳定的排序算法；原地排序
    * 时间复杂度平均O(nlogn),最差O(n^2)
* **Bucket Sort**
	* [BucketSort-Golang](https://github.com/Galvin-wjw/Golang-study/blob/master/Algorithm/bucket-sort.go)
    * 排序的数据M在一个可量化的区间内，等分区间到N个桶内，每个桶有M/N个数据；桶内做快速排序，时间复杂度O(M/Nlog(M/N));N个桶，O(Mlog(M/N));M接近N的时候，时间复杂度为O(M)
    * 适合数据在磁盘中的外部排序；
    * 数据要能尽可能均分，桶和桶之间要有序
 * **Counting Sort**
    * 数据量大，范围不大，设置每个桶内数据一样；高考成绩
    * 时间复杂度 O(n)
 * **Radix Sort**
    * 排序手机号码；利用稳定的排序算法，从低位排序到高位
    * 时间复杂度 O(n)
    * 基数排序对要排序的数据是有要求的，需要可以分割出独立的“位”来比较，而且位之间有递进的关系，如果 a 数据的高位比 b 数据大，那剩下的低位就不用比较了。除此之外，每一位的数据范围不能太大，要可以用线性排序算法来排序，否则，基数排序的时间复杂度就无法做到 O(n) 了
* **C qsort()**
    * qsort() 会优先使用归并排序来排序输入数据
    * 要排序的数据量比较大的时候，qsort() 会改为用快速排序算法来排序
    * O(n^2) 时间复杂度的算法并不一定比 O(nlogn) 的算法执行时间长；O(nlogn) 在没有省略低阶、系数、常数之前可能是 O(knlogn + c)，而且 k 和 c 有可能还是一个比较大的数
* **Golang sort()**
    * [Golang-sort()](https://github.com/golang/go/blob/master/src/sort/sort.go)
    
    
    
