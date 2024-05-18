---
title: 깃블로그에 댓글 기능 추가하기 (Jekyll, Chirpy, Giscus)
date: 2023-08-05 23:20 +09:00
categories: [Tips, Jekyll]
tags: [Chirpy, Jekyll, Giscus]
math: true
toc: true
author: suyeonsu
pin: false
image: 
    path: /assets/img/thumbnails/298390507-55e58e3a-3c7f-4d38-a811-8d3c5e941b30.png
---

## 댓글 기능을 넣자 💬

역시나 바쁘단 핑계와 함께 미뤄두었던 댓글 기능을 추가했다. 이거 뭐 얼마나 걸린다고 미뤘던건지 모르겠는점...  
아직 댓글이 달릴 법한 포스팅은 없지만 댓글이 있어야 소통이 되니까 필요성을 느끼고 있었다.

<br>

## 여러 댓글 서비스들과 Giscus 💎

- Disqus
- Utterances
- Giscus

이렇게 세 가지 서비스를 가장 많이 사용하는 것 같다.  
이 중에서 가장 유명한 서비스는 `Disqus`인데, 내 아담한 기록용 기술 블로그에 적용하기엔 불필요한 기능들이 많아 보였다.  

`Utterances`와 `Giscus`는 둘 다 Github API를 사용해 댓글을 저장하는 서비스인데, Repository에 댓글을 저장하고 API로 이를 불러오는 형태이다.  
차이점으로는 `Utterances`가 리포지토리의 Issue를 사용한다면 `Giscus`는 Discussions를 사용한다는 것.  

내 생각에 댓글을 저장하는 위치가 Issue 보다는 Discussions가 더 적합하다고 생각해서 최종적으로 Giscus를 선택하게 되었다.  
그 외에도 Giscus는 **댓글 정렬, reaction, 그리고 다양한 테마를 적용할 수 있다는 점**이 상당히 매력적이었다.

<br>

## Giscus를 적용해보자

1. 먼저 [giscus 앱](https://github.com/apps/giscus)을 **댓글을 저장할 Repository**에 설치한다.  
    나는 내 깃블로그 저장소를 선택했다.
    
1. 선택한 저장소의 Settings > General > Features 에서 Discussions를 체크해준다.  
    ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/a9eba44e-3efc-49a5-8f64-6562dba55b7c)
    
2. [Giscus 설정 페이지](https://giscus.app/ko)에서 내 블로그에 추가하기 위한 설정을 해준다.  
    (설정 페이지가 한국어를 지원해서 굉장히 편리하다)  

    - 댓글을 저장할 저장소 이름을 입력한다. (1번에서 giscus 앱을 설치한 저장소)
        ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/ada6b398-54c5-4d84-9380-fcb693797573)
        
    - 페이지와 Discussions를 연결할 방법을 선택한다.  
        Github API가 댓글을 조회할 수 있는 방법인데 `경로`를 포함하는 것을 권장한다.
        ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/80187fdd-f5ea-436e-ae12-6d6862dd2969)
        
    - Giscus를 이용하여 게시글에 댓글을 달면 Discussion을 생성하고 댓글이 달리는데, 해당 Discussion에 사용 될 카테고리를 선택한다.  
        (Announcements가 권장이라서 그대로 설정해줬다)
        ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/f51d5606-959c-4d51-a05d-eed83ff56852)
        
    - 추가 기능을 선택해준다.  
        나는 **댓글 느리게 불러오기**(게시글에서 댓글 근처로 스크롤할 때까지 댓글 로딩을 지연함)와 **댓글 위에 댓글 상자 배치(**댓글 입력 상자를 댓글들의 상단에 배치)를 체크해줬다.  
        개인적으로 **댓글 위에 댓글 상자 배치**는 사용자 편의성을 위해 체크하는 것을 추천한다.  
        ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/01b84d01-c716-4446-9eb1-3c352158cc6c)
        
    - 원하는 테마를 선택한다.
        ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/19e868f7-f527-4357-8ea7-1b52ffefeefc)
        
    
    모든 설정을 마치면 아래에 script 태그가 생성되는데 이를 내 블로그에 적용하면 된다.  
    (레포 Id와 카테고리 Id는 위에서 입력하고 선택한 것에 따라 자동으로 생성된다.)
    
    ```jsx
    <script src="https://giscus.app/client.js"
            data-repo="suyeonsu/suyeonsu.github.io"
            data-repo-id="{repoId}"
            data-category="Announcements"
            data-category-id="{catogoryId}"
            data-mapping="pathname"
            data-strict="0"
            data-reactions-enabled="1"
            data-emit-metadata="0"
            data-input-position="top"
            data-theme="preferred_color_scheme"
            data-lang="ko"
            data-loading="lazy"
            crossorigin="anonymous"
            async>
    </script>
    ```

<br>    

## Chirpy 테마에 추가하기

Chirpy 테마는 댓글 기능을 적용하기에 굉장히 편리하게 되어있다.
1. _config.yml 에서 `comments`의 `active`에 `giscus`를 입력한다.  
    생성된 `<script>`대로 `comments` 아래 값들을 설정해준다.
    ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/f1eb0d11-6ce4-46b8-87e7-e34867644a4b)
    
2. _includes > comments > giscus.html 에서 변수들을 설정해준다.  
    ![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/0d571050-3a49-43bb-94b0-239561f479c8)
    
모든 설정이 끝났다. 이렇게 하면 이제 당신의 블로그에 댓글 스크립트가 만들어졌을 것입니당.

<br>

## 적용된 모습

이제 사람들이 Github 로그인을 통해 내 블로그에서 댓글을 작성할 수 있다!!  
![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/551aab18-e3d7-4a0d-af63-b70f2e9b752e)

<br>

## 댓글 관리는? 

**저장소의 Discussions 탭**에서 관리할 수 있다!  
선택한 맵핑 방식에 따라(`pathname`이나 `url` 또는 `title`) 연관된 discussion에 가보면 해당 게시글의 댓글들을 볼 수 있다.  
여기에서 댓글들을 삭제하거나 숨기거나 관리하면 된다.  

![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/e40fc9bd-9510-4f28-8cbf-154d12f9f14c)

![image](https://github.com/suyeonsu/suyeonsu.github.io/assets/54935106/5fc4268a-4f62-45f9-abd1-6220d7061c37)