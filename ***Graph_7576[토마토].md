[7576](https://www.acmicpc.net/problem/7576)

1. Java(BFS-내 풀이)
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
	public static final int[] dx = {0, 1,0, -1};
	public static final int[] dy = {1, 0, -1, 0};
	public static ArrayList<Pair> ripe;
	
	public static void bfs(int[][] graph, int[][] group, int n, int m) {
		Queue<Pair> q = new LinkedList<Pair>();
		for(Pair i : ripe) {  // 1(익은 토마토)를 모두 큐에 넣어준다.
			q.add(i);
		}		
		while(!q.isEmpty()) {
			Pair p = q.remove();
			int x = p.row;
			int y = p.column;
			
			for(int k=0; k < 4; k++) {
				int nx = x + dx[k];
				int ny = y + dy[k];
				if(nx >= 0 &&nx < n && ny >= 0 && ny < m) {
					if(graph[nx][ny] == 0 && group[nx][ny] == 0) {
						q.add(new Pair(nx, ny));
						group[nx][ny] = group[x][y] + 1;
					}
				}
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] input = br.readLine().split(" ");  // M N으로 입력됨(순서 주의)
		int N = Integer.parseInt(input[1]);
		int M = Integer.parseInt(input[0]);
		
		int[][] graph = new int[N][M];
		int[][] group = new int[N][M];
		ripe = new ArrayList<Pair>();
		
		for(int i=0; i < N; i++) {
			input = br.readLine().split(" ");
			for(int j=0; j < M; j++) {
				graph[i][j] = Integer.parseInt(input[j]);
				if(graph[i][j] == 1)
					ripe.add(new Pair(i, j));
			}
		}
		
		bfs(graph, group, N, M);
		int max = 0;
		for(int i=0; i < N; i++) {
			for(int j=0; j < M; j++) {
				if(group[i][j] > max)
					max = group[i][j];
				if(graph[i][j] == 0 && group[i][j] == 0) {
					System.out.println(-1);
					System.exit(0);
				}
			}
		}
		System.out.println(max);
		br.close();
	}
}
```

2. Java(BFS-BOJ 풀이 참고)
