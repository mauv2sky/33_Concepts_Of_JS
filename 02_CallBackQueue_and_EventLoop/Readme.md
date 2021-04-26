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
콜스택이 아니라면 어디서 콜백을 관리하는 걸까? 브라우저의 실행환경부터 차근히 살펴보자.

실행 환경(Run Time)
-------------------
<img src="https://github.com/mauv2sky/33_Concepts_Of_JS/blob/main/02_CallBackQueue_and_EventLoop/image/1.png" width="600">

브라우저에는 단순히 자바스크립트 엔진만 존재하는 것은 아니다.<br>
DOM, AJAX, setTimeOut 등의 브라우저에서 제공하는 **WebAPI**가 있고,<br>WebAPI 호출을 통제하기 위한 **CallBack Queue**와 **Event Loop**가 있다.<br>
콜백의 `보관` 및 `실행`은 **CallBack Queue**와 **Event Loop**에서 비동기적으로 동작한다.<br>

### 콜백 큐(Callback Queue)
**큐(Queue)** 는 기본적으로 선입선출(**F**irst **I**n **F**irst **O**ut, **FIFO**)로 동작하는 자료구조다.<br> 비동기적으로 실행되는 `WebAPI` 함수가 호출되면
`WebAPI`가 자바스크립트 런타임 환경의<br>콜백 큐에 해당 콜백을 삽입한다.

### 이벤트 루프(Event Loop)
이벤트 루프는 자바스크립트 엔진 내의 호출 스택이 완전히 비는 순간에<br>콜백 큐안의 함수를 하나씩 빼내어 호출 스택으로 삽입한다. 

이벤트 루프와 비동기 콜백의 처리 과정
----------------------------------
```javascript
setTimeOut(function(){
    console.log("first log");
}, 0);
console.log("second log");
```
아마 자바스크립트의 이벤트 루프와 콜백 큐에 대해 알지 못했더라면<br>
로그가 `first log`, `second log` 순으로 출력되길 기대했을 것이다.<br>

<img src="https://github.com/mauv2sky/33_Concepts_Of_JS/blob/main/02_CallBackQueue_and_EventLoop/image/2.png" width="700">

코드를 실행하면 `second log`가 먼저 출력되는 것을 볼 수 있다.<br> 

### 이미지로 이해하기
<br>
<img src="https://github.com/mauv2sky/33_Concepts_Of_JS/blob/main/02_CallBackQueue_and_EventLoop/image/3.png" width="600">

`setTimeOut`의 두 번째 인자가 0(ms) 일지라도 `setTimeOut`은 곧장 콜백 큐로 들어가야 한다.<br>
WebAPI에 의해 호출된 `first log` 콜백함수는 콜백 큐로 삽입된다.<br>

<img src="https://github.com/mauv2sky/33_Concepts_Of_JS/blob/main/02_CallBackQueue_and_EventLoop/image/4.png" width="600">

이후 `second log`가 콜 스택에 삽입된다.<br>

<img src="https://github.com/mauv2sky/33_Concepts_Of_JS/blob/main/02_CallBackQueue_and_EventLoop/image/6.png" width="600">

`second log`가 출력되고 콜 스택에서 삭제된다.<br>

<img src="https://github.com/mauv2sky/33_Concepts_Of_JS/blob/main/02_CallBackQueue_and_EventLoop/image/5.png" width="600">

콜 스택이 비어있음이 확인되면 **이벤트 루프**는 콜백 큐에 있는 `first log` 콜백 함수를 콜 스택에 삽입한다.<br>이제 콜 스택에 들어간 `first log` 함수는 실행 가능 상태가 된 것이다.