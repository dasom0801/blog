---
title: (TIL) 코드스피츠 73 - 2
date: 2020-05-19 21:05:00
category: til
draft: false
---

**goto**

코드가 나열되어 있을 때 특정한 위치로 점프 시킬 수 있다. 고전적인 goto에서는 line number로. 현대는 label의 이름으로 점프를 시킨다.

**label**

```javascript
for(const i of [1, 2, 3, 4]) {
    if ( i === 3 ) break;
    console.log(i); // 1, 2 출력
}
```

```javascript
abc: {
    console.log('a');
    break; // Uncaught SyntaxError: Illegal break statement 이터레이션이 아니면 자동으로 label을 만들 수 없기 때문에 에러가 발생한다. label 이름을 생략할 수 없다. break abc; 라고 적어주어야 한다.
    console.log('b');
}
console.log(b);
```

**변수**

일반적으로 변수는 스코프와 라이프 사이클을 가지고 있다.

+ 라이프 사이클: 변수는 영원히 존재하지 않고 특정 시점에 태어났다가 사라진다
+ 접근제어: 어떤 변수에는 누가 접근할 수 있고 누구는 접근 못 함 => **scope**

javascript에서는 어떤 변수가 태어나서 죽는데까지 살아있는 범위를 나타낼 때 scope라는 표현을 사용한다.

**자유변수와 클로저**

자기를 기준으로 자기가 원래 알 수 있는 권한이 없는 변수를 **자유변수**라고 한다.

원래 함수가 알 수 있는 변수는 지역 변수와 인자밖에 없다. 지역변수와 인자가 아닌 것은 자유변수이다.

클래스 메소드가 알 수 있는 건 this로 접근할 수 있는 멤버 변수, 함수의 인자 & 지역변수. 이게 아니면 메소드 입장에서는 자유변수이다.

자유변수는 내가 선언한 것도 아니고 내가 소유한 것도 아닌데 사용할 수 있다. 쓸 수 있는 자유변수들이 실제로 사용될 수 있는 공간(block)을 **클로저**라고 한다.

```javascript
let a = 3;
const f = () => {
    console.log(a); // 함수 body는 a라는 자유변수의 클로저가 된다.
}
```

**shadowing**

자유변수와 자유변수가 아닌 변수 사이에 이름이 같으면 일반적으로 언어들은 자기 변수를 우선시하는 경향이 있다. 자유변수의 우선순위는 낮고, 가까울 수록 우선 순위가 높아진다.

```javascript
let a = 3;
const f = () => {
    let a = 5;
    console.log(a); // 5
}
```

**switch**

```javascript
switch(3) {
    case 3: break;
}
```

switch의 `{}` 는 중문이 아니라 switch label block이다. switch문의 break는 익명 label을 만든다. 

```javascript
a: 
switch(3) {
    case 3: break a; // label을 명시해주는 것과 명시하지 않는 것은 동일하다.
}
```

 