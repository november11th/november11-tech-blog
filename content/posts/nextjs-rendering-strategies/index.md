---
title: Next.js 렌더링 전략
date: 2022-03-06
tags:
  - React
  - Next.js
---

> 
> Next.js 공식 홈페이지의 [문서]( https://nextjs.org/learn/seo/rendering-and-ranking/rendering-strategies )를 기반으로 작성했습니다. 
> 

## Next.js 렌더링 전략

### Static Site Generation (SSG)

SSG는 빌드 단계에서 HTML 파일을 생성한다. Static Site Generation은 리액트 코드를 미리 렌더링 된 HTML 페이지로 만들기 때문에 성능이 좋고 검색엔진 최적화(SEO)에 적합하다(e.g. 블로그 게시물). 

```typescript
import { GetStaticProps } from 'next'

export const getStaticProps: GetStaticProps = async (context) => {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```

### Server-Side Rendering (SSR)

SSG와 같이 SSR도 클라이언트 브라우저에 응답하기 전에 미리 렌더링 되기 때문에 SEO에 적합하다. 빌드 시간에 미리 렌더링 하는 SSG와는 다르게 SSR은 리액트 코드를 요청 시간에 HTML로 렌더링 한다. 따라서 SSR은 매우 동적인 페이지에 적합하다(e.g. 뉴스 피드). 

```typescript
import { GetServerSideProps } from 'next'
export const getServerSideProps: GetServerSideProps = async (context) => {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```

### Incremental Static Regeneration (ISR)

페이지 개수가 많다면 빌드 과정에서 SSG로 모두 생성하기 어렵다. ISR을 사용하면 빌드 이후 시점에서 정적 페이지를 생성하거나 수정할 수 있다. ISR을 사용하면 개발자나 콘텐츠 편집자가 전체 사이트를 빌드하지 않고 페이지 단위로 정적인 HTML 파일을 생성할 수 있다. 이를 통해 수백만 개의 페이지를 정적으로 관리할 수 있다.  

```typescript
import { GetStaticPaths } from 'next'

export const getStaticPaths: GetStaticPaths = async () => {
  return {
    paths: [
      {params: {...}} // See the "paths" section below
    ],
    fallback: true, false or "blocking" // See the "fallback" section below
  };
}
```

### Client Side Rendering (CSR)

CSR을 사용하면 브라우저 상에서 자바스크립트로 전체 웹사이트를 렌더링 한다. 초기 HTML 파일을 로드하면 빈 화면인 상태에서 자바스크립트가 실행되어 리액트 코드를 HTML로 렌더링 한다. SSG, SSR과 다르게 CSR은 SEO에는 적합하지 않다. CSR은 데이터 양이 많은 대시보드, 계정 페이지나 검색 엔진에 노출이 필요하지 않은 페이지 렌더링에 적합하다(e.g. 고객 계정 대시보드). 
