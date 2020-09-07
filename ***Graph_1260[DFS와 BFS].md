[1260](https://www.acmicpc.net/problem/1260)

- 설명은 낼와서 달기!!!! 주석도!!!

1. Java
```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Stack;

public class Main {
	static ArrayList<Integer>[] graph;
	static boolean[] check;
	
	static void dfs(int x) {
		if (check[x]) return;
		
		check[x] = true;
		System.out.print(x + " ");
		for(int y : graph[x]) {
			if(!check[y])
				dfs(y);
		}
	}
	
	static void bfs(int start) {
		Queue<Integer> q = new LinkedList<Integer>();
		q.add(start);
		check[start] = true;
		
		while(!q.isEmpty()){
			int x = q.remove();
			System.out.print(x + " ");
			for(int y : graph[x]) {
				if(!check[y]) {
					check[y] = true;
					q.add(y);
				}
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] input = br.readLine().split(" ");
		int N = Integer.parseInt(input[0]);
		int M = Integer.parseInt(input[1]);
		int V = Integer.parseInt(input[2]);
		
		graph = (ArrayList<Integer>[]) new ArrayList[N+1];
		for (int i=1; i<=N; i++)
			graph[i] = new ArrayList<Integer>();
		
		for(int i=0; i < M; i++) {
			input = br.readLine().split(" ");
			int from = Integer.parseInt(input[0]);
			int to = Integer.parseInt(input[1]);
			graph[from].add(to);
			graph[to].add(from);
		}
		
		for (int i=1; i <= N; i++)  // 중요!!!!
            Collections.sort(graph[i]);
		
		check = new boolean[N+1];
        dfs(V);
        System.out.println();
        
        check = new boolean[N+1];
        bfs(V);
        System.out.println();
		
		br.close();
	}
}
```
