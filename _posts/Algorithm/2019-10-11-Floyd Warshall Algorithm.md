---
title: Floyd-Warshall Algorithm
categories:
  - algorithm
tags:
  - Algorithm
last_modified_at: 2019-12-22T13:00:00+09:00
toc: false
use_math: true
---

## Floyd-Warshall Algorithm

#### 개념

* 플로이드-워셜 알고리즘은 모든 최단경로를 구하는 방법이다.
* 플로이드-워셜 알고리즘에서는 음의 가중치를 가진 간선도 쓸 수 있다.
* 모든 정점에 대한 경로를 계산하므로 거리를 저장할 자료구조는 2차원 배열이 된다.
* 플로이드-워셜은 optimal substructure의 개념을 이용하여 최단 경로를 찾는다.
* optimal substructure은 특정 경로 안에 무수히 많은 경로가 있을 때, 중간 정점들이 각각 최단이 된다면 이를 모두 이은 경로 또한 최단이 된다는 개념이다.

#### 기본 로직

* 플로이드-워셜에서는 2개의 테이블을 사용하는데, 하나는 모든 경로에 대한 비용을 저장하는 테이블, 나머지 하나는 각 정점까지 가기 직전의 정점을 저장한 테이블이다. 각각의 테이블을 D와 P라고 해보자.

* 테이블 D와 P에는 처음엔 인접 리스트에 대한 내용만 들어가게 된다. 그 후 경로를 추가 할 때마다 두 테이블이 갱신된다.

  ![](https://i.imgur.com/rVwRerD.png)

* 플로이드-워셜에선 처음엔 인접리스트의 정보만 들어간다고 했다. 따라서 맨 처음에 초기화되는 테이블 D, P는 다음과 같다.

  ![](https://i.imgur.com/1UZCUS9.png)

* 여기서 INF는 무한대를 뜻하고, NIL은 직전 정점이 없다(즉 연결되어있지 않다)는 뜻이다. 위 정점은 아무런 정점을 중간 경로로 사용하지 않고, 오직 인접 리스트만 표현한 것이기 때문에, 그 외는 INF, NIL로 명시되었다.

* 이제 여기서 경로 1을 중간 경로로 하는 테이블을 구해보자. 4->2가 중간경로 1을 추가하면 4->1->2로 접근할 수 있고, 4->5도 4->1->5로 접근할 수 있게 된다.

  ![](https://i.imgur.com/ZMRvY1S.png)

* 이렇게 경로가 갱신되며, 갱신될 시 테이블 P의 해당 컬럼 또한 1로 갱신된다. 직전 경로가 갱신되므로 테이블 P를 이용하면 최단 경로 또한 구할 수 있다.

* 이를 모든 정점을 경로에 추가할 때 까지 반복한다.

#### 코드

```java
    /**
     * 배달 - 플로이드-워셜 알고리즘
     * 모든 최단 경로를 구하는 문제에서 사용
     * 다익스트라와는 다르게 음의 가중치를 가진 간선도 쓸 수 있다.
     * https://hsp1116.tistory.com/45?category=547783 참고
     * @param N  마을의 갯수
     * @param road  도로의 정보
     * @param K  음식 배달이 가능한 시간
     * @return
     */
    public static int solution2(int N, int[][] road, int K) {
        int answer = 0;

        // 거리를 저장한 테이블
        int[][] mapD = new int[N + 1][N + 1];

        for (int i = 0; i < mapD.length; ++i) {
            Arrays.fill(mapD[i], 987654321); // 도달 못함(INF)으로 초기화. 더했을 때 오버플로우가 나지 않을 정도의 수로
            mapD[i][i] = 0; // 자기 자신의 비용은 0
        }

        // 그래프 제작
        for (int i = 0; i < road.length; ++i) {
            // a<->b인 경로가 여러개 존재하면 가장 비용이 낮은 경로로 설정해준다.
            if (mapD[road[i][0]][road[i][1]] > road[i][2]) {
                mapD[road[i][0]][road[i][1]] = road[i][2];
                mapD[road[i][1]][road[i][0]] = road[i][2];
            }
        }

        // 중간경로 추가
        // 첫번째 반복문 - 거쳐가는 정점
        // 두번째 반복문 - 출발하는 정점
        // 세번째 반복문 - 도착하는 정점
        for (int k = 1; k < N + 1; ++k) {
            for (int i = 1; i < N + 1; ++i) {
                for (int j = 1; j < N + 1; ++j) {
                    if (mapD[i][j] > mapD[i][k] + mapD[k][j]) {
                        mapD[i][j] = mapD[i][k] + mapD[k][j];
                    }
                }
            }
        }

        // 배달 가능한 지역 탐색
        for (int i = 1; i < N + 1; ++i) {
            if (mapD[1][i] <= K) {
                answer++;
            }
        }

        return answer;
    }
```

