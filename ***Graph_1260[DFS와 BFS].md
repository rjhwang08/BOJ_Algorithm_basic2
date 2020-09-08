[1260](https://www.acmicpc.net/problem/1260)

- bfs와 dfs에 대한 개념적 문제
- BOJ 제출에 간선리스트로 구현된 답과 dfs를 비재귀로 구현한 2개의 답이 있으니 반드시 참고
- ArrayList를 정렬하는 Collections.sort() 사용법을 잘 알아둬야 함
[참고1(상세)](https://wjheo.tistory.com/entry/Java-%EC%A0%95%EB%A0%AC%EB%B0%A9%EB%B2%95-Collectionssort) [참고2(간단)](https://includestdio.tistory.com/35)

1. Java
```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
	static ArrayList<Integer>[] graph;
	static boolean[] check;
	
	static void dfs(int x) {  // 재귀 함수
		if (check[x]) { //만약 정점 x에 방문했었다면, 바로 직전 정점으로 돌아온다.
			return;
		}
		check[x] = true;  // 현재 방문한 정점 x를 true로 변경
		System.out.print(x + " ");
		for(int y : graph[x]) {  // 정점 x와 연결된 정점들을 차례로 확인
			if(!check[y])  // 정점 y에 방문한 적이 없으면,
				dfs(y);   // y에서 다시 dfs를 돌린다.
		}
	}
	
	static void bfs(int start) {  // 비재귀 함수
		Queue<Integer> q = new LinkedList<Integer>();  // 정점들을 방문한 순서를 기록
		q.add(start);  // 정점 start를 queue에 넣는다.
		check[start] = true;
		// 이제 queue가 empty상태가 될 때까지 반복문을 돌린다.
		while(!q.isEmpty()){  // bfs에서 empty가 된다는 건 더이상 다음에 방문할 정점이 없다는 것을 의미
			int x = q.remove();  //현재 방문한 점을 queue에서 제거
			System.out.print(x + " ");
			for(int y : graph[x]) {  // x와 연결된 정점들을 차례로 확인
				if(!check[y]) {  // 정점 y에 방문한 적이 없으면, queue에 저장한다.
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
		// ArrayList를 정렬하기 위해서는 java.util.Collections클래스의 sort() 메소드를 이용한다.
		for (int i=1; i <= N; i++)
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
