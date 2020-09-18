[4963](https://www.acmicpc.net/problem/4963)

1. Java(BFS 이용(
```java
import java.io.*;
import java.util.*;

class Pair {
	int row, column;
	Pair(int row, int column){
		this.row = row;
		this.column = column;
	}
}

public class Main {
	public static final int[] dx = {0, 1, 1, 1, 0, -1, -1, -1};
	public static final int[] dy = {1, 1, 0, -1, -1, -1, 0, 1};
	
	public static void bfs(int[][] g, int[][] l, int w, int h, int cnt, int x, int y) {
		Queue<Pair> q = new LinkedList<Pair>();
		q.add(new Pair(x, y));
		
		while(!q.isEmpty()) {
			Pair p = q.remove();
			x = p.row;
			y = p.column;
			for(int k=0; k < 8; k++) {
				// dx, dy 배열을 통해 (x, y) 기준으로 연결된 8방향의 새로운 점들을 각각 구한다.
				int nx = x + dx[k];
				int ny = y + dy[k];
				if(nx >= 0 && nx < h && ny >= 0 && ny < w) {  // 새로운 점 (nx, ny)가 w, h의 범위에 들어가는지
					if(g[nx][ny] == 1 && l[nx][ny] == 0) {  // 그리고 land이면서 아직  카운트되지 않았는지
						q.add(new Pair(nx, ny));
						l[nx][ny] = cnt;
					}
				}
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		
		while(true) {
			String[] input = br.readLine().split(" ");
			int w = Integer.parseInt(input[0]);
			int h = Integer.parseInt(input[1]);
			if(w == 0 && h ==0) break;
			
			// 그래프 정보 입력
			int[][] graph = new int[h][w];
			for(int i=0; i < h; i++) {
				input = br.readLine().split(" ");
				for(int j=0; j < w; j++) {
					graph[i][j] = Integer.parseInt(input[j]);
				}
			}
			
			int cnt = 0;
			int[][] land = new int[h][w];
			for(int i=0; i < h; i++) {
				for(int j=0; j < w; j++) {
					if(graph[i][j] == 1 && land[i][j] == 0) {
						bfs(graph, land, w, h, ++cnt, i, j);
					}
				}
			}
			sb.append(cnt + "\n");
		}
		
		System.out.print(sb);
		br.close();
	}
}
```

2. Java(DFS 이용)
```java
import java.io.*;
import java.util.*;

class Pair {
	int row, column;
	Pair(int row, int column){
		this.row = row;
		this.column = column;
	}
}

public class Main {
	public static final int[] dx = {0, 1, 1, 1, 0, -1, -1, -1};
	public static final int[] dy = {1, 1, 0, -1, -1, -1, 0, 1};
	
	public static void dfs(int[][] g, int[][] l, int w, int h, int cnt, int x, int y) {
		l[x][y] = cnt;
		for(int k=0; k < 8; k++) {
			// dx, dy 배열을 통해 (x, y) 기준으로 연결된 8방향의 새로운 점들을 각각 구한다.
			int nx = x + dx[k];
			int ny = y + dy[k];
			if(nx >= 0 && nx < h && ny >= 0 && ny < w) {  // 새로운 점 (nx, ny)가 w, h의 범위에 들어가는지
				if(g[nx][ny] == 1 && l[nx][ny] == 0) {  // 그리고 land이면서 아직  카운트되지 않았는지
					dfs(g, l, w, h, cnt, nx, ny);
				}
			}
		}		
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		
		while(true) {
			String[] input = br.readLine().split(" ");
			int w = Integer.parseInt(input[0]);
			int h = Integer.parseInt(input[1]);
			if(w == 0 && h ==0) break;
			
			// 그래프 정보 입력
			int[][] graph = new int[h][w];
			for(int i=0; i < h; i++) {
				input = br.readLine().split(" ");
				for(int j=0; j < w; j++) {
					graph[i][j] = Integer.parseInt(input[j]);
				}
			}
			
			int cnt = 0;
			int[][] land = new int[h][w];
			for(int i=0; i < h; i++) {
				for(int j=0; j < w; j++) {
					if(graph[i][j] == 1 && land[i][j] == 0) {
						dfs(graph, land, w, h, ++cnt, i, j);
					}
				}
			}
			sb.append(cnt + "\n");
		}
		
		System.out.print(sb);
		br.close();
	}
}
```
