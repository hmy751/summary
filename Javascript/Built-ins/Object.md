# Object.is()

^ef3ef9
두 개의 인자를 비교하여 같은 값인지 판별하는 메서드이다.
`===` 연산자와 같지만 0, -0의 비교를 false로 처리하고, 서로 다른 NaN은 true 처리하여 좀 더 개발자가 기대하는 방식으로 비교처리를 한다.

Object.is는 `===`연산자의 약점을 보완하고자 하며 해결되지 않는 예외 케이스 0, -0 NaN을 다르게 처리한다.