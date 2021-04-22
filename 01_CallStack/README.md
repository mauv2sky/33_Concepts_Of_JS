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

**1. 함수 baz 호출**

<img width="400" alt="1" src="https://user-images.githubusercontent.com/74294301/115711775-ea91e900-a3ae-11eb-8aa9-f3a89239ad5d.PNG">

함수 `baz`가 호출되고 콜스택에 함수 `baz`가 `push`된다.

**2. 함수 bar 호출**

<img width="400" alt="2" src="https://user-images.githubusercontent.com/74294301/115711778-eb2a7f80-a3ae-11eb-92f5-bee1efd15528.PNG">

함수 `baz`가 함수 `bar`를 호출하면서 `bar`이 `push`된다.

**3. 함수 foo 호출**

<img width="400" alt="3" src="https://user-images.githubusercontent.com/74294301/115711780-ebc31600-a3ae-11eb-9d00-2a521699b8c2.PNG">

함수 `bar`이 함수 `foo`를 호출하면서 `foo`가 `push`된다.

**4. 최종 콜스택**

<img width="400" alt="8" src="https://user-images.githubusercontent.com/74294301/115712939-617bb180-a3b0-11eb-90b6-d29ac9c7a127.PNG">

모든 함수들이 스택에 `push`되고 아무 문제가 없으면 스택의 가장 위 스택프레임부터 차례로 `pop`된다.<br>
그 후 스택에 아무 것도 있지 않은 상태가 되면 프로그램이 종료된다.<br>
위의 예시에서는 스택 상단의 `foo` 함수에서 error가 발생해 프로그램이 비정상 종료된다.

### 콘솔로 이해하기
위의 자바스크립트 소스코드를 직접 실행해 보았다.
<img width="650" alt="6" src="https://user-images.githubusercontent.com/74294301/115716922-abff2d00-a3b4-11eb-9802-20c240d9869b.PNG">

각 `baz`, `bar`, `foo` 함수의 로그가 호출한 순서대로 찍혀있는 것을 확인할 수 있다.<br>
`foo` 함수에서 에러가 발생한 이후의 로그를 보면 콜스택을 확인할 수 있다.

<img width="800" alt="5" src="https://user-images.githubusercontent.com/74294301/115716577-46ab3c00-a3b4-11eb-8678-5f5944164f9b.PNG">

최종 콜스택 이미지 그대로 로그가 `foo`, `bar`, `baz` 순서로 쌓여있는 것을 확인할 수 있다.

### 스택 오버플로우
스택 오버플로우는 이름 그대로 스택의 용량을 초과했을 때 발생하는 에러다.<br>
이 에러는 생각보다 쉽게 일어날 수 있는데 특히 재귀호출에서 그렇다.<br>
```javascript
function overflow(){
    overflow();
}

overflow();
```
`overflow` 함수는 어떤 값을 반환하지 않은 채 재귀호출을 반복하고 있다.<br>

<img width="700" alt="8" src="https://user-images.githubusercontent.com/74294301/115721965-7c9eef00-a3b9-11eb-8020-08bb5634beca.PNG">

**Maximum call stack size exceeded** 콜스택의 사이즈를 초과했다는 에러를 볼 수 있다.

<img width="500" alt="7" src="https://user-images.githubusercontent.com/74294301/115721305-e36fd880-a3b8-11eb-8ac3-273e2cb9dac7.PNG">

스택 오버플로우가 발생하면 더이상 함수를 호출하지 못하고 프로그램이 비정상 종료된다.

