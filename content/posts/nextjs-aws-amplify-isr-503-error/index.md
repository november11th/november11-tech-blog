---
title: AWS Amplify로 호스팅한 Next.js에서 ISR 기능을 사용할 경우 503 에러 발생 문제 해결 방법
date: 2022-04-07
tags:
- React
- Next.js
- AWS
- Amplify
- Cloudfront
---

>
> 참고 URL : https://zenn.dev/eringiv3/articles/5fa044cc92c3a3
> 일본어 사이트라 한글 번역이 필요하다. 

### 현상
- ISR로 revalidate를 설정한 페이지에서 revalidate 시간 경과 후 접속하면 Cloudfront 의 503 에러가 발생
```typescript
503 ERROR 
The request could not be satisfied
```
- 

### 해결 방법
- Lambda@Edge에 구축된 Default Lambda 함수에 할당된 IAM role에 SQS 송신 권한을 추가해야 한다.  
  (해당 문제는 AWS Amplify에서 향후 개선할 것으로 예상된다.)
  
```typescript
{
            "Effect": "Allow",
            "Resource": "arn:aws:sqs:us-east-1:<ACCOUND-ID>:<SQS-ID>.fifo",
            "Action": [
                "sqs:SendMessage"
            ]
        }
```
