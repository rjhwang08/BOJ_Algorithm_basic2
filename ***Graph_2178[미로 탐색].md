[2178](https://www.acmicpc.net/problem/2178)

[필독 FAQ!!](https://www.acmicpc.net/board/view/25832)

1. Java(DFS-실패케이스)
- DFS를 이용해서 풀려고 했으나, 이 문제는 DFS로는 풀 수가 없음
- 일단 BFS던 DFS던 탐색의 본질은 모든 정점을 한번씩만 방문하는 것임
- 근데 여기서 DFS를 통해 가장 끝점에 도착하면 그게 최소 이동이지 않을 수 있음
- 그렇다고 아래 내 코드처럼 구현하면 DFS가 아니라 BF가 되어버림
- 왜냐면 check를 하지 않음으로써 같은 정점에 여러번 가면서 모든 경우의 수를 구하고, 그 중에서 최소를 구하기 때문임

```java
import java.io.*;
import java.util.*;

public class Main {
	public static final int[] dx = {0, 1,0, -1};
	public static final int[] dy = {1, 0, -1, 0};
	public static int[][] graph;
	public static boolean[][] check;
	public static ArrayList<Integer> ans = new ArrayList<Integer>();
	
	public static void dfs(int n, int m, int cnt, int x, int y) {
		if(x == n-1 && y == m-1) {
			ans.add(cnt);
			return;
		}
    
    // DFS가 되려면 여기에 check[x][y] = true 일 때 return 코드가 있어야함
    // 현재 내 코드는 BF임
    //그렇다고 넣으면 위에 말한대로 DFS가 되어버려서 최소 이동을 구할 수 없음
    
		check[x][y] = true;
		for(int k=0; k < 4; k++) {
			int nx = x + dx[k];
			int ny = y + dy[k];
			
			if(nx >= 0 &&nx < n && ny >= 0 && ny < m) {
				if(graph[nx][ny] == 1 && check[nx][ny] == false) {
					dfs(n, m, cnt+1, nx, ny);
				}
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] input = br.readLine().split(" ");
		int N = Integer.parseInt(input[0]);
		int M = Integer.parseInt(input[1]);
		
		graph = new int[N][M];
		check = new boolean[N][M];
		for(int i=0; i < N; i++) {
			String line = br.readLine();
			for(int j=0; j < M; j++) {
				graph[i][j] = line.charAt(j) - '0';
			}
		}
		
		dfs(N, M, 1, 0, 0);
		Collections.sort(ans);
		System.out.println(ans.get(0));
		br.close();
	}
}
```

2. Java(BFS 이용)
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
	public static int[][] graph;
	public static int[][] group;
	
	public static void bfs(int n, int m) {
		Queue<Pair> q = new LinkedList<Pair>();
		q.add(new Pair(0, 0));
		group[0][0] = 1;
		
		while(!q.isEmpty()) {
			Pair p = q.remove();
			int x = p.row;
			int y = p.column;
			
			for(int k=0; k < 4; k++) {
				int nx = x + dx[k];
				int ny = y + dy[k];
				if(nx >= 0 &&nx < n && ny >= 0 && ny < m) {
					if(graph[nx][ny] == 1 && group[nx][ny] == 0) {
						q.add(new Pair(nx, ny));
						group[nx][ny] = group[x][y] + 1;
					}
				}
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] input = br.readLine().split(" ");
		int N = Integer.parseInt(input[0]);
		int M = Integer.parseInt(input[1]);
		
		graph = new int[N][M];
		group = new int[N][M];
		for(int i=0; i < N; i++) {
			String line = br.readLine();
			for(int j=0; j < M; j++) {
				graph[i][j] = line.charAt(j) - '0';
			}
		}
		
		bfs(N, M);
		System.out.println(group[N-1][M-1]);
		br.close();
	}
}
```
