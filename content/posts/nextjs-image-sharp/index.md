---
title: Next.js 이미지 로딩이 느릴 경우 조치 방법
date: 2022-04-17
tags:
- React
- Next.js
- Next.js Image
---

> 참고 URL : 
- https://github.com/vercel/next.js/discussions/31336
- https://nextjs.org/docs/messages/sharp-missing-in-production


Next.js 에서 next/image를 사용하는 경우 이미지 렌더링에 3초 이상 시간이 소요되는 경우가 있다.

이 경우 아래 명령어를 사용해 sharp 패키지를 설치하면 로딩 속도가 개선된다.
`yarn add sharp`

Next.js는 기본 이미지 로더로 `squoosh`를 사용하고 한다.

하지만 상용 환경(`process.env.NODE_ENV === 'production'`)에서는 sharp 패키지를 추가 설치하기를 권장한다. 

> The next/image component's default loader uses squoosh because it is quick to install and suitable for a development environment. For a production environment using next start, it is strongly recommended you install sharp by running yarn add sharp in your project directory.

Next.js는 상용 환경에서 sharp 패키지를 찾을 수 없는 경우 관련 Warning 메시지를 출력한다. 

```typescript
console.warn(
  chalk.yellow.bold('Warning: ') +
    `For production Image Optimization with Next.js, the optional 'sharp' package is strongly recommended. 
    Run 'yarn add sharp', and Next.js will use it automatically for Image Optimization.\n` +
    'Read more: https://nextjs.org/docs/messages/sharp-missing-in-production'
)
```


