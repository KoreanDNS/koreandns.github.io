---
title: 삽입 정렬
author: Koreandns
date: 2020-04-19 14:10:00 +0800
categories: [Blogging, Tutorial]
tags: [writing]
---

# [결과] 오름 차순

------



|      |  31  |  41  |  59  |  26  |  41  |  58  |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  1.  |  31  |  <span style="color:red">41</span>  |  59  |  26  |  41  |  58  |
|  2.  |  31  |  41  |  <span style="color:red">59</span>  |  26  |  41  |  58  |
|  3.  |  <span style="color:red">26</span>  |  31  |  41  |  59  |  41  |  58  |
|  4.  |  26  |  31  |  41  |  <span style="color:red">41</span>  |  59  |  58  |
|  5.  |  26  |  31  |  41  |  41  |  <span style="color:red">58</span>  |  59  |
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