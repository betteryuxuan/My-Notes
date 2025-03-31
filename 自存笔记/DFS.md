```c
void dfs(int step) {
    //判断边界
    //尝试每一种可能
    for(i = 1; i <= n; i++) {
    	//继续下一步
        dfs(step + 1);
	}
    //返回
}
```
1. 先判断是否到达目标位置，如果到达目标位置，在试探有误更短路径
2. 如果没有，则找到下一步可以到达的位置，直接找到目标位置

```c
int xstart, ystart;
int p, q; // 终点坐标
int m, n;
int min = INT_MAX;
int a[100][100]; // 1空地，2障碍物
int v[100][100]; // 1已访问，0未访问
void dfs(int x, int y, int step) {
    if (x == p && y == q) {
        min = fmin(step, min);
        return;
    }
    if (a[x][y + 1] == 1 && v[x][y + 1] == 0) {
        v[x][y + 1] = 1;
        dfs(x, y + 1, step + 1);
        v[x][y + 1] = 0;
    }
    if (a[x + 1][y] == 1 && v[x + 1][y] == 0) {
        v[x + 1][y] = 1;
        dfs(x + 1, y, step + 1);
        v[x + 1][y] = 0;
    }
    if (a[x - 1][y] == 1 && v[x - 1][y] == 0) {
        v[x - 1][y] = 1;
        dfs(x - 1, y, step + 1);
        v[x - 1][y] = 0;
    }
    if (a[x][y - 1] == 1 && v[x][y - 1] == 0) {
        v[x][y - 1] = 1;
        dfs(x, y - 1, step + 1);
        v[x][y - 1] = 0;
    }
    return;
}
int main() {

    scanf("%d%d", &m, &n);
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            scanf("%d", &a[i][j]);
            v[i][j] = 0; // 初始化访问标记数组
        }
    }
    scanf("%d%d", &xstart, &ystart);
    scanf("%d%d", &p, &q);
    v[xstart][ystart] = 1;
    dfs(xstart, ystart, 0);
    printf("%d", min);
}
```

```c
int xstart, ystart;
int p, q; // 终点坐标
int m, n;
int min = INT_MAX;
int a[100][100]; // 1空地，2障碍物
int v[100][100]; // 1已访问，0未访问
int dx[4]={0, 1, 0, -1};
int dy[4]={1, 0, -1, 0};
void dfs(int x, int y, int step) {
    if (x == p && y == q) {
        min = fmin(step, min);
        return;
    }
    for (int i = 0; i < 4; i++) {
        int tx, ty;
        tx = x + dx[i];
        ty = y + dy[i];
        if (a[tx][ty] == 1 && v[tx][ty] == 0) {
            v[tx][ty] = 1;
            dfs(tx, ty, step + 1);
            v[tx][ty] = 0;
        }
    }
    return;
}
int main() {

    scanf("%d%d", &m, &n);
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            scanf("%d", &a[i][j]);
            v[i][j] = 0; // 初始化访问标记数组
        }
    }
    scanf("%d%d", &xstart, &ystart);
    scanf("%d%d", &p, &q);
    v[xstart][ystart] = 1;
    dfs(xstart, ystart, 0);
    printf("%d", min);
}
```

