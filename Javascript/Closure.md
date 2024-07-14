# 클로저

## 클로저 정의

클로저란 중첩된 함수에서 상위 함수의 변수를 참조하는 내부함수가 있고 이 내부함수가 외부로 전달되어질 때, 외부 함수의 실행 컨텍스트가 종료된 이후에도 변수를 계속 참조하는 현상을 말한다.

```js
var outer = function () {
  var a = 1;

  var inner = function () {
    return ++a;
  };

  return inner;
};

var outer1 = outer();

outer1(); // 2
outer1(); // 3
```

클로저가 발생하는 이유는 함수의 실행 컨텍스트가 종료돼 콜 스택에서 제거 되어도 변수가 계속 참조되어지고 있는 상황이면 가비지 컬렉터 해당 렉시컬 환경을 수거하지 않기 때문이다.

## 클로저 활용

클로저는 함수에서 변수를 참조하여 유지하며 사용할 때 유용하다. 전역 변수 같이 해당 함수의 상위 스코프에 변수를 유지할 수 있지만, 이렇게 하면 변수가 해당 함수 뿐만 아니라 다른 공간에도 노출이 되어 불변성이 보장되지 않을 수 있다.
클로저를 이용하면 해당 함수 스코프내에 지역 변수로 남아, 해당 변수를 유지하며 사용하는 것 뿐만 아니라 변수의 안정성도 높일 수 있다.

- 모듈 패턴

```js
var Counter = (function () {
  var num = 0;

  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    },
  };
})();

Counter.num; // undefined;

Counter.increase(); // 1
Counter.increase(); // 2
Counter.decrease(); // 1
```

클로저를 통해서 num이라는 변수는 보호 하면서, 값은 유지하면서 활용이 가능하다.
