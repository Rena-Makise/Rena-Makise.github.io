---
title: Dijkstra Algorithm
categories:
  - algorithm
tags:
  - Algorithm
last_modified_at: 2019-12-22T13:00:00+09:00
toc: false
use_math: true
---

## Dijkstra Algorithm

#### 개념

* 다익스트라 알고리즘은 하나의 정점에서 다른 모든 정점까지의 최단 경로를 구한다.
* 간선들은 모두 양의 간선들을 가져야 한다.
* 다익스트라 알고리즘의 기본 로직은, 첫 정점을 기준으로 연결되어 있는 정점들을 추가해가며, 최단 거리를 갱신하는 것이다.
* 정점을 잇기 전까지는 시작점을 제외한 정점들은 모두 무한대의 값을 가진다.
* 정점 A에서 정점 B로 이어지면, 정점 B가 가지는 최단 거리는 (시작점부터 정점 A까지의 최단거리 + 점 A와 점B 간선의 가중치)와 (기존에 가지고 있던 정점 B의 최단거리) 중 작은 값이 B의 최단 거리가 된다.

#### 우선순위 큐(힙구조)를 이용한 다익스트라 알고리즘

![](https://i.imgur.com/1iaO28v.png)

* 먼저 모든 정점들을 힙(우선순위 큐)에 넣는다.

  ![](https://i.imgur.com/7lRgqUh.png)

* 위 표에서 i는 정점 인덱스, d는 최단거리이고 p는 이전 정점이다. d를 기준으로 최소 힙으로 구성한다.

* 힙에서 가장 위에 있는 값을 꺼낸다. 인덱스가 5이고 최소 거리가 0인 값이다.

* 기존에 있던 dist[5]와 d를 비교한다. dist[5]가 더 작으면 연산하지 않고, 같거나 크면 연산한다.

* 둘 다 0으로 같으므로, 5 주변의 인접 정점을 계산하고 기존의 dist[i]보다 더 작아지는 정점들을 큐에 넣는다.

  ```java
  dist[2] = min(dist[2], dist[5] + adj[5][2])
  dist[4] = min(dist[4], dist[5] + adj[5][4])
  ```

  ![](https://i.imgur.com/tu0xgGS.png)

* 힙에서 가장 위에 있는 값을 꺼낸다. 인덱스가 4이고 최소거리가 2인 값이다.

* 기존에 있던 dist[4]와 d를 비교한다. dist[4]가 더 작으면 연산하지 않고, 같거나 크면 연산한다.

* dist[4]는 2이고 d는 2로 같으므로, 4 주변의 인접 정점을 계산해서 기존의 dist[i]보다 더 작아지는 정점들을 큐에 넣는다.

  ```java
  dist[2] = min(dist[2], d + adj[4][2])
  dist[3] = min(dist[3], d + adj[4][3])
  ```

  ![](https://i.imgur.com/3nH2w0K.png)

* 힙에서 가장 위에 있는 값을 꺼낸다. 인덱스가 2이고 최소거리가 3인 값이다.

* 기존에 있던 dist[2]와 d를 비교한다. dist[2]가 더 작으면 연산하지 않고, 같거나 크면 연산한다.

* dist[2]는 3이고, d는 3으로 같으므로, 2 주변의 인접 정점을 계산해서 기존의 dist[i]보다 더 작아지는 정점들을 큐에 넣는다.

  ```java
  dist[1] = min(dist[1], dist[2] + adj[2][1])
  ```

  ![](https://i.imgur.com/R1B50mU.png)

* 힙에서 가장 위에 있는 값을 꺼낸다. 인덱스가 3이고 최소 거리가 3인 값이다.

* 기존에 있던 dist[3]과 d를 비교한다. dist[3]이 더 작으면 연산하지 않고, 같거나 크면 연산한다.

* dist[3]은 3이고, d는 3으로 같으므로, 3 주변의 인접 정점을 계산해서 기존의 dist[i]보다 더 작아지는 정점들을 큐에 넣는다.

  ```java
  dist[4] = min(dist[4], dist[3] + adj[3][4])
  ```

* 여기서 dist[4]는 2로 계산값보다 더 작으므로 큐에 넣지 않는다.

  ![](https://i.imgur.com/nAq646S.png)

* 힙에서 가장 위에 있는 값을 꺼낸다. 인덱스가 2이고 최소 거리가 4인 값이다.

* dist[2] = 3보다 d = 4가 더 크므로 계산하지 않는다.

  ![](https://i.imgur.com/EcNQWq2.png)

* 힙에서 가장 위에 있는 값을 꺼낸다. 인덱스가 1이고 최소거리가 6인 값이다.

* 기존에 있던 dist[1]과 d를 비교한다. dist[1]은 6이고 d는 6으로 같으므로, 1주변의 인접 정점을 계산해서 기존의 dist[i]보다 더 작아지는 정점들을 큐에 넣는다.

  ```java
  dist[4] = min(dist[4], dist[1] + adj[1][4])
  dist[3] = min(dist[3], dist[1] + adj[1][3])
  ```

* 둘다 기존의 dist[i]보다 크므로 큐에 넣지 않는다.

  ![](https://i.imgur.com/38olzkc.png)

* 큐에 남아있는 정점들은 전부 d가 INF로 dist[i]보다 크므로 계산되지 않는다.

#### 코드

```java
    /**
     * 배달 - 다익스트라 알고리즘
     * 하나의 정점에서 다른 모든 정점까지의 최단 경로를 구하는 문제 해결법
     * 단, 다익스트라 알고리즘에서는 음의 가중치를 가진 간선은 쓸 수 없다!!!
     * https://hsp1116.tistory.com/42 참고
     * @param N  마을의 갯수
     * @param road  도로의 정보
     * @param K  음식 배달이 가능한 시간
     * @return
     */
    public static int solution(int N, int[][] road, int K) {
        int answer = 0;
        int[] dist = new int[N + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);  // 처음에 dist 배열을 INF값으로 채워준다.

        // 그래프 제작
        List<Node>[] nodes = new ArrayList[N + 1];
        for (int i = 0; i < nodes.length; ++i) {
            nodes[i] = new ArrayList<>();
        }

        for (int i = 0; i < road.length; ++i) {
            nodes[road[i][0]].add(new Node(road[i][1], road[i][2]));
            nodes[road[i][1]].add(new Node(road[i][0], road[i][2]));
        }

        Queue<Node> pq = new PriorityQueue<>((a, b) -> a.weight - b.weight);
        dist[1] = 0;  // 시작정점의 최단 경로는 0이다.
        pq.add(new Node(1, 0));

        while (!pq.isEmpty()) {
            Node c = pq.poll();

            // 현재 정점까지의 비용이 이미 구한 비용보다 크다면 무시
            if(c.weight > dist[c.index])
                continue;
        
            // 현재 정점과 연결된 정점들을 탐색한다.
            for (Node n : nodes[c.index]) {
                // (현재 정점까지 온 비용 + 다음 정점까지 가는 비용) < 이미 구한 다음 정점까지의 비용이면,
                if (dist[c.index] + n.weight < dist[n.index]) {
                    dist[n.index] = dist[c.index] + n.weight;   // 다음 정점까지의 비용을 갱신하고
                    pq.add(new Node(n.index, dist[c.index] + n.weight));  // 큐에 삽입

                }
            }   
        }

        // 1부터 다른 마을까지의 비용 중 K보다 작게 걸린다면 배달 가능!
        for (int i = 1; i < dist.length; ++i) {
            if (dist[i] <= K)
                answer++;
        }
        
        return answer;
    }
```

