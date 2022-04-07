---
title: AWS Amplify에서 Next.js 를 호스팅할 경우 사용되는 Lamda@Edge 구조 설명 
date: 2022-04-07
tags:
- React
- Next.js
- AWS
- Amplify
- Serverless
---

>
> 참고 URL : https://zenn.dev/laiso/articles/8c619c38bd7b7b
> 일본어 사이트라 한글 번역이 필요하다. 

AWS Amplify에서는 Serverless Next.js Component를 사용해서 호스팅 서비스를 제공한다. 
이때 사용하는 Lambda@Edge는 다음과 같다. (Lambda@Edge의 목록은 us-east-1 리전에서 확인 가능하다.)

- Image Lambda@Edge for Next CloudFront distribution
- Default Lambda@Edge for Next CloudFront distribution
- API Lambda@Edge for Next CloudFront distribution
- Next.js Regeneration Lambda

#### AWS Amplify 배포로그 내 Lamda@Edge Function 정보 
```typescript
...
[INFO]: - SSR Lambda@Edge: p8rgni3-k1v0td7h <- Default Lambda@Edge
[INFO]: - API Lambda@Edge: p8rgni3-lz11kaj <- API Lambda@Edge
[INFO]: - Image Optimization Lambda@Edge: p8rgni3-bzlof5 <- Image Lambda@Edge
[INFO]: - ISR Lambda: p8rgni3-ewtgiam <- Next.js Regeneration Lambda
...
```
<br />

#### Amplify에서 설정한 Cloudfront 동작 목록

| 경로 패턴 | Origin | 
| :------ |:------|
| _next/static/* | S3 버킷 | 
| static/* | S3 버킷 |
| api/* | API Lambda@Edge | 
| _next/data/* | Default Lambda@Edge | 
| _/next/image* | Image Lambda@Edge | 
| 기본값(*) | Default Lambda@Edge | 

static, api, image를 제외한 모든 요청은 `Default Lambda@Edge`로 전달된다. 

<br />

#### Default Lambda@Edge
- SSR이 발생하는 경우 Default Lambda@Edge에서 처리하고 응답한다. 
- ISR 처리가 필요한 경우에는 SQS로 메시지를 송신하여 'Next.js Regeneration Lambda' 함수를 호출한다. 
- [관련 소스](https://github.com/serverless-nextjs/serverless-next.js/blob/62c0fe2e0c9d6341831d496bf143369d8df0a93e/packages/libs/lambda-at-edge/src/default-handler.ts)

#### API Lambda@Edge
- 요청된 경로에 맞춰 API를 처리한다. 
- [관련 소스](https://github.com/serverless-nextjs/serverless-next.js/blob/62c0fe2e0c9d6341831d496bf143369d8df0a93e/packages/libs/lambda-at-edge/src/image-handler.ts)

#### Image Lambda@Edge
- 이미지 최적화를 처리한다. 
- 이미지가 외부에 존재하는 경우 해당 함수가 호출되는 시점에 이미지를 가져와 처리한다.
- [관련 소스](https://github.com/serverless-nextjs/serverless-next.js/blob/62c0fe2e0c9d6341831d496bf143369d8df0a93e/packages/libs/lambda-at-edge/src/image-handler.ts)

#### Next.js Regeneration Lambda
- ISR을 처리하는 함수이며 SQS 메시지를 통해 트리거된다. 
- 해당 함수가 호출되면 SSG를 수행한 후 S3 버킷에 응답한다. 
- Revalidate 시간을 반영하여 html 파일의 메타 정보에 입력하며,  
  Default Lambda@Edge 함수는 이를 참고하여 ISR 실행 여부를 결정한다.
- [관련 소스](https://github.com/serverless-nextjs/serverless-next.js/blob/62c0fe2e0c9d6341831d496bf143369d8df0a93e/packages/libs/lambda-at-edge/src/regeneration-handler.ts)
