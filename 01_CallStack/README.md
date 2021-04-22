콜스택
====================
스택이란?
-------------
스택(stack)은 후입선출(**L**ast **I**n **F**irst **O**ut, **LIFO**)의 특징을 갖는 자료구조다.<br>
리스트의 한 쪽으로만 입력과 출력이 가능하고, 
기본적으로 `push`, `pop`, `peek`, `isEmpty`의 연산을 갖는다.<br>
* `push(item)` : 스택의 입력 연산. item을 스택의 가장 윗 부분에 추가한다.
* `pop()` : 스택의 출력 연산. 스택에서 가장 위에 있는 항목을 제거한다.
* `peek()` : 스택의 가장 위에 있는 항목을 반환한다.
* `isEmpty()` : 스택이 비어있으면 true를, 아니면 false를 반환한다.

### 스택에는 무엇이 들어갈까?
* 지역변수
* 함수로 들어온 인수(Arguments)
* 호출한 함수(caller)의 스택에 대한 정보
* 반환값 주소(return address)

### 스택의 사용 사례
* 콜스택(함수의 재귀호출)
* 웹 브라우저 방문기록(뒤로가기)
* 실행 취소(undo)
* 괄호 검사
* 후위표기법 계산

자바스크립트에서의 콜스택
-------------------------
자바스크립트는 **싱글 스레드 기반**으로 동작하는 인터프리터 언어다.<br>
싱글 스레드 기반은 **한 번에 단 하나의 작업(Task)** 만 처리할 수 있음을 뜻한다.<br>
자바스크립트는 작업을 처리할 때 `스택`, `힙`, `큐`로 구성된 단일 콜스택을 갖는다.

콜스택이란?
-----------
자바스크립트 엔진이 코드를 실행하는 동안 **함수를 추적하는 공간**을 **콜스택**이라고 한다.<br>
함수(`function`)의 호출(`call`)을 스택(`stack`)이라는 자료구조에 기록하는 것을 말한다.

### 코드로 이해하기
```javascript
function foo() {
    console.log("function foo is called");
    throw new Error("oops!");
}

function bar() {
    console.log("function bar is called");
    foo();
}

function baz() {
    console.log("function baz is called");
    bar();
}

baz();
```
위의 코드에서 `foo`, `bar`, `baz` 세 가지의 함수를 작성했다.<br>
세 함수의 역할은 다음과 같다.
* `baz` : 함수 `bar`를 호출
* `bar` : 함수 `foo`를 호출
* `foo` : `error` 발생

### 이미지로 이해하기
