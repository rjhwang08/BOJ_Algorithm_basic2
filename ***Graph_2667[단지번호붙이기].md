[2667](https://www.acmicpc.net/problem/2667)

1. Java(BFS 이용)
```java
import java.io.*;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

class Pair{
	int row, column;
	Pair(int row, int column){
		this.row = row;
		this.column = column;
	}
}

public class Main {
	public static final int[] dx = {0, 0, 1, -1};
	public static final int[] dy = {1, -1, 0, 0};
	public static void bfs(int[][] a, int[][] g, int x, int y, int cnt, int N) {
		Queue<Pair> q = new LinkedList<Pair>();
		q.add(new Pair(x, y));
		g[x][y] = cnt;
		while(!q.isEmpty()) {
			Pair p = q.remove();
			x = p.row;
			y = p.column;
			for(int k=0; k < 4; k++) { // 상 하 좌 우, 4방향에 대해서 각각 group 추가
				int nx = x + dx[k];
				int ny = y + dy[k];
				
				if(nx >= 0 && nx < N && ny >= 0 && ny < N){  // 새로운 점이 NxN 범위에 들어오는지 확인
					if(a[nx][ny] == 1 && g[nx][ny] == 0) { // 집이 있는 곳이고, 아직 단지에 속하지 않았다면
						g[nx][ny] = cnt;
						q.add(new Pair(nx, ny));
					}
				}
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		int N = Integer.parseInt(br.readLine());
		int[][] arr = new int[N][N];
		
		for(int i=0; i < N; i++) {
			String input = br.readLine();
			for(int j=0; j < N; j++) {
				arr[i][j] = input.charAt(j) - '0'; // + '0'하니까 인덱스 문제 생김;;???
			}
		}
		
		int cnt = 0;
		int[][] group = new int[N][N];
		for(int i=0; i < N; i++) {
			for(int j=0; j < N; j++) {
				if(arr[i][j] == 1 && group[i][j] == 0){ // 집이 있는 곳이고, 아직 단지에 속하지 않았다면
					bfs(arr, group, i, j, ++cnt, N);
				}
			}
		}
		
		int[] ans = new int[cnt];
		for(int i=0; i < N; i++) {
			for(int j=0; j < N; j++) {
				if(arr[i][j] != 0) {
					ans[group[i][j] - 1]++;
				}
			}
		}
		Arrays.sort(ans);
		sb.append(cnt + "\n");
		for(int i=0; i < cnt; i++)
			sb.append(ans[i] + "\n");
		System.out.print(sb);
		br.close();
	}
}
```

2. Java(DFS 이용)
```java

```
