# 실행 컨텍스트

## 실행 컨텍스트 란?

실행 컨텍스트는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체이다. 전역 공간과, eval공간을 제외하면 함수를 실행해야지만 구성되며, 스택 자료구조 형태로 가장 최신 정보가 맨 위로 쌓여 나가는 형태다.

실행 컨텍스트는 소스코드의 실행과정에 관여한다.
코드의 평가 과정에서 변수, 함수 식별자등의 정보를 모아 실행 컨테스트를 생성하고 이 정보들을 토대로 코드를 실행해 나간다.
처음 실행하게 되면 전역 컨텍스트가 생성되며 전역 공간에 식별자 정보들을 모아 나간다.
생성된 컨텍스트는 콜 스택에 담기며 가장 위에 있는 스택은 자동으로 실행하게 된다.
컨텍스트 정보를 바탕으로 실행 해나가며 함수 호출부를 만나면 다시 평가 과정이 시작된다.
평가 후 해당 함수의 실행 컨텍스트를 생성 후 콜 스택에 담으며, 콜 스택에 새로 담긴 컨텍스트를 기준으로 다시 실행 과정이 시작된다.
이런 과정을 반복하다가 해당 컨텍스트의 실행이 모두 종료되면 스택에서 빠지고 다시 기존에 컨텍스트가 실행 기준이 되며 실행이 중단 되었던 부분부터 다시 실행해 나간다.

컨텍스트의 구성은 아래와 같이 3가지로 구성되어 있다.

- VariableEnvironment(스냅샷)
  - environmentRecord
  - outerEnvironmentReference
- LexicalEnvironment
  - environmentRecord
  - outerEnvironmentReference
- ThisBinding

## VariableEnvironment

VariableEnvironment는 실행 컨테스트를 생성할 때 만들어지며 스냅샷으로 유지 된다.
그리고 복사하여 LexicalEnvironment를 만들며 이후에는 스냅샷용도로 사용되며 실제로는 LexicalEnvironment가 이용된다.

## LexicalEnvironment

컨텍스트 내부의 식별자와 참조하는 외부 정보로 구성되어 있는 환경으로 environmentRecord와 outerEnvironmentReference로 구성되어 있다.

## environmentRecord, 호이스팅

environmentRecord에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다.
실행 컨텍스트 생성 과정에서 식별자 정보를 먼저 수집한기 때문에, 선언된 변수 이전에 실행된 코드에서 변수를 참조할 수 있게 되어 호이스팅 현상이 발생한다.
호이스팅은 선언된 변수가 이전에 실행될 코드 위에서 먼저 참조되어 마치 끌어올려져서 해석되는 현상을 말한다.

## outerEnvironmentReference, 스코프 스코프 체인

outerEnvironmentReference는 해당 컨텍스트의 함수가 선언되는 시점의 LexicalEnvironment를 담으며 이러한 이유로 해당 컨텍스트 뿐만 아니라 선언되는 시점에 있는 바깥의 컨텍스트의 식별자 정보를 참조할 수 있게 된다.
이러한 outerEnvironmentReference의 특성으로 스코프 체인 현상이 발생한다.
먼저 스코프는 식별자에 대한 유효범위를 말하며, 스코프 체인은 바깥의 컨텍스트의 식별자 정보를 참조할 수 잇는 특성으로 인해 스코프를 안에서 부터 바깥으로 차례로 검색해나가는 것을 말한다.

## ThisBinding

실행 컨텍스트의 thisBinding에는 this로 지정된 객체가 저장된다. 실행 컨텍스트 활성화 당시 this가 지정되지 않은 경우 전역 객체가 저장된다.

## 코드 평가 과정
