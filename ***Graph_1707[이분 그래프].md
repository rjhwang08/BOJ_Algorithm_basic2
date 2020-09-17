[1707](https://www.acmicpc.net/problem/1707)

1. Java(BFS 이용)
```java
import java.io.*;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
	static int[] check;
	static ArrayList<Integer>[] graph;	
	static void bfs(int start) {
		Queue<Integer> q = new LinkedList<Integer>();
		check[start] = 1;
		q.add(start);
		while(!q.isEmpty()) {
			int x = q.remove();
			for(int y : graph[x]) {
				if(check[y] != 0) continue;
				check[y] = 3 - check[x];
				q.add(y);
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		int k = Integer.parseInt(br.readLine());
		
		while(k-- > 0) {
			String[] input = br.readLine().split(" ");
			int V = Integer.parseInt(input[0]);
			int E = Integer.parseInt(input[1]);
			
			graph = (ArrayList<Integer>[]) new ArrayList[V+1];
			check = new int[V+1];
			for(int i=1; i <= V; i++)
				graph[i] = new ArrayList<Integer>();
			
			for(int i=1; i <= E; i++) {
				input = br.readLine().split(" ");
				int from = Integer.parseInt(input[0]);
				int to = Integer.parseInt(input[1]);
				graph[from].add(to);
				graph[to].add(from);
			}
			for(int i=1; i <= V; i++) {
				if(check[i] == 0) bfs(i);
			}
			boolean flag = true;
			for(int i=1; i <= V; i++) {
				for(int j : graph[i]) {
					if(check[i] == check[j]) {
						flag = false;
						break;
					}
				}
				if(flag == false) break;
			}
			if(flag == true) sb.append("YES\n");
			else sb.append("NO\n");
		}
		System.out.print(sb);
		br.close();
	}
}
```

2. Java(DFS 이용)
- BOJ 풀이 참고
