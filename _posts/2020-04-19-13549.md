---
title: 백준 13549 숨바꼭질 3 [Gold V]
author: Koreandns
date: 2020-04-19 16:44:00 +0800
categories: [Blogging, Tutorial]
tags: [writing]
---




링크 정보 : [https://www.acmicpc.net/problem/13549](https://www.acmicpc.net/problem/13549)



```c++
// 이 문제는 단순한 BFS를 요구하는 문제가 아니다.
// 왜냐하면, BFS를 하기 위해서는 모든 간선의 가중치가 동일해야 한다는
// 전제 조건이 필요하기 때문이다.

// 순간이동을 하면 거리가 늘지 않는다.
// BFS 특성상 거리가 가까운 곳을 먼저 방문하기 때문에 BFS인데
// 순간이동을 하면 거리가 늘지 않는데도 불구하고, 큐의 맨 뒤에 넣어서
// 반대로 거리가 가장 먼곳이 먼저 나오는 상황이 발생한다.
// 그래서 다익스트라 알고리즘을 이용하거나 쪼금의 꼼수를 부려야 한다.

#include <bits/stdc++.h>
using namespace std;

const int MAX = 1e5 + 1;

int vis[MAX];

int main(void)
{
	ios_base::sync_with_stdio(0);
	cin.tie(0);

	int n, k;
	cin >> n >> k;

	if (n == k)
	{
		cout << 0;
		return 0;
	}

	deque<int> q;
	q.push_back(n);
	fill(vis, vis + MAX, -1);

	int dx[3] = { 2, -1, 1 };
	while (false == q.empty())
	{
		int cur = q.front();
		q.pop_front();

		for (int dir=0; dir<3; ++dir)
		{
			int nx = 0 == dir ? cur * dx[dir] : cur + dx[dir];
			if (nx < 0 or MAX <= nx or -1 != vis[nx])
			{
				continue;
			}

			if (-1 == vis[cur])
			{
				++vis[cur];
			}

			if (nx == k)
			{
				cout << (0 == dir ? vis[cur] : vis[cur] + 1);
				return 0;
			}

			vis[nx] = 0 == dir ? vis[cur] : vis[cur] + 1;
			0 == dir ? q.push_front(nx) : q.push_back(nx);
		}
	}

	return 0;
}
```