# 데이터 타입
---
데이터 타입은 데이터의 종류를 말하며 크게 **2가지**로 **원시형(primitive)**과 **참조형(reference)**으로 나뉜다.

## 원시형(Primitive Type)
원시형 데이터는 아래와 같이 7가지가 존재하며, 값이 **불변한다는 특징**이 있다.

- number
- string
- undefined
- null
- boolean
- symbol
- bigint

### truthy, falsy
자바스크립트는 **boolean 문맥**에서 **자동으로 형 변환**을 사용한다.
falsy값은 boolean 문맥에서 **false로 평가**되는 값을 말하며 아래와 같이 8가지가 있다.

- false
- 0
- -0
- 0n
- ""
- null
- undefined
- NaN
truthy값은 boolean 문맥에서 **true로 평가**되는 값을 말하며 **falsy값을 제외한 모든 값**을 말한다.

### undefined, null
undefined, null 모두 **값이 없음을 나타내는** 데이터다.
**차이점은** undefined의 경우 개발자가 직접 명시할 수 도 있지만 **자바스크립트 엔진이** 데이터가 없는 경우 **자동적으로 부여**하는 경우가 있다.
null은 자바스크립트 엔진이 **자동적으로 부여할 수 없다.**

따라서 두 경우를 구분하기 위해 데이터가 없음을 직접 명시하려는 경우 null을 사용하는게 낫다.

### Symbol


## 참조형(Reference Type)
참조형 데이터는 객체, 배열, 함수, 정규 표현식등 **모든 객체 데이터**를 의미 하며 데이터가 **가변하는 특징**을 갖는다.