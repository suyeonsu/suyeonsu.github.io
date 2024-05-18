---
title: "Intellij 창전환(alt + tab) 자동 빌드 해제하는 온갖 방법"
date: 2024-03-12 12:22 +09:00
last_modified_at: 2024-03-13 03:00 +09:00
categories: [Tips, Intellij]
tags: [intellij]
author: suyeonsu
pin: false
image: 
  path: /assets/img/thumbnails/아니 자동빌드 안할건데.png
  alt: "아니! 자동 빌드 안할건데?!"
description: 인텔리제이 자동 빌드를 해제해보자.
---

인텔리제이와 다른 창들을 왔다갔다 하면서 작업을 하고 있었는데 자꾸 시도때도없이 빌드가 되는 상황이 펼쳐졌다.  
코드에 아무런 수정이 없음에도 불구하고 자기 혼자 빌드하더니 빌드 성공했다고 하단 dock에서 통통 튀면서 날뛰고..  
물론 자동 빌드되면 편리하지만 그건 소스 수정이 있을때나 편한 것 같다..  

구글에 검색해봐도 자동 빌드 켜는 법만 나왔다.  
나같은 문제를 겪는 사람은 okky에 올라온 글 하나가 전부였는데 그마저도 해결책을 찾지 못한 글이었다.  
*(선생님🥹.. 제 글 봐주세요..!!)*  

곰곰이 생각해보다가  
자동빌드를 켜는 방법이 여러가지던데 **모조리 반대로 해주면 되지 않을까?** 라며 내 안의 금쪽이가 기어나왔다.  

결과부터 말하자면 이젠 알트탭해도 빌드되지 않는다.  

혹시 나처럼 빌드에 시달리는 사람들이 있다면 아래 방법들을 모두 해보길 바란다.

<br>

---

<br>

Intellij 버전: 2023.03

## 1. spring boot devtools 디펜던시 확인

혹시나 build.gradle과 application.yml에 다음과 같이 설정되어 있다면 지운다

build.gradle
```java
developmentOnly("org.springframework.boot:spring-boot-devtools") 
```

applictaion.yml 
```java
spring:
  devtools:
    remote:
      restart:
        enabled: true
```

## 2. Preference 설정
Settings > Build,Excution,Deployment > Compiler  
Build project Automatically 체크 해제  
<img width="1094" alt="image" src="https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/a885b459-3181-4832-9dbb-df20ce889caf">

## 3. Edit Configurations 설정
Intellj 검색창(mac 기준 shift 2번 연타, 또는 Idle 우상단 돋보기 아이콘) > edit configurations 검색 > Edit Configurations...  
<img width="716" alt="image" src="https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/144efcb2-ce42-4ee3-84af-158a8777882a">  

On frame deactivation: **Do nothing**으로 설정 > Apply > OK  
(이 옵션이 Intellij 창이 비활성화 됐을 때의 동작을 설정하는 것 같다.)
![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/91b8ab9f-d836-455c-b65a-69f4f3bb1c91)

---

<br>

나는 여기까지 다 해서 자동 빌드가 해제됐다. (내 생각에 edit config가 가장 중요한 듯)  

현재 나는 On frame deactivation을 다시 Update resources로 설정해놨다.  
js나 html 작업할 때 바로바로 확인하기 좋다.  
(기존엔 Update classes and resources였는데 컴푸타가 조금 후져서 자동 빌드할 때 좀 무거웠다)

<br>

모쪼록 다들 행복한 코딩하길 바랍니닷