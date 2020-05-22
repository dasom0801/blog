---
title: (TIL) 코드스피츠 73 - 3
date: 2020-05-21 20:05:92
category: til
draft: false
---

### Interface

1. 인터페이스란 사양에 맞는 값과 연결된 속성키의 셋트
2. 어떤 오브젝트라도 인터페이스의 정의를 충족시킬 수 있다.
3. 하나의 오브젝트는 여러개의 인터페이스를 충족시킬 수 있다.

#### Interface test

1. test라는 키를 갖고

2. 값으로 문자열 인자를 1개 받아 불린 결과를 반환하는 함수가 온다


```javascript
// interface test를 만족하는 object
const object = {
	test(str) {
		return true;
	}
};

// interface test를 만족하는 class
const Test = class {
    test(str) {
        return true;
    }
};
const test = new Test();
```

class 정의에서 Interface의 요구사항을 만족시키면 class의 인스턴스도 그 요구사항을 만족시킨다.

### Iterator Interface

1. next라는 키를 갖고
2. 값으로 인자를 받지 않고 <u>InterfaceResultObject</u> 를 반환하는 함수가 온다.
3. <u>InterfaceResultObject</u>는 value와 done이라는 키를 갖고있다.
4. done은 계속 반복할 수 있을지에 따라 불린값을 반환한다. 계속 next를 부를지 말지 판정하는데 쓰인다. 

#### Iterator object

```javascript
const iterator = {
    next() {
        return {
            done: ture,
            value: 1
        }
    }
};
```

#### Iterator 사용하기

```javascript
{
    data: [1, 2, 3, 4],
    next() {
        return {
            done: this.data.length === 0,
            value: this.data.pop()
        }
    }
}
```

#### Iterator class

```javascript
const Iterator = class {
    constructor() {
        this.data = [1, 2, 3, 4];
    }
    next() {
        return {
        	done: this.data.length === 0,
            value: this.data.pop()
        }   
    }
}
```

### Iterable Interface

1. Symbol.iterator(내장된 symbol)라는 키를 갖고

2. 값으로 인자를 받지 않고 Iterator Object를 반환하는 함수가 온다.

   ```javascript
   const iterable = {
       [Symbol.iterator]() { // computed property name
           return {
               return new Iterator(); // 새롭게 반복 시킬 수 있는 iterator를 얻는다.
           }
       }
   }
   ```

   iterator만 있으면 한 번 반복한 다음 수명이 다해서 없어지기 때문에 원본은 그대로 둔 상태에서 계속 반복 가능한 객체를 얻기 위해서 iterable이 필요하다.

### while문으로 살펴보는 Iterator

```javascript
let arr = [1, 2, 3, 4];
while(arr.length > 0) { // 계속 반복할지 판단, iterator interface에서 done
    console.log(arr.pop()); // 반복시마다 처리할 것 iterator interface에서 value
}
```

iterator의 구조는 loop를 흉내내고 있다. while은 문 iterator는 값. 값은 메모리상에 저장하거나 다른 곳에 할당할 수 있지만, 문은 실행하고 나면 사라진다. <u>문으로 loop를 돌리면 재현이 불가능하고 원할 때 다시 실행할 수 없다.</u> 문으로 하는 건 대부분 휘발성. 문으로 하는 일을 값으로 바꾸지 않으면 휘발 된다. <u>값은 통제할 수 있다.</u> next를 호출하지 않으면 그 다음 loop가 발생하지 않는다.

문은 엔진이 알아서 실행하지만, 값은 호출해야만 실행된다.


Iterator는 반복자체를 하지 않지만 외부에서 반복을 하려고할 때 반복에 필요한 조건과 실행을 미리 준비해둔 객체

 => 반복 행위와 반복을 위한 준비를 분리

 => 미리 반복에 대한 준비를 해두고 필요할 때 필요한만큼 반복 반복을 재현할 수 있음

#### 지연

원래는 한번에 다 실행되어야 하는 것을 실행하는 중에 실행을 미루거나 점프시킬 수 있는 행위

**연산 지연**

```javascript
const a = b || c;
```

b가 참일 때는 c가 실행되지 않고, a에는 b가 들어간다. b가 false일 때 c를 실행한다.

**호출 지연**

코드를 함수에 가두면 함수를 호출 할 때가지 지연. 원할 때 실행시킨다.

#### 은닉과 캡슐화

**은닉**은 숨기는 것, 데이터가 안 보이는 것.

**캡슐화**는 내부에서 실제로 일어나는 것을 숨기는 것. 외부에는 내가 보여주고 싶은 행위와 내가 보여주고 싶은 결과를 보여주되 내부에서 발생하고 있는 일은 보여주지 않는다. 내부에서 수정사항이 있어도 외부에서 사용하는 사람에게 영향을 주지 않는다 => 격리!

### ES6 + Loop

#### 사용자 반복 처리기

직접 iterator 반복처리기를 구현

``` javascript
const loop = (iter, f) => {
	// iterable이라면 iterator를 얻음
    if(typeof iter[Symbol.iterator] == 'function') {
        iter = iter[Symbol.iterator]();
    }
  	// iterator Object가 아니라면 건너뜀
	if(typeof iter.next != 'function') return;
	while(true) {
        const v = iter.next() // iterator result object
        if(v.done) return ; // 종료 처리
        f(v.value) // 현재 값을 전달함
    }
};

const iter = {
    [Symbol.iterator]() {return this;},
    arr: [1, 2, 3, 4],
    next() {
        return {
            done: this.arr.length == 0,
            value: this.arr.pop()
        };
    }
};

loop(iter, console.log)
// 4
// 3
// 2
// 1
```

#### Array Destructuring

```javascript
const [a, ... b] = iter;
console.log(a, b); // 4, [3, 2, 1]
const [a, ...b] = iter[Symbol.iterator]();
console.log(a, b); // 4, [3, 2, 1]
const [a, ...b] = [1, 2, 3, 4];
console.log(a, b); // 1, [2, 3, 4]
const [a, ...b] = "ABCD";
console.log(a, b); // A, [B, C, D]
```

배열과 string도 iterator가 있기 때문에  destructuring이 가능하다. 

#### Spread operator(펼치기)

```javascript
const a = [...iter];
console.log(a) //[4, 3, 2, 1]
```

#### Rest Parameter(나머지 인자)

```javascript
const test = (...arg) => console.log(arg); // 여기서 ...arg는 rest parameter [4, 3, 2, 1]
test(...iter); // 여기서 ...iter는 spread operator, 4 3 2 1 
```

spread와 rest를 구분할 때 사용하는 장소를 보기. rest는 함수 선언시 사용된다. 여러개의 배열이 아닌 요소들을 모아서 배열로 만들어준다. spead는 묶었던 걸 펼쳐준다. 

#### For of

```javascript
for(const v of iter) {
    console.log(v)
}
```

얼마만큼 loop를 돌릴지를 통제하는 것은 for가 아닌 이터러블, 이터레이터

#### 제곱을 요소로 갖는 가상 컬렉션

```javascript
const N2 = class {
    constructor(max) {
        this.max = max;
    }
    
    [Symbol.iterator]() {
        let cursor = 0, max = this.max;
        return {
            done: false,
            next() {
                if(cursor > max) {
                    this.done = true;
                } else {
                    this.value = cursor * cursor;
                    cursor++;
                }
                return this;
            }
        };
    }
};

console.log([...new N2(5)]); // [0, 1, 4, 9, 16, 25]

for(const v of new N2(5)) {
    console.log(v); // 0 1 4 9 16 25
}
```

#### Generator

iterator를 만들어 내는 것은 iterable. iterable은 Symbol.iterator를 호출하면 그 반환값으로 iterator를 만든다. 

generator를 호출한 결과도 iterator!

```javascript
const generator = function*(max) {
	let cursor = 0;
	while(cursor < max) { // iterator의 next 역할
		yield cursor * cursor;
		cursor++;
	}
};

console.log([...generator('5')]);
for(const v of generator(5)) {
	console.log(v);
}
```

