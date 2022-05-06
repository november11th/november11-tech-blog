---
title: TIL - You don't know javascript 
date: 2022-05-06
tags:
- TIL
- javascript
---
---
### 22-05-05 
#### Chapter 1. Asynchrony: Now & Later

- nontrivial : 적지 않은
> Practically all nontrivial programs ever written (especially in JS) have in some way or another had to manage this gap, whether that be in waiting for user input, requesting data from a database or file system, sending data across the network and waiting for a response, or performing a repeated task at a fixed interval of time (like animation).

##### now와 later 사이의 gap을 관리하는 것이 Async의 핵심이다.  

- crop up : 불쑥 나타나다.
> But most JS developers have never really carefully considered exactly how and why it crops up in their programs, or explored various other ways to handle it.

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
