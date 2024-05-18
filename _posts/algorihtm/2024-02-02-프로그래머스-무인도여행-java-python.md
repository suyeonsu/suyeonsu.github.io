---
title: "[프로그래머스] 무인도 여행 - Java, Python"
date: 2024-02-01 23:45 +09:00
categories: [알고리즘, 프로그래머스]
tags: [프로그래머스, bfs]
math: true
toc: true
author: suyeonsu
pin: false
image:
  path: /assets/img/thumbnails/무인도 여행.png
  alt: "프로그래머스 무인도 여행"
---

## 문제

[무인도 여행](https://school.programmers.co.kr/learn/courses/30/lessons/154540)

<br>

## 아이디어
> 문제 설명은 자바를 기준으로 합니다. 풀이 과정은 동일하나 파이썬 코드는 [여기](#파이썬)를 참고해주세요.

> **bfs 탐색**으로 각각의 섬을 방문하여 그 합을 구해 저장한다.  
> 참고로 새로운 섬 탐색을 시작할 때 visited 배열을 초기화하면 안된다. 
{: .prompt-tip }

*괜히 새로운 섬을 탐색할 때 visited를 초기화하는 바람에 테케는 다맞는데 돌려보면 틀려서 방황했다.*

<br>

1. 각 섬의 누적값을 저장할 리스트를 만든다.  
    섬이 몇 개 있을지 모르니 `answer`를 배열이 아닌 List로 만들었다. 반환시 배열로 변환한다.  
    ```java
    List<Integer> answer = new ArrayList<>();
    ```

2. **탐색을 시작할 지점('X'가 아니면서 방문한 적 없는 곳)**을 찾는다.  
```java
    n = maps.length;
    m = maps[0].length();
    visited = new int[n][m];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (maps[i].charAt(j) != 'X' && visited[i][j] == 0) {
```

3. 해당 지점부터 상하좌우 1칸씩 'X'가 아닌 곳으로 bfs 탐색을 하며 **값을 누적**한다.  
    **탐색이 끝나면(dq가 비었으면) 누적한 값을 answer에 추가**한다.
```java
                Queue<Point> dq = new LinkedList<>();
                int sum = maps[i].charAt(j) - '0';

                dq.offer(new Point(i, j));
                visited[i][j] = 1;
                while (!dq.isEmpty()) {
                    Point cur = dq.poll();
                    for (int k = 0; k < 4; k++) {
                        int nx = cur.x + dx[k];
                        int ny = cur.y + dy[k];
                        if (0 <= nx && nx < n && 0 <= ny && ny < m && visited[nx][ny] == 0 && maps[nx].charAt(ny) != 'X') {
                            visited[nx][ny] = 1;
                            sum += maps[nx].charAt(ny) - '0';
                            dq.offer(new Point(nx, ny));
                        }
                    }
                }
                answer.add(sum);
            }
        }
    }
```
 
4. `answer`에 값이 있으면 정렬하여 반환한다.  
```java
    return !answer.isEmpty() ? answer.stream().mapToInt(Integer::intValue).sorted().toArray() : new int[] {-1};
```

<br>

## 전체 코드 

#### 자바
```java
import java.util.*;

class Solution {
    int[] dx = {-1, 0, 1, 0};
    int[] dy = {0, -1, 0, 1};
    int n, m;
    int[][] visited;

    static class Point {
        int x;
        int y;

        Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    public int[] solution(String[] maps) {
        List<Integer> answer = new ArrayList<>();
        n = maps.length;
        m = maps[0].length();
        visited = new int[n][m];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (maps[i].charAt(j) != 'X' && visited[i][j] == 0) {
                    Queue<Point> dq = new LinkedList<>();
                    int sum = maps[i].charAt(j) - '0';

                    dq.offer(new Point(i, j));
                    visited[i][j] = 1;
                    while (!dq.isEmpty()) {
                        Point cur = dq.poll();
                        for (int k = 0; k < 4; k++) {
                            int nx = cur.x + dx[k];
                            int ny = cur.y + dy[k];
                            if (0 <= nx && nx < n && 0 <= ny && ny < m && visited[nx][ny] == 0 && maps[nx].charAt(ny) != 'X') {
                                visited[nx][ny] = 1;
                                sum += maps[nx].charAt(ny) - '0';
                                dq.offer(new Point(nx, ny));
                            }
                        }
                    }
                    answer.add(sum);
                }
            }
        }
        return !answer.isEmpty() ? answer.stream().mapToInt(Integer::intValue).sorted().toArray() : new int[] {-1};
    }
}
```

#### 파이썬
```python
from collections import deque

def solution(maps):
    answer = []
    n, m = len(maps), len(maps[0])
    visited = [[0]*m for _ in range(n)]
    dx = [-1, 0, 1, 0]
    dy = [0, -1, 0, 1]

    for i in range(n):
        for j in range(m):
            if maps[i][j] != 'X' and not visited[i][j]:
                dq = deque([(i, j)])
                visited[i][j] = 1
                s = eval(maps[i][j])
                while dq:
                    x, y = dq.popleft()
                    for k in range(4):
                        nx = x+dx[k]
                        ny = y+dy[k]
                        if 0 <= nx < n and 0 <= ny < m and not visited[nx][ny] and maps[nx][ny] != 'X':
                            dq.append((nx, ny))
                            visited[nx][ny] = 1
                            s += eval(maps[nx][ny])
                answer.append(s)
                
    return sorted(answer) if answer else [-1]
```

<br>

## 노트

### 리스트(List)를 배열(Array)로 바꾸기

1. 리스트 -> **객체 타입 배열**  
    > list.**toArray()**
    {: .prompt-tip }
    ```java
    List<Integer> list = new ArrayList<>();
    Integer[] arr = list.toArray(Integer[]::new);
    ```

2. 리스트 -> **기본 타입 배열**  
    > list.**stream().mapToInt(Integer::intValue).toArray()**
    {: .prompt-tip }
    > mapToInt() , mapToDouble(), mapToLong() ...  

    ```java
    List<Integer> list = new ArrayList<>();
    list.stream().mapToInt(Integer::intValue).toArray();            // 정렬 x
    list.stream().mapToInt(Integer::intValue).sorted().toArray();   // 정렬 o
    ```

### 배열(Array)을 리스트(List)로 바꾸기

1. 객체 타입 배열 -> 리스트  
    > **Arrays.asList(**arr**)**  
    > **List.of(**arr**)**
    {: .prompt-tip }
    > 이 때 반환되는 리스트는 **고정 사이즈 리스트**로 **원소 추가, 삭제가 불가능**하다.
    {: .prompt-warning}

    ```java
    String[] arr = { "A", "B", "C" };
    List<String> list1 = Arrays.asList(arr);
    List<String> list2 = List.of(arr);
    ```

    변경 가능한 리스트로 바꾸려면 ArrayList 생성자로 감싸주자
    ```java
    List<String> list = new ArrayList<String>(Arrays.asList(arr));
    ```

2. 기본 타입 배열 -> 리스트
    > **Arrays.stream(**arr**).boxed().collect(Collectors.toList())**
    {: .prompt-tip }
    > 리스트는 기본 타입(Primitive type)을 지원하지 않기 때문에 **boxed()를 이용해 래퍼 타입(Wrapper type)으로 박싱한 후에 리스트로 변환**한다.

    ```java
    int[] arr = {1, 2, 3};
    List<Integer> list = Arrays.stream(arr).boxed().collect(Collectors.toList());
    ```