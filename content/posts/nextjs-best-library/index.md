---
title: Next.js URL 구조 설계
date: 2022-03-06
tags:
- React
- Next.js
- URL
- SEO
---

>
> Next.js 공식 홈페이지의 [문서]( https://nextjs.org/learn/seo/rendering-and-ranking/url-structure )를 기반으로 작성했습니다.
>

### Next.js URL 구조 설계 원칙

- 의미(Semantic)<br/>
  ID 또는 랜덤 숫자 대신 의미가 있는 단어를 사용하는 것이 가장 좋다. <br />
  예를 들어 `learn/course-1/lesson-1` 보다 `/learn/basics/create-nextjs-app`이 SEO 측면에서 더 좋은 URL이다.
  - e.g. https://www.example.com/blog/seo-in-nextjs → pages/blog/[blog-name].js


- 논리적이고 일관된 패턴(Patterns that are logical and consistent)
  URL은 페이지 간에 일관된 패턴을 따라야 한다. <br />
  예를 들어 제품 페이지의 경우 각 제품이 서로 다른 경로를 갖는 대신 모든 제품 페이지를 그룹화하는 폴더를 만드는 것이 좋다. 
  - e.g. https://www.example.com/products/nextjs-shirt → pages/products/[product].js

- 키워드 포커스(Keyword focused)
  구글은 여전히 웹사이트가 포함한 키워드에 기반을 두고 있다. 페이지의 목적을 쉽게 전달하려면 URL에 키워드를 사용하길 권한다.   
  
- 매개변수 기반을 지양(Not parameter-based)
  URL에 매개변수를 사용하는 것은 일반적으로 좋지 않다. 매개변수는 대부분의 경우에 의미가 없으며 검색 엔진을 혼란스럽게 만들 수 있다. 이는 검색 결과 노출 순위에 악영향을 끼칠 수 있다. 


 
