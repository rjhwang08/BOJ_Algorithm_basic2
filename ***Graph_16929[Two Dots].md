[16929](https://www.acmicpc.net/problem/16929)

1. Java(DFS)
```java
import java.io.*;
import java.util.*;

class Pair{
	int row, column;
	Pair(int row, int column){
		this.row = row;
		this.column = column;
	}
}

public class Main {
	static final int[] dx = {0, 1, 0, -1};
	static final int[] dy = {1, 0, -1, 0};
	static char[][] graph;
	static boolean[][] check;
	static int[][] dist;
	static boolean dfs(int x, int y, int N, int M, int cnt) {
		if(check[x][y]) {
			return cnt - dist[x][y] >= 4;
		}
		check[x][y] = true;
		dist[x][y] = cnt;
		for(int k=0; k < 4; k++) {
			int nx = x + dx[k];
			int ny = y + dy[k];
			if(nx >= 0 && nx < N && ny >= 0 && ny < M) {
				if(graph[x][y] == graph[nx][ny]) {
					if(dfs(nx, ny, N, M, cnt+1))
						return true;
				}
			}
		}
		return false;
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] input = br.readLine().split(" ");
		int N = Integer.parseInt(input[0]);
		int M = Integer.parseInt(input[1]);
		
		graph = new char[N][M];
		check = new boolean[N][M];
		for(int i=0; i < N; i++) {
			String line = br.readLine();
			for(int j=0; j < M; j++)
				graph[i][j] = line.charAt(j);
		}
		
		for(int i=0; i < N; i++) {
			for(int j=0; j < M; j++) {
				if(check[i][j]) continue;
				dist = new int[N][M];
				if(dfs(i, j, N, M, 0)) {
					System.out.println("Yes");
					System.exit(0);
				}
			}
		}
		System.out.println("No");
		br.close();
	}
}
```
