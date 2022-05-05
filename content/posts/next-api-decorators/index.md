---
title: next-api-decorators를 활용한 API 서버 만들기
date: 2022-05-05
tags:
- React
- Next.js
- next-api-decorators
- Next.js
---

> 
> 참고 URL : 
- https://next-api-decorators.vercel.app/docs/
- https://javascript.plainenglish.io/awesome-next-js-api-routes-with-next-api-decorators-d6117f963bee
> 

### 배경 
Next.js에서 제공하는 기본 API는 API Method에 대한 세분화가 어렵고 파라미터에 대한 검증이 불가능하다. 

백엔드 전용 프레임워크인 Nest.js에서 제공하는 데코레이터가 있으면 좋다고 생각해왔는데
next-api-decorators를 사용하면 이와 비슷한 수준으로 Controller 레이어를 처리할 수 있다. 

- 기본 API 처리 코드 
```typescript
export default function handler(req, res) {
  if (req.method === 'POST') {
    checkAuth();
    doSomething();
    res.status(200).json({ name: 'John Doe' })
  } else if (req.method === 'DELETE') {
    checkAuth();
    doSomethingElse();
    res.status(200).json({ status: 'Deleted' })
  } else if (req.method === 'GET') { // no auth
    const items = doAThirdThing();
    res.status(200).json({ data: items })
  } else {
    res.status(405).text('Method Not Allowed')
  }
}
```

- next-api-decorators를 활용한 코드
```typescript
import {
  createHandler,
  Post,
  Delete,
  Get,
} from "@storyofams/next-api-decorators";

class TempHandler {
  @Post()
  post() {
    return { method: "Post" };
  }

  @Delete()
  delete() {
    return { method: "Delete" };
  }

  @Get()
  get() {
    return { method: "Get" };
  }
}

export default createHandler(TempHandler);
```

### next-api-decorators 를 위한 사전 설정
1. 패키지 설치
`yarn add @storyofams/next-api-decorators`
2. (타이브크립트 기준)tsconfig.json 내 compilerOptions에 아래 설정 추가
```typescript
"experimentalDecorators": true,
"emitDecoratorMetadata": true
```
3. babel 관련 패키지 설치
```typescript
yarn add --dev @babel/core babel-plugin-transform-typescript-metadata @babel/plugin-proposal-decorators babel-plugin-parameter-decorator
```

### 추가 기능
1. 라우팅 매칭 
```typescript
// pages/api/user/[[...params]].ts
import { createHandler, Get, Param } from '@storyofams/next-api-decorators';

class UserHandler {
  @Get()
  public list() {
    return DB.findAllUsers();
  }

  @Get('/:id')
  public details(@Param('id') id: string) {
    return DB.findUserById(id);
  }

  @Get('/:userId/comments')
  public comments(@Param('userId') userId: string) {
    return DB.findUserComments(userId);
  }

  @Get('/:userId/comments/:commentId')
  public commentDetails(@Param('userId') userId: string, @Param('commentId') commentId: string) {
    return DB.findUserCommentById(userId, commentId);
  }
}

export default createHandler(UserHandler);
```
2. 유효성 검사
```typescript
import { IsNotEmpty, IsEmail } from 'class-validator';

export class CreateUserDTO {
  @IsEmail()
  email: string;

  @IsNotEmpty()
  fullName: string;
}

class UserHandler {
   @Post()
   createUser(@Body(ValidationPipe) body: CreateUserDTO) {
      return await DB.createUser(body);
   }
}

export default createHandler(UserHandler);
```

3. 파이프라인
```typescript
class UserHandler {
  @Get()
  users(@Query('active', ParseBooleanPipe) active: boolean) {
    // Do something with the `active` argument.
    return 'Active users';
  }
}
```

4. 미들웨어(주료 인증 로직에 활용)
```typescript
const JwtAuthGuard = createMiddlewareDecorator(
  (req: NextApiRequest, res: NextApiResponse, next: NextFunction) => {
    if (!validateJwt(req)) {
      throw new UnauthorizedException();
      // or
      return next(new UnauthorizedException());
    }

    next();
  }
);

class SecureHandler {
  @Get()
  @JwtAuthGuard()
  public securedData(): string {
    return 'Secret data';
  }
}
```
