[13023](https://www.acmicpc.net/problem/13023)

- 이 문제의 경우는 반드시 추후 다른 방법을 이용하여 풀어 볼 것!!!

1. Java(BOJ 풀이)
- 간략히 설명하면, 인접행렬 / 인접리스트 / 간접 리스트를 모두 이용함
- 각각의 자료형이 어떤식으로 쓰인지는 코드 내에 주석 참고

```java
import java.io.*;
import java.util.ArrayList;

class Edge{
	int from, to;
	Edge(int from, int to){
		this.from = from;
		this.to = to;
	}
}

public class Main {	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] input = br.readLine().split(" ");
		int N = Integer.parseInt(input[0]);
		int M = Integer.parseInt(input[1]);
		boolean[][] a = new boolean[N][N];    // 인접행렬
		ArrayList<Integer>[] graph = (ArrayList<Integer>[]) new ArrayList[N]; // 인접리스트
		ArrayList<Edge> edges = new ArrayList<Edge>();  // 간선 리스트
		for(int i=0; i < N; i++) {
			graph[i] = new ArrayList<Integer>();
		}
		// 그래프 정보를 입력받아서 위 3개의 자료형에 각각 정보를 저장
		for(int i=0; i < M; i++) {
			input = br.readLine().split(" ");
			int from = Integer.parseInt(input[0]);
			int to = Integer.parseInt(input[1]);
			//인접행렬
			a[from][to] = true;
			a[to][from] = true;
			//인접리스트
			graph[from].add(to);
			graph[to].add(from);
			//간선 리스트
			edges.add(new Edge(from, to));
			edges.add(new Edge(to, from));
		}
		//1. 간접리스트에서 각각 연결된 A-B, C-D 찾기
		for(int i=0; i < M*2; i++) {  // M*2 이유는 A-B, B-A 처럼 2개가 저장되었으니까
			for(int j=0; j < M*2; j++) {
				int A = edges.get(i).from;
				int B = edges.get(i).to;
				int C = edges.get(j).from;
				int D = edges.get(j).to;
				// 서로 같은 놈이 하나라도 있으면 안된다.
				if (A == B || A == C || A == D || B == C || B == D || C == D) {
                    continue;
                }
				//2. 인접행렬을 이용하여 B와 C가 연결되어 있는지 확인
				if(!a[B][C]) continue;
				//3. 마지막으로 D와 연결된 A,B,C,D가 아닌 정점이 있는지 확인
				for(int E : graph[D]) {
					if (E == A || E == B || E == C || E == D) {
                        continue;
                    }
					System.out.println(1);
					System.exit(0);
				}
			}
		}
		System.out.println(0);		
		br.close();
	}
}
```
