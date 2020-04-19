---
title: 삽입 정렬
author: Koreandns
date: 2020-04-19 14:10:00 +0800
categories: [Blogging, Tutorial]
tags: [writing]
---

# [진행 과정] 오름 차순

------



| 1. |  <span style="color:blue">31</span>  |  <span style="color:red">41</span>  |  59  |  26  |  41  |  58  |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  2.  |  <span style="color:blue">31</span>  |  <span style="color:blue">41</span>  |  <span style="color:red">59</span>  |  26  |  41  |  58  |
|  3.  |  <span style="color:blue">31</span>  |  <span style="color:blue">41</span>  |  <span style="color:blue">59</span>  |  <span style="color:red">26</span>  |  41  |  58  |
|  4.  |  <span style="color:blue">26</span>  |  <span style="color:blue">31</span>  |  <span style="color:blue">41</span>  |  <span style="color:blue">59</span>  |  <span style="color:red">41</span>  |  58  |
|  5.  |  <span style="color:blue">26</span>  |  <span style="color:blue">31</span>  |  <span style="color:blue">41</span>  |  <span style="color:blue">41</span>  |  <span style="color:blue">59</span>  |  <span style="color:red">58</span>  |
|  완료  |  26  |  31  |  41  |  41  |  58  |  59  |




`1번째 인덱스부터 시작하여 횟수가 증가할수록 인덱스 시작 위치도 + 횟수 만큼 증가한다. `

<br>



```c++
#include <bits/stdc++.h>
using namespace std;

int main(void)
{
	ios_base::sync_with_stdio(0);
	cin.tie(0);

	vector<int>arr{ 31, 41, 59, 26, 41, 58 };

	const int ARR_LENGTH = arr.size();
	for (int fixedIndex = 1; fixedIndex < ARR_LENGTH; ++fixedIndex)
	{
		int key = arr[fixedIndex];
		int variableIndex = fixedIndex - 1;

		while (0<=variableIndex and key < arr[variableIndex])
		{
			arr[variableIndex + 1] = arr[variableIndex];
			--variableIndex;
		}

		arr[variableIndex + 1] = key;

		for (auto cur : arr)
		{
			cout << cur << ' ';
		}
		cout << '\n';
	}

	return 0;
}
```