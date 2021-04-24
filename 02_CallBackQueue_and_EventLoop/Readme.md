콜백 큐와 이벤트 루프
=====================
>자바스크립트는 **싱글 스레드 기반**으로 **단일한 콜스택**을 가진다고 배웠다.<br>
싱글 스레드로 코드를 실행하는 것은 멀티 스레드 환경에서 발생하는 DeadLock과 같은<br>
복잡한 시나리오를 고려할 필요가 없으므로 사용하기 매우 쉽다.<br>
그러나 단 하나의 콜스택을 가지는 자바스크립트에서, 하나의 함수가 연산이 오래 걸려<br>
다른 함수를 실행하는데 지장이 생기는 경우에는 어떻게 해야할까?<br>

비동기 콜백
--------------
제일 쉬운 해결책은 **비동기 콜백(Asynchronous callbacks)** 이다.<br>
자바스크립트 엔진은 우리의 코드를 실행하면서 즉시 실행될 함수와 나중에 실행될 함수를 나누고,<br>
**나중에 실행될 함수에 대한 콜백**을 제공해 준다. 다시 말하면 비동기 콜백은 즉시 실행되는 것이 아닌,<br>
특수한 시점에 실행되므로 `console.log`와 같은 동기 함수와는 다르게 스택 안에 바로 `push` 되지 않는다.<br>
콜스택이 아니라면 콜백을 어디서 관리하는 것일까?

실행 환경(Run Time)
-------------------
<img src="https://github.com/mauv2sky/33_Concepts_Of_JS/blob/main/02_CallBackQueue_and_EventLoop/image/1.png" width="600">

브라우저는 다음과 같은 요소들로 구성되어 있다.
* 자바스크립트 엔진(`Javascript Engine`)
* Web APIs
* 콜백 큐(`Callback Queue`)
* 이벤트 루프(`Event Loop`)
