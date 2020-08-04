## 迷宫问题

### 路径打印

#### 方法1：数组记录到达每个点的最短距离

```c++
char mat[60][60]; //0可以走
int vis[60][60]; //记录是否访问并记录顺序
int dx[] = { 1, 0, 0, -1 }, dy[] = { 0, -1, 1, 0 };
char ans[1000], dir[] = { 'U', 'R', 'L', 'D' };
int n = 30, m = 50;
bool check(int x, int y)
{
    if (x < 1 || x > n || y < 1 || y > m) {
        return false;
    }
    return true;
}
void bfs()
{
    queue<int> q;
    q.push(1);
    q.push(1);
    q.push(1);
    while (!q.empty()) {
        int x = q.front();
        q.pop();
        int y = q.front();
        q.pop();
        int len = q.front();
        q.pop();
        vis[x][y] = len;
        //回溯
        if (x == n && y == m) {
            int sum = len;
            while (!(x == 1 && y == 1)) {
                for (int i = 0; i < 4; i++) {
                    int bx = x + dx[i], by = y + dy[i];
                    if (check(bx, by) && vis[bx][by] == vis[x][y] - 1) {
                        ans[--len] = dir[i];
                        x = bx;
                        y = by;
                        break;
                    }
                }
            }
            for (int i = 1; i <= sum - 1; i++) {
                cout << ans[i];
            }
            return;
        }
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if (check(nx, ny) && !vis[nx][ny] && mat[nx][ny] == '0') {
                q.push(nx);
                q.push(ny);
                q.push(len + 1);
            }
        }
    }
}
int main()
{
    ios::sync_with_stdio(0), cin.tie(0);
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        string temp;
        cin >> temp;
        for (int j = 1; j <= m; j++) {
            mat[i][j] = temp[j - 1];
        }
    }
    bfs();
}
```

#### 方法2：结构体存储所有结果

```c++
#include <cstdio>
#include <cstring>
#include <iostream>
#include <queue>
using namespace std;
int m, n;
char map[60][60];
bool vis[60][60];
int dir[4][2] = { { 1, 0 }, { 0, -1 }, { 0, 1 }, { -1, 0 } }; //方向 下左右上
char dirc[4] = { 'D', 'L', 'R', 'U' };

struct node {
    int x;
    int y;
    int step;
    string str;
    node(int xx, int yy, int ss, string s) //构造函数
    {
        x = xx;
        y = yy;
        step = ss;
        str = s;
    }
};

bool check(int x, int y)
{
    if (x < 0 || x >= m || y < 0 || y >= n || vis[x][y] || map[x][y] == '1')
        return false;
    return true;
}

queue<node> q;
void bfs(int x, int y)
{
    q.push(node(0, 0, 0, "")); //起点入队
    vis[0][0] = true;
    while (!q.empty()) {
        node now = q.front();
        if (now.x == m - 1 && now.y == n - 1) //到达终点
        {
            cout << now.str << endl;
            cout << now.step << endl;
            break;
        }
        q.pop();
        for (int i = 0; i < 4; i++) {
            int xx = now.x + dir[i][0];
            int yy = now.y + dir[i][1];
            if (check(xx, yy)) {
                q.push(node(xx, yy, now.step + 1, now.str + dirc[i]));
                vis[xx][yy] = true;
            }
        }
    }
}

int main()
{
    cin >> m >> n;
    m = 30, n = 50;
    for (int i = 0; i < m; i++)
        scanf("%s", map[i]);
    bfs(0, 0);
    return 0;
}
```

