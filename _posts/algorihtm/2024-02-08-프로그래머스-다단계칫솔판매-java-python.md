---
title: "[프로그래머스] 다단계 칫솔 판매 - Java, Python"
date: 2024-02-08 22:20 +09:00
categories: [알고리즘, 프로그래머스]
tags: [프로그래머스, 재귀]
math: true
toc: true
author: suyeonsu
pin: false
image:
  path: /assets/img/thumbnails/다단계 칫솔 판매.png
  alt: "프로그래머스 다단계 칫솔 판매"
---

## 문제

[다단계 칫솔 판매 (2021 Dev-Matching: 웹 백엔드 개발자(상반기))](https://school.programmers.co.kr/learn/courses/30/lessons/77486)

<br>

*다단계 탈출 완료!*

<br>

## 아이디어

**재귀**를 통해 그래프를 탐색하여 풀 수 있는 문제이다.  

1. 먼저 enroll(직원)과 referral(추천인)을 통해 그래프 정보를 구성한다.  
    문제를 보면 판매사원의 매출을 해당 위치부터 부모 노드로 올라가며 나눠 갖게된다.  
    즉, **`자식 -> 부모` 방향의 탐색이 필요**하므로 노드들은 부모 노드 이름을 가지고 있어야 한다.  

    따라서 **각 노드가 가지고 있어야할 정보는 `부모 노드, 나의 수익`**이 되고,  
    **해시** 자료형으로 **모든 직원들에 대해 `직원 이름: 노드 정보`를 구성**한다.  

    <br>

2. seller(판매원)와 amount(수량)를 순차 탐색하며,  
    i번째 판매원 `seller[i]`의 판매액 `amount[i]*100`을 **`해당 직원, 부모, 조상, ... , 루트` 가 나눠 갖는다.**

    이를 위해 **'직원 -> 루트' 방향으로 탐색하며 각 직원들의 수익을 업데이트**한다.  
    **재귀 함수**를 통해 이러한 반복 작업을 수행할 수 있다.  
    >재귀 과정에서 필요한 정보는 **현재 노드, 나눠 가질 수익** 이다.  
    >
    >먼저 `현재 노드(child)`가 `수익(money)`의 10%를 제외한 금액을 갖는다.  
    >
    >그리고 **제외한 10% 를 (부모, 조상 ...) 끼리 나눠가지라고 재귀 호출**로 전달해줘야 하는데,  
    다음 두 경우는 탐색하지 않는다.  
    >- **부모 노드가 루트 노드**인 `center(-)`인 경우  
    >(루트 노드 수익은 결과값에 포함되지 않기 때문에 계산할 필요 x)  
    > 
    >- 전달해주는 **10%가 0.XXX원**인 경우  
    >문제에서 `10%를 계산한 금액이 1원 미만인 경우에는 이득을 분배하지 않고 자신이 모두 가집니다.`라는 조건이 있어서 현재 노드가 수익을 모두 가져가기 때문이다.  
    >(예를 들어, 자식 `sam`으로부터 `edward`가 2원을 받았을 때 2원의 10%는 0.2원 이므로 `edward`는 `mary`에게 10%를 주지않고 본인이 모두 갖는다.)
    {: .prompt-info }

    <br>

3. enroll 배열에 있는 직원 이름 순서대로 각 직원이 갖게 되는 이득을 answer에 저장하여 반환한다.

<br>

## 전체 코드

#### 자바 풀이
```java
import java.util.*;

class Solution {
    
    static Map<String, Node> info;

    static class Node {
        String parent;
        int profit;

        Node(String parent) {
            this.parent = parent;
            this.profit = 0;
        }
    }

    static void dfs(String child, int money) {
        String parent = info.get(child).parent;
        int fee = (int)(money*0.1);
        info.get(child).profit += money-fee;
        if (!parent.equals("-") && fee > 0) {
            dfs(parent, fee);
        }
    }

    public int[] solution(String[] enroll, String[] referral, String[] seller, int[] amount) {
        int[] answer = new int[enroll.length];
        info = new HashMap<>();
        for (int i=0; i<enroll.length; i++) {
            info.put(enroll[i], new Node(referral[i]));
        }

        for (int i=0; i<seller.length; i++) {
            dfs(seller[i], amount[i]*100);
        }

        for (int i=0; i<enroll.length; i++) {
            answer[i] = info.get(enroll[i]).profit;
        }

        return answer;
    }
}
```

#### 파이썬 풀이
```py
def solution(enroll, referral, seller, amount):
    answer = []
    info = {e: [r, 0] for e, r in zip(enroll, referral)}
    
    def dfs(child, money):
        p = info[child][0]
        fee = int(money*0.1)
        info[child][1] += money-fee
        if p != "-" and fee > 0:
            dfs(p, fee)
        
    for s, a in zip(seller, amount):
        dfs(s, a*100)
    
    for e in enroll:
        answer.append(info[e][1])
    
    return answer
```

<br>

## 노트

### java.lang.NullPointerException 원인
`Exception in thread "main" java.lang.NullPointerException`

- **문자열을 비교할 땐 `==`가 아닌 `equals()`**  
    IDE에서는 잘 돌아갔는데 채점 돌리니 NPE가 발생했었다.  
    그 원인은 문자열 비교시 == 으로 비교해서 였다.  
    
    또한 equals()를 사용할 때 비교 문자열을 먼저 배치하면 NPE를 피할 수 있다.  
    ```java
    if (!str.equals("-")) { ... }   // NPE 발생 가능
    if (!"-".equals(str)) { ... }   // NPE 발생 x
    ```

- **`toString()` 대신 `String.valueOf()`**  
    toString()의 경우 해당 변수가 null일 경우 NPE가 발생한다.  
    String.valueOf()를 사용하면 그대로 null이 반환되어 NPE가 발생하지 않는다.
    ```java
    Integer x = null;
    System.out.println(x.toString());       // 실행 결과: ~~~~ java.lang.NullPointerException
    System.out.println(String.valueOf(x));  // 실행 결과: null
    ```