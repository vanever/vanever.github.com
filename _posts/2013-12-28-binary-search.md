---
layout: post
title: Interested Binary Search
---

第一次见到二分搜索觉得很简单，随手写了一个，结果和编程珠玑的对比下来，发现果然写错了
今天总结下二分搜索：
最常见的错误是如下的类型，忘记在缩半的范围内进行额外的+1或者-1，导致出现了死循环:
#### 错误示例
{% highlight cpp %}
int bsearchErrorSample1(int l, int r, int key, int * arr)
{
	while(l <= r) {
		int m = l + (r - l) / 2;
		if (arr[m] < key)
			l = m;	// should be m + 1
		else if (arr[m] > key)
			r = m;	// should be m - 1
		else
			return m;
	}
	return -1;
}
{% endhighlight %}

纠正后的版本如下所示：
#### 正常版本
{% highlight cpp %}
int bsearch(int l, int r, int key, int * arr)
{
	while(l <= r) {
		int m = l + (r - l) / 2;
		if (arr[m] < key)
			l = m + 1;
		else if (arr[m] > key)
			r = m - 1
		else
			return m;
	}
	return -1;
}
{% endhighlight %}

上面的这个版本循环内的分支数目3个，然而优化的版本下可以把循环内的`判断分支减少到两个`，第一次在编程珠玑见到这个调优技术还是觉得很惊艳的，虽然代码没几行.

调优的版本最后可以达成两种效果，一种是向左靠拢，另一种是向右靠拢
向左靠拢就是指，假如有序数组有多个数值等于待查值，取最左边的那个，向右靠拢亦然。

以向左靠拢为例，将循环不变量设置为`a[l] < key <= a[r]`，可以写出只带两个判断分支的版本，而这个循环不变式的效果就是即使有某个`r`使得`a[r] == key`，循环将不会终止，直到`r`成为最小的满足`a[r] == key`的那个值，也就达到了取最左边的那个满足条件的值，因为外层循环的终止条件不外乎`l == r`.

同理，向右靠拢版本的循环不变式为`a[l] <= key < a[r]`. 注意这个版本里，外层循环的终止条件为`l == r - 1`，而不能是`l == r`，原因在于由于`m = (l + r)/2 and l == r - 1`的时候，`m`的值永远为`l`，而此时将会出现死循环，也就是我们文初讲的错误版本所放的错误。
#### 调优版本
{% highlight cpp %}
int bsearchRightAlign(int l, int r, int key, int * arr)
{
	while(l < r - 1) {
		int m = l + (r - l) / 2;
		if (arr[m] > key)
			r = m - 1;
		else
			l = m;
	}
	if (arr[l] == key)
		return l;
	if (arr[r] == key)
		return r;
	return -1;
}

int bsearchLeftAlign(int l, int r, int key, int * arr)
{
	while(l < r) {
		int m = l + (r - l) / 2;
		if (arr[m] < key)
			l = m + 1;
		else
			r = m;
	}
	if (arr[l] == key)
		return l;
	return -1;
}
{% endhighlight %}
