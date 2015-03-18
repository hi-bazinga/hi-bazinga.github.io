---
title: 用贪心算法解决Scheduling问题
date: 2013-09-16 22:16:11
categories: Algorithms
tags: 
    - Algorithms
---

>There is no silver bullet in algorithm design.

没有一种算法能普适于所有的计算问题。常见的算法有：

+ **divide and comquer**（归并排序等）
+ **randomized algorithms**（基于随机pivot的快速排序、Randomized Selection算法、Hash函数的设计等）
+ **greedy algorithms**（Dijkstra算法等）
+ **dynamic programming**（Sequence Alignment问题、背包问题、Floyd-Warshall、最长递增子序列LIS等）

## 贪心算法

什么是贪心算法？简单地说，就是在算法执行的每一步都“目光短浅”地选择当下最好的方案，而不考虑整体效果的优劣。
贪心算法与分治算法相比有以下一些特点：

<!--more-->

+ 算法易于构造和实现
+ 易于计算时间复杂度（不同于分治法的Master Method）
+ 难以证明其正确性，大多数情况下贪心算法并不正确（分治法可以用归纳法证明）

证明贪心算法的正确性，一般有两种方式：

1. **greedy stays ahead**（类似归纳法，即证明贪心算法的每一步完成后当前解决方案是最佳的）
2. **exchange argument**（下面证明Scheduling问题的最优解将会用到这种方法）

## Scheduling问题
	
假设一台单核计算机要完成若干个任务，每个任务有两个属性，一个是完成任务所需要的时间t，另一个是任务的权重w（优先级），时间与权重都用正整数定义。现在要求设计计算一种完成任务的先后顺序，使得所有任务的带权完成时间之和最小。`注：任务的带权完成时间 = 任务权重w * 任务完成时的时间（包含之前的等待时间）`

	举个例子：
		      权重   任务时间
	 T1	   2	    5
	 T2	   1	    3

	两种顺序方案的加权完成时间之和：
	① T1,T2： 5*2+(5+3)*1=18
	② T2,T1： 3*1+(3+5)*2=19
	所以选择方案①

### **问题分析**

如果所有任务只有一个时间属性，没有权重的区分，那么显而易见，按任务时间t从小到大排序可以使***完成时间***之和最小。然后，现在要考虑带权重的情况，不能再简单地按优先级或任务时间进行排序，而要寻找一种兼顾权重和任务时间的排序条件。

我们现在来考虑这两种排序条件：

1. w - t (权重与任务时间的差值)
2. w / t (权重与任务时间的比值)

### **使用w/t排序的正确性证明**

设任务序列 `Seq=T1,T2,T3...Tn` 按照w/t的比值大小从高到低排序，此时加权完成时间之和为C-sum。
假设存在某种与序列Seq不同的排序方式 Seq\* ，使得C-sum\* < C-sum。
因为`W1/t1 > w2/t2 > w3/t3 ... > wn/tn`,所以 Seq\* 中必然存在某两个相邻的任务Ti、Tj使得**wi / ti < wj / tj**：
`T1,T2,T3...Ti-1,Ti,Tj,Tj+1...Tn-1,Tn`
此时我们运用**exchange**思想，将Ti与Tj交换位置，**可以发现`T1...Ti-1`以及`Tj+1...Tn`的加权完成时间之和不变**，所以，我们只要考察Ti和Tj这两个任务加权完成时间在交换前后的变化情况即可。`Ti,Tj`变成`Tj,Ti`后，Ti的加权完成时间增加了tj\*wi，而Tj的加权完成时间减少了ti\*wj,所以总的加权完成时间C-sum\* = C-sum + tj \* wi - ti \* wj。又因为wi / ti < wj / tj,即tj \* wi < ti \* wj，即C-sum\* > C-sum ，与假设矛盾！

## **练习**
[Algorithms: Design and Analysis, Part 2](https://class.coursera.org/algo2-002/lecture/index) 这门课给了一个小练习：

In this programming problem and the next you'll code up the greedy algorithms from lecture for minimizing the weighted sum of completion times.. Download the text file [here](http://spark-public.s3.amazonaws.com/algo2/datasets/jobs.txt). This file describes a set of jobs with positive and integral weights and lengths. It has the format
[number_of_jobs]
[job_1_weight] [job_1_length]
[job_2_weight] [job_2_length]
...
For example, the third line of the file is "74 59", indicating that the second job has weight 74 and length 59. You should NOT assume that edge weights or lengths are distinct.

Q1. Your task in this problem is to run the greedy algorithm that schedules jobs in decreasing order of the difference (weight - length). Recall from lecture that this algorithm is not always optimal. IMPORTANT: if two jobs have equal difference (weight - length), you should schedule the job with higher weight first. Beware: if you break ties in a different way, you are likely to get the wrong answer. You should report the sum of weighted completion times of the resulting schedule --- a positive integer --- in the box below.

Q2. For this problem, use the same data set as in the previous problem. Your task now is to run the greedy algorithm that schedules jobs (optimally) in decreasing order of the ratio (weight/length). In this algorithm, it does not matter how you break ties. You should report the sum of weighted completion times of the resulting schedule --- a positive integer --- in the box below.

Python实现：

``` lang:python Scheduling.py

    def diff_compare(j1,j2):
        return (j1[0] - j1[1]) > (j2[0] - j2[1]) or \
            ((j1[0] - j1[1]) == (j2[0] - j2[1]) and j1[0] > j2[0])

    def ratio_compare(j1,j2):
        return j1[0] * j2[1] > j1[1] * j2[0]

    def quickSort(jobs):
        if len(jobs) <= 1:
            return jobs
        i = 1
        for j in range(1,len(jobs)):
            if diff_compare(jobs[j],jobs[0]):   #use ratio_compare() to solve Q2
                jobs[j],jobs[i] = jobs[i],jobs[j]
                i += 1               
        jobs[i-1],jobs[0] = jobs[0],jobs[i-1]
        return quickSort(jobs[0:i-1]) + [jobs[i-1]] + quickSort(jobs[i:])

    f = open('jobs.txt')
    num_of_jobs = int(f.readline())

    jobs = []
    for line in f:
        jobs.append(map(int,line.split(' ')))

    jobs = quickSort(jobs)

    sum_time = 0L
    time = 0L
    for i in range(0,num_of_jobs):
        time += jobs[i][1]
        sum_time += jobs[i][0] * time

    print sum_time
    
```
