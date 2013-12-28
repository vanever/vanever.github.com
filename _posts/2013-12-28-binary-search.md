---
layout: post
title: interested binary search - ��Ȥ�Ķ�������
---

��һ�μ��������������úܼ򵥣�����д��һ��������ͱ������ĶԱ����������ֹ�Ȼд����
�����ܽ��¶���������
����Ĵ��������µ����ͣ�����������ķ�Χ�ڽ��ж����+1����-1�����³�������ѭ��:
## ����ʾ��
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

������İ汾������ʾ��
## �����汾
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

���������汾ѭ���ڵķ�֧��Ŀ3����Ȼ���Ż��İ汾�¿��԰�ѭ���ڵ�`�жϷ�֧���ٵ�����`����һ���ڱ���������������ż������Ǿ��úܾ��޵ģ���Ȼ����û����.
���ŵİ汾�����Դ������Ч����һ��������£����һ�������ҿ�£
����£����ָ���������������ж����ֵ���ڴ���ֵ��ȡ����ߵ��Ǹ������ҿ�£��Ȼ��
������£Ϊ������ѭ������������Ϊ`a[l] < key <= a[r]`������д��ֻ�������жϷ�֧�İ汾�������ѭ������ʽ��Ч�����Ǽ�ʹ��ĳ��`r`ʹ��`a[r] == key`��ѭ����������ֹ��ֱ��`r`��Ϊ��С������`a[r] == key`���Ǹ�ֵ��Ҳ�ʹﵽ��ȡ����ߵ��Ǹ�����������ֵ����Ϊ���ѭ������ֹ���������`l == r`.
ͬ�����ҿ�£�汾��ѭ������ʽΪ`a[l] <= key < a[r]`. ע������汾����ѭ������ֹ����Ϊ`l == r - 1`����������`l == r`��ԭ����������`m = (l + r)/2 and l == r - 1`��ʱ��`m`��ֵ��ԶΪ`l`������ʱ���������ѭ����Ҳ���������ĳ����Ĵ���汾���ŵĴ���
## ���Ű汾
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
