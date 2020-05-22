---
title: (TIL) Not Bitwise Operator
date: 2020-05-20 21:05:65
category: til
draft: false
---

Not 비트 연산자는 기본적으로 비트를 반전시킨다.

  | a    | ~a   |
  | ---- | ---- |
  | 0    | 1    |
  | 1    | 0    |

정수에 사용하면 -(N + 1) 로 변환된다.

  ```javascript
  ~2 === -3;
  ~1 === -2;
  ~0 === -1;
  ~-1 === 0;
  ```

이중 Not 비트 연산자는 부동소수점 숫자를 정수로 변환하는데 사용한다. 양수는 `Math.floor()`와 동일, 음수는 `Math.ceil()`과 동일하다.

  ```javascript
  ~~2.4 === Math.floor(2.4);
  ~~3.9 === Math.floor(3.9);
  ~~-2.4 === Math.ceil(-2.4);
  ```

  