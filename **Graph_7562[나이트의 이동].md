[7562](https://www.acmicpc.net/problem/7562)

1. Java(BFS)
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
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		int[] dx = {-2, -1, 1, 2, 2, 1, -1, -2};
		int[] dy = {1, 2, 2, 1, -1, -2, -2, -1};
		
		int T = Integer.parseInt(br.readLine());  // 테스트 케이스 개수
		while(T-- > 0) {
			int l = Integer.parseInt(br.readLine()); // 체스판의 한 변의 길이 (4 ≤ l ≤ 300)
			String[] input = br.readLine().split(" ");  // 현재 있는 칸 입력
			Pair start = new Pair(Integer.parseInt(input[0]), Integer.parseInt(input[1]));
			input = br.readLine().split(" ");  // 이동하려는 칸 입력
			Pair end = new Pair(Integer.parseInt(input[0]), Integer.parseInt(input[1]));
			
			int[][] group = new int[l][l];
			Queue<Pair> q = new LinkedList<Pair>();
			q.add(start);
			while(!q.isEmpty()) {
				Pair p = q.remove();
				int x = p.row;
				int y = p.column;
				if(x == end.row && y == end.column) {
					sb.append(group[x][y] + "\n");
					break;
				}				
				for(int k=0; k < 8; k++) {
					int nx = x + dx[k];
					int ny = y + dy[k];
					
					if(nx >= 0 && nx < l && ny >= 0 && ny < l) {
						if(group[nx][ny] == 0) {
							group[nx][ny] = group[x][y] + 1;
							q.add(new Pair(nx, ny));
						}
					}
				}
			}
		}
		System.out.print(sb);
		br.close();
	}
}
```
