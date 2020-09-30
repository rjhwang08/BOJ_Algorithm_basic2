[16964](https://www.acmicpc.net/problem/16964)

1. Java(DFS)
```java
import java.io.*;
import java.util.*;

public class Main {
	static ArrayList<Integer>[] graph;
	static ArrayList<Integer> dfs_order = new ArrayList<>(); // DFS 방문 순서 저장
	static boolean[] check;
	
	static void dfs(int x) {
		if(check[x]) return;
		
		dfs_order.add(x);
		check[x] = true;
		for(int y : graph[x]) {
			dfs(y);
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int N = Integer.parseInt(br.readLine());
		graph = new ArrayList[N];
		check = new boolean[N];
		for(int i=0; i < N; i++) {
			graph[i] = new ArrayList<>();
		}
		
		for(int i=0; i < N-1; i++) {
			String[] input = br.readLine().split(" ");
			int from = Integer.valueOf(input[0]) - 1;
			int to = Integer.valueOf(input[1]) - 1;
			graph[from].add(to);
			graph[to].add(from);
		}
		
		int[] b = new int[N];
		int[] order = new int[N];
		String[] input = br.readLine().split(" ");
		// 왜 b라는 배열을 별도로 만들어서 앞에 BFS와는 다르게 했느냐
		// 이번 풀이의 핵심은 주어진 DFS 방문 순서로 정렬하는 것임
		// 정렬 후 DFS를 돌렸을 때, 주어진 순서와 DFS 결과의 비교
		for(int i=0; i < N; i++) {
			b[i] = Integer.valueOf(input[i]) - 1; // 주어진 방문 순서
			order[b[i]] = i; // 이놈은 각 노드들이 몇 번째 방문 순서인지 저장
		}
		
		// 위 방문 순서를 바탕으로 graph의 순서를 조정해준다.
		for(int i=0; i < N; i++) {
			Collections.sort(graph[i], new Comparator<Integer>() {
				public int compare(Integer u, Integer v) {
					if(order[u] < order[v]) {
						return -1;
					} else if(order[u] == order[v]) {
						return 0;
					} else {
						return 1;
					}
				}
			});
		}
		
		dfs(0);
		boolean flag = true;
		for(int i=0; i < N; i++) {
			if(b[i] != dfs_order.get(i)) {
				flag = false;
				break;
			}
		}
		
		if(flag) System.out.println(1);
		else System.out.println(0);
		br.close();
	}
}
```
