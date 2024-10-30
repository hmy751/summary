# use strict
---
use strict는 자바스크립트에서 엄격 모드(stict mode)를 활성화 하기위해 사용하는 명령어로, 사전에 오류를 방지하기 위해 사용한다.

파일 전체 또는 함수 내부에서 명령어를 선언하여 사용한다. 
```js
"use strict";

function msFunction() {
	"use strict";
}
```

## 제한 기능

- 변수 선언 강제
  var, let, const 없이 변수를 선언하면 오류가 발생한다.
	```js
	"use strict";
	x = 10; // ReferenceError: x is not defined
```

- this의 암시적 전역 바인딩 금지
  엄격 모드에선느 함수 내에서 this를 사용할 때, 전역 객체가 아니라 undefined로 설정된다.
  ```js
  "use strict"; 
  
  function myFunc() { 
    console.log(this); // undefined (엄격 모드에서는 전역 객체에 바인딩되지 않음) 
  } 
  
  myFunc();
```

- 예약어의 변수 사용 금지
  yield, public 등 변수나 함수 이름으로 사용할 수 없다.

- 중복된 매개변수 금지
  함수내 동일한 이름의 매개변수를 사용할수 없다.
  ```js
  "use strict"; function sum(a, a) { // SyntaxError: Duplicate parameter name not allowed in this context return a + a; }
```

- 암시적 오류 처리
  일반 모드에서 사용되는 오류들이 표시된다.
  ```js
  "use strict"; 
  delete Object.prototype; // TypeError: Cannot delete property 'prototype' of function Object()
```

- 읽기 전용 속성 수정 금지
  객체의 읽기 전용 속성에 값을 할당하려고 하면 오류가 발생한다.
  ```js
  "use strict";
  const obj = Object.freeze({ name: "Alice" });
  obj.name = "Bob"; // TypeError: Cannot assign to read only property 'name'

```

## ES6 모듈과의 관계

ES6모듈 또는 클래스에서는 기본적으로 엄격 모드로 동작하여 "use strict"를 선언하지 않아도 엄격 모드 규칙이 적용된다.