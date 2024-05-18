---
title: "[프로그래머스] 문자열 압축 (2020 KAKAO BLIND RECRUITMENT) - Java, Python"
date: 2024-03-14 00:00 +09:00
last_modified_at:
categories: [알고리즘, 프로그래머스]
tags: [프로그래머스, 문자열]
math: true
toc: true
author: suyeonsu
pin: false
# image:
#   path:
#   alt: 
---


# 문제

[문자열 압축 (2020 KAKAO BLIND RECRUITMENT)](https://school.programmers.co.kr/learn/courses/30/lessons/60057)  

*- 문자열 찌부 완료 -*

<br>

## 아이디어

문자열 + 단순한 구현 문제였다.  
입력되는 문자열 `s`의 최대 길이가 1000으로 작기 때문에 문자를 1개, 2개, 3개, ... , 1000개 단위로 잘라서 압축하는 경우의 수를 모두 탐색한다.  

1. 문자열 `s`를 k개 단위로 잘라 저장한다. 이 때 문자열이 k개로 딱 나누어 떨어지지 않는 경우도 있음에 주의하자.  
  예를 들어 'abcabcd'를 3개 단위로 자르는 경우 'abc', 'abc', 'd'가 된다.  
  맨 마지막 문자열이 누락되는 일이 없도록 나머지 연산을 통해 인덱스를 계산해 마지막 substring을 추가한다.  
  ```java
      for (int k=1; k<=s.length(); k++) {
          List<String> l = new ArrayList<>();
          for (int i=0; i<s.length()-k+1; i+=k) {
              l.add(s.substring(i, i+k));
          }
          if (s.length()%k >= 1) {
              l.add(s.substring(s.length()-s.length()%k, s.length()));
          }
  ```

2. 자른 문자열이 담긴 배열에서 현재 문자열과 그 다음 문자열의 값을 비교하며 cnt와 cur을 갱신한다.  
  for 반복문이 종료되면서 마지막에 append 되지 않은 문자열까지 빠뜨리지 않고 추가한다.  
  ```java
      String cur = l.get(0);
      int cnt = 1;
      StringBuilder sb = new StringBuilder();
      for (int i=1; i<l.size(); i++) {
          if (l.get(i).equals(cur)) {
              cnt++;
          } else {
              if (cnt > 1) {
                  sb.append(String.valueOf(cnt));
              }
              sb.append(cur);
              cur = l.get(i);
              cnt = 1;
          }
      }
      sb.append(cnt > 1 ? String.valueOf(cnt)+cur : cur);
  ```

3. 압축된 문자열의 최소 길이를 갱신하며 정답을 구한다.

<br>

## 전체 코드

#### 자바 풀이
```java
import java.util.*;

class Solution {
    public int solution(String s) {
        int answer = Integer.MAX_VALUE;
        for (int k=1; k<=s.length(); k++) {
            List<String> l = new ArrayList<>();
            for (int i=0; i<s.length()-k+1; i+=k) {
                l.add(s.substring(i, i+k));
            }
            if (s.length()%k >= 1) {
                l.add(s.substring(s.length()-s.length()%k, s.length()));
            }
            
            String cur = l.get(0);
            int cnt = 1;
            StringBuilder sb = new StringBuilder();
            for (int i=1; i<l.size(); i++) {
                if (l.get(i).equals(cur)) {
                    cnt++;
                } else {
                    if (cnt > 1) {
                        sb.append(String.valueOf(cnt));
                    }
                    sb.append(cur);
                    cur = l.get(i);
                    cnt = 1;
                }
            }
            sb.append(cnt > 1 ? String.valueOf(cnt)+cur : cur);
            answer = Math.min(answer, sb.toString().length());
        }
        return answer;
    }
}
```

#### 파이썬 풀이
```py
def solution(s):
    answer = 1e9
    for k in range(1, len(s)+1):
        l = []
        for i in range(0, len(s), k):
            l.append(s[i:i+k])

        cur = l[0]
        cnt = 1
        res = []
        for i in range(1, len(l)):
            if l[i] == cur:
                cnt += 1
            else:
                if cnt > 1:
                    res.append(str(cnt))
                res.append(cur)
                cur = l[i]
                cnt = 1
        res.append(str(cnt) + cur if cnt > 1 else cur)
        answer = min(answer, len(''.join(res)))
    return answer
```

<br>

## 노트

파이썬은 문자열이나 리스트를 슬라이싱할 때 end 인덱스로 길이를 넘는 값을 줘도 에러가 발생하지 않는다.  
내부적으로 알아서 데이터의 사이즈로 지정하여 슬라이싱 된다.  
```py
x = [1, 2, 3]
print(x[:9999])   # [1, 2, 3]
```

하지만 자바에서 substring을 사용할 땐 인덱스 범위에 주의하여 사용하도록 하자.  
```java
String x = "123";
System.out.println(x.substring(0, 9999));   // StringIndexOutOfBoundsException
```
