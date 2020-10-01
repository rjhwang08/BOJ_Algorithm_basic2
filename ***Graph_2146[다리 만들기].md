[2146](https://www.acmicpc.net/problem/2146)

1. Java(BFS 2번 사용)
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
	public static final int[] dx = {0, 1, 0, -1};
	public static final int[] dy = {1, 0, -1, 0};
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int N = Integer.parseInt(br.readLine());
		int[][] graph = new int[N][N];
		int[][] group = new int[N][N];
		for(int i=0; i < N; i++) {
			String[] input = br.readLine().split(" ");
			for(int j=0; j < N; j++) {
				graph[i][j] = Integer.parseInt(input[j]);
			}
		}
		
		// 먼저 각각의 섬들을 구분하기 위해 그룹화를 한다.(아파트 단지 문제)
		int cnt = 0;
		for(int i=0; i < N; i++) {
			for(int j=0; j < N; j++) {
				if(graph[i][j] == 0 || group[i][j] != 0) {
					continue;
				}
				Queue<Pair> q = new LinkedList<Pair>();
				group[i][j] = ++cnt;
				q.add(new Pair(i, j));
				while(!q.isEmpty()) {
					Pair p = q.remove();
					int x = p.row;
					int y = p.column;
					
					for(int k=0; k < 4; k++) {
						int nx = x + dx[k];
						int ny = y + dy[k];
						
						if(nx >= 0 && nx < N && ny >= 0 && ny < N) {
							if(graph[nx][ny] == 1 && group[nx][ny] == 0) {
								group[nx][ny] = group[x][y];
								q.add(new Pair(nx, ny));
							}
						}
					}
				}
			}
		}
		
		// 각각의 섬들 사이에서 가장 거리가 짧은 다리의 길이를 구한다.
		// 여기서는 토마토 문제처럼 bfs를 이용해서 거리를 구해준다.
		int[][] distance = new int[N][N];
		int ans = -1;  // 답 저장
		
		// 섬 하나를 기준으로 다른 섬들의 거리를 구해서 비교하는 과정
		for(int l = 1; l <= cnt; l++) {
			Queue<Pair> q = new LinkedList<Pair>();
			for(int i=0; i < N; i++) {
				for(int j=0; j < N; j++) {
					// l번째 섬만 표시한다.
					distance[i][j] = -1;
					if(group[i][j] == l) {
						distance[i][j] = 0;
						q.add(new Pair(i, j));
					}
				}
			}
			
			// 큐에 저장된 l번째 섬에 속하는 점들에 대해서 bfs를 실행
			// 해당 섬에서 지도 전 범위까지 거리를 구한다.
			while(!q.isEmpty()) {
				Pair p = q.remove();
				int x = p.row;
				int y = p.column;
				for(int k=0; k < 4; k++) {
					int nx = x + dx[k];
					int ny = y + dy[k];
					if(nx >= 0 && nx < N && ny >= 0 && ny < N) {
						if(distance[nx][ny] == -1) {
							distance[nx][ny] = distance[x][y] + 1;
							q.add(new Pair(nx, ny));
						}
					}
				}
			}
			
			// 아래 과정을 통해 이제 다른 섬까지의 최소 거리를 구해서 비교, 저장한다.
			for(int i=0; i < N; i++) {
				for(int j=0; j < N; j++) {
					// 아래 조건문을 통해 l번째 섬이 아닌 다른 섬을 구분한다.
					if(graph[i][j] == 1 && group[i][j] != l) {
						if(ans == -1 || distance[i][j] - 1 < ans) {
							ans = distance[i][j] - 1;
						}
					}
				}
			}
		}		
		System.out.println(ans);
		br.close();
	}
}
```
