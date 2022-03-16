---
title: Styled-component를 활용한 딤드(Dimmed) 이미지 만들기
date: 2022-03-17
tags:
- React
- Next.js
- Styled-component
---

딤(Dim) 또는 딤드(Dimmed)는 화면 또는 div 레이어를 어둡게 처리하는 것을 의미한다. 

일반적으로 다이얼로그가 출력되었을때 배경을 어둡게하거나,
카드 컴포넌트에서 이미지 상단의 텍스트의 가시성을 높여주기 위해 처리한다. 

리액트(Next.js)와 Styled-componet를 활용한 딤드 처리 방법을 간단하게 정리한다. 

`ImageWrapper`는 css를 사용해 after 요소로 딤드 레이어를 추가한다. <br />
여기서는 이미지 상단에 텍스트가 있는 경우를 가정하여 상단에서 하단으로 내려오는 그라데이션 딤드를 적용했다.

```typescript
const ImageWrapper = styled.div`
  &:after {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    
    content: "";
    background: linear-gradient(180deg, #000000 0%, rgba(0, 0, 0, 0.0001) 100%);
    mix-blend-mode: normal;
    opacity: 0.3;
  }
`;
```

사용 예제는 다음과 같다.

```typescript
<ImageWrapper>
    <Image
      src={이미지주소}
      width={400}
      height={200}
    />
</ImageWrapper>
```

