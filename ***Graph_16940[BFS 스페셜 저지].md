[16940](https://www.acmicpc.net/problem/16940)

1. Java
```java
import java.io.*;
import java.util.*;

public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int N = Integer.parseInt(br.readLine());
		ArrayList<Integer>[] graph = new ArrayList[N];
		boolean[] check = new boolean[N]; // 방문 여부
		int[] parent = new int[N];  // 해당 정점의 바로 앞 정점
		int[] order = new int[N];  // BFS 방문 순서
		
		for(int i=0; i < N; i++)
			graph[i] = new ArrayList<Integer>();
		
		for(int i=0; i < N-1; i++) { // 그래프 생성
			String[] input = br.readLine().split(" ");
			int from = Integer.parseInt(input[0]) - 1;
			int to = Integer.parseInt(input[1]) - 1;
			graph[from].add(to);
			graph[to].add(from);
		}
		
		String[] input = br.readLine().split(" ");
		for(int i=0; i < N; i++) {
			order[i] = Integer.parseInt(input[i]) - 1;
		}
		
		
		// BFS를 통해 순서가 맞는지 검증
		Queue<Integer> q = new LinkedList<Integer>();
		q.add(0);
		check[0] = true;
		int m = 1;
		
		for(int i=0; i < N; i++) {
			if(q.isEmpty()) {
				System.out.println(0);
				System.exit(0);
			}
			int x = q.remove();
			if(x != order[i]) {
				System.out.println(0);
				System.exit(0);
			}
			// 연결된 노드 방문하면서 차수 구하고,
			// 부모 노드 정보 입력
			int cnt = 0;
			for(int y : graph[x]) {
				if(!check[y]) {
					parent[y] = x;
					cnt += 1;
				}
			}
			for(int j=0; j < cnt; j++) {
				if(m+j >= N || parent[order[m+j]] != x) {
					System.out.println(0);
					System.exit(0);
				}
				q.add(order[m+j]);
				check[order[m+j]] = true;
			}
			m += cnt;
		}
		System.out.println(1);
		br.close();
	}
}
```
