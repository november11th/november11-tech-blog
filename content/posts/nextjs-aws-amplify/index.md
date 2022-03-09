---
title: Next.js 이미지 AWS Amplify 캐시 최적화 
date: 2022-03-09
tags:
- React
- Next.js
- AWS
- Amplify
---

이미지와 같은 정적 리소스는 AWS Amplify에서 캐시를 설정해서 사이트를 최적화한다. 

1. Amplify 콘솔에서 성능 모드(Performance mode) 활성화
   - `앱설정` -> `일반` -> `브랜치` 섹션에서 설정할 수 있다. 

2. 커스텀 헤더로 리소스별 캐시 기간을 설정한다.
   - `앱설정` -> `사용자 지정 헤더` 에서 설정할 수 있다.
   - `max-age`는 브라우저 캐싱 기간을 설정한다.
   - `s-maxage`는 CloudFront 캐싱 기간을 설정한다. (default 600초)
     - 성능 모드를 활성화 하면 CloudFront 캐싱의 최소 TTL이 **10분(600초)**, 최대 TTL이 **1일(86,400초)**로 지정된다.
       s-maxage이 최대 TTL인 1일을 초과하는 경우 Amplify는 s-maxage를 최대 TTL인 1일로 지정한다.
   - 예제) next/image의 CloudFront 캐싱 기간을 1일(86,400초), 브라우저 캐싱 기간을 일주일(604,800초)로 설정하는  
     ```yaml
     customHeaders:
      - pattern: /_next/image*
        headers:
          - key: Cache-Control
            value: 'max-age=604800, s-maxage=86400' 
     ```
