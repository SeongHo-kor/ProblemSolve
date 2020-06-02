# 삼성 SW 역량 테스트 기출 문제(codeplus)

## [06]Maaaaaaaaaaaze

**코드 동작 방식**

```cpp
#include<iostream>
#include<vector>
#include<queue>
#include<algorithm>
#include<memory.h>
using namespace std;
struct info
{
	int y, x, h, cnt;
};
vector<int> check;
int check2[6];
int cube[5][5][5];
int temp[5][5][5];
int ntemp[5][5][5];
int visited[5][5][5];

int dx[6] = { -1,1,0,0,0,0 };
int dy[6] = { 0,0,-1,1,0,0 };
int dh[6] = { 0,0,0,0,-1,1 };

int answer = 987654321;

void rotation()
{
	for (int k = 0; k < 5; k++) {
		for (int i = 0; i < 5; i++) {
			for (int j = 0; j < 5; j++) {
				temp[i][j][k] = cube[i][j][check[k]];
			}
		}
	}

	for (int p = 0; p < 5; p++) {
		if (check2[p] == 1) {
			for (int i = 0; i < 5; i++) {
				for (int j = 0; j < 5; j++) {
					ntemp[i][j][p] = temp[i][j][p];
				}
			}
		}
		else if (check2[p] == 2) {
			for (int i = 0; i < 5; i++) {
				for (int j = 0; j < 5; j++) {
					ntemp[j][4-i][p] = temp[i][j][p];
				}
			}
		}
		else if (check2[p] == 3) {
			for (int i = 0; i < 5; i++) {
				for (int j = 0; j < 5; j++) {
					ntemp[4 - i][4 - j][p] = temp[i][j][p];
				}
			}
		}
		else if (check2[p] == 4) {
			for (int i = 0; i < 5; i++) {
				for (int j = 0; j < 5; j++) {
					ntemp[4-j][i][p] = temp[i][j][p];
				}
			}
		}
	}
}
int bfs()
{
	queue<info> q;
	info now;
	now.x = 0; now.y = 0; now.h = 0; now.cnt = 0;
	q.push(now);
	visited[now.x][now.y][now.h] = 1;
	while (!q.empty()) {
		info cur = q.front();
		q.pop();
		if (cur.x == 4 && cur.y == 4 && cur.h == 4) {
			return cur.cnt;
		}
		for (int i = 0; i < 6; i++) {
			info next;
			next.x = cur.x + dx[i];
			next.y = cur.y + dy[i];
			next.h = cur.h + dh[i];
			if (next.x < 0 || next.y < 0 || next.h < 0)continue;
			if (next.x >= 5 || next.y >= 5 || next.h >= 5)continue;
			if (ntemp[next.x][next.y][next.h] == 0)continue;
			if (visited[next.x][next.y][next.h] == 1)continue;
			next.cnt = cur.cnt + 1;
			visited[next.x][next.y][next.h] = 1;
			q.push(next);
		}
	}
	return -1;
}
void overlap_permu(int cnt) {
	if (cnt == 5) {
		rotation();
		
		if (ntemp[0][0][0] == 1 && ntemp[4][4][4] == 1) {
			int res = bfs();
			if (res != -1) {
				if (answer > res)answer = res;
			}
			memset(visited, 0, sizeof(visited));
		}
		return;
	}
	for (int i = 1; i <= 4; i++) {
		if (check2[cnt] > 0)continue;
		check2[cnt] = i;
		overlap_permu(cnt + 1);
		check2[cnt] = 0;
	}
}
void permu()
{
	for (int i = 0; i <= 4; i++) {
		check.push_back(i);
	}
	do {
		overlap_permu(0);
	} while (next_permutation(check.begin(), check.end()));
}

int main()
{
	//freopen("input.txt", "r", stdin);
	for (int k = 0; k < 5; k++) {
		for (int i = 0; i < 5; i++) {
			for (int j = 0; j < 5; j++) {
					cin >> cube[i][j][k];
				}
			}
		}
		
		permu();
		if (answer == 987654321)cout << -1 << '\n';
		else cout << answer << '\n';
	return 0;
}
```
**한 번에 구현하지 못했던 이유(실수)**
- 없음