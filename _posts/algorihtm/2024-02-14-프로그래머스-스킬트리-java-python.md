---
title: "[프로그래머스] 스킬트리 - Java, Python"
date: 2024-02-14 23:20 +09:00
last_modified_at:
categories: [알고리즘, 프로그래머스]
tags: [프로그래머스, 문자열]
math: true
toc: true
author: suyeonsu
pin: false
image:
  path: /assets/img/thumbnails/스킬트리.png
  alt: "프로그래머스 스킬트리"
---

# 문제

[스킬트리 Summer/Winter Coding(~2018)](https://school.programmers.co.kr/learn/courses/30/lessons/49993)

<br>

## 아이디어

문제 분류는 자료구조라고 되어있는데 간단한 문자열 문제인 것 같다.  
다른 사람들의 풀이를 보니 정규표현식을 사용하는 방법도 있는데 나는 그냥 반복문을 통해 겹치는 문자들을 뽑아내는 코드를 작성했다.  

1. 사용자의 `skill_tree`에서 정해진 순서인 `skill`에 들어있는 문자들만 뽑아 문자열을 만든다.  
2. `skill`의 시작이 1에서 만든 문자열로 시작하는지 체크한다.  
    (반드시 선수스킬이 있어야 다음 스킬을 사용할 수 있기 때문에 0번째 인덱스부터 일치해야한다.)  

<br>

## 전체 코드

#### 자바 풀이
```java
class Solution {
    public int solution(String skill, String[] skill_trees) {
        int answer = 0;
        for (String st: skill_trees) {
            String tmp = "";
            for (char x: st.toCharArray()) {
                if (skill.indexOf(x) != -1) {
                    tmp += x;
                }
            }
            if (skill.substring(0, tmp.length()).equals(tmp)) {
                answer++;
            }
        }
        return answer;
    }
}
```

#### 파이썬 풀이
```py
def solution(skill, skill_trees):
    answer = 0
    for st in skill_trees:
        st = ''.join([x for x in st if x in skill])
        if skill.startswith(st):
            answer += 1
    return answer
```

<br>

## 노트

### 자바의 startsWith와 endsWith

파이썬에서 문자열 다룰 때 유용하게 사용하던 `startswith()` 메서드가 자바에도 있다는 것을 처음 알았다.  
파이썬과 동일한 방식으로 사용한다.

#### startsWith()
```java
String prefix = "abc";
String str = "abcdef";
System.out.println(str.startsWith(prefix));     // true
```

#### endsWith()
```java
String suffix = "123";
String str = "xyz123";
System.out.println(str.endsWith(suffix));       // true
```