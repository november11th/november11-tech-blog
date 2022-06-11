---
title: TIL - You don't know javascript 
date: 2022-05-07
tags:
- TIL
- javascript
---
### 22-05-12

- Promise.resolve를 사용하면 즉시값을 전달할 수 있다. 
```typescript
var p1 = new Promise( function(resolve,reject){
    resolve( 42 );
} );

var p2 = Promise.resolve( 42 );
```


---
### 22-05-11

- evident : 분명한, 눈에 띄는
> there’s something deeper wrong, which isn’t evident just in that code example.

- wreak : 큰 피해를 가하다
- havoc : 대파괴, 대혼란
> there’s still one more hazard that could wreak havoc. Can you spot what it is?

- errands : 심부름

---
### 22-05-08

- Countless : 무수한 
> Countless JS programs,

- workhorse : 열심히 일하는 사람 (일개미)
> The callback function is the async workhorse for JavaScript,

- continuation : 연속성

- deficient : 부족한 
>  both versions are deficient in explaining this code

---
### 22-05-07

- hog : 돼지, 독차지하다. 
> So, to make a more cooperatively concurrent system, one that’s friendlier and doesn’t hog the event loop queue, you can process these results in asynchronous batches,

- convoluted : 난해한, 복잡한
> Unfortunately, at the moment it’s a mechanism without an exposed API, and thus demonstrating it is a bit more convoluted.

##### Job은 event loop와 별개로 실행 순서에 우선 순위를 가진다. (즉시 실행은 아니지만 최우선 순위를 가진다)  
> later, but as soon as possible.

##### event loop와 Job의 차이를 알아야 한다. 

- crystal clear : 아주 분명한
> But before we do, we should be crystal clear on something:

- tenuous : 미약한
>  how tenuous a link there is between the way source code is authored
 

---
### 22-05-06

- do as they please : 원하는대로
> So, different browsers and JS environments do as they please, which can sometimes lead to confusing behavior.

- the preceding code : 선행 코드
> Most of the time, the preceding code will probably produce an object representation in your developer tools’ console that’s what you’d expect.

- innate : 타고난
> In other words, the JS engine has had no innate sense of time, but has instead been an on-demand execution environment for any arbitrary snippet of JS. It’s the surrounding environment that has always scheduled “events” (JS code executions).


- vastly : 대단히
> vastly simplified pseudocode to illustrate the concepts.

- conflate : 혼용하다.
> It’s very common to conflate the terms “async” and “parallel,” but they are actually quite different.

- subtle : 미묘한
> the problems would be much more subtle.

- run-to-completion : javascript는 single thread 이기 때문에 함수 실행의 원자성을 보장한다.
> Because of JavaScript’s single-threading, the code inside of foo() (and bar()) is atomic, which means that once foo() starts running, the entirety of its code will finish before any of the code in bar() can run, or vice versa. This is called run-to-completion behavior.

---
### 22-05-05 
#### Chapter 1. Asynchrony: Now & Later

- nontrivial : 적지 않은
> Practically all nontrivial programs ever written (especially in JS) have in some way or another had to manage this gap, whether that be in waiting for user input, requesting data from a database or file system, sending data across the network and waiting for a response, or performing a repeated task at a fixed interval of time (like animation).

##### now와 later 사이의 gap을 관리하는 것이 Async의 핵심이다.  

- crop up : 불쑥 나타나다.
> But most JS developers have never really carefully considered exactly how and why it crops up in their programs, or explored various other ways to handle it.

--- 

