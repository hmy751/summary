# Compile, Run time
---
타입스크립트는 컴파일 언어로 컴파일 단계를 거쳐 소스코드의 타입 검사를 한 후에 자바스크립트 파일로 변환한다.
컴파일 과정은 사람이 이해하기 쉬운 고수준 언어에서 컴퓨터가 이해할 수 있는 저수준 단계로 변환하는 과정을 의미한다.
타입스크립트 컴파일은 타입을 명시한 타입스크립트 파일을 변환하여 브라우저가 실행할 수 있는 자바스크립트 파일로 변환하는 컴파일 과정을 거치는데 고수준 언어에서 고수준 언어로 변경하여 트랜스파일 과정이라고도 부른다.

타입스크립트 컴파일러가 소스코드를 컴파일하는 과정은 
- 타입스크립트 소스코드를 타입스크립트 AST로 변환한다.
- 타입 검사기가 AST를 확인하여 타입을 확인한다.
- 타입스크립트 AST를 자바스크립트 소스로 변환한다.
- 