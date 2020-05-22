---
title: (TIL) Factory Function in Javascript
date: 2020-05-11 22:05:61
category: til
draft: false
---

> 참고: https://www.youtube.com/watch?v=ImwrezYhw4w

Factory Function은 object를 만드는 게 아니라 object를 반환하는 함수이다. 여러 경우에서 class를 대신해 factory를 사용할 수 있다.

**Class**

```javascript
class Dog {
    contructor() {
        this.sound = 'woof';
    }
    talk() {
        console.log(this.sound);
    }
}
const sniffles = new Dog();
sniffles.talk(); // Outputs: 'woof'
```

class는 문제 없이 동작한다. 그럼 button의 click 이벤트에 연결하면 어떻게 될까?🤔

```javascript
const myButton = document.getElementById('myButton');
myButton.addEventListener('click', sniffles.talk);
```

버튼을 클릭하면 'woof'가 아닌 undefined가 찍힌다. 이 때 this는 이벤트가 연결된 DOM element이기 때문이다. 의도대로 동작하게 하려면 this를 바꿔주어야 한다. 

```javascript
// bind로 this 넘겨주기
const myButton = document.getElementById('myButton');
myButton.addEventListener('click', sniffles.talk.bind(sniffles));
```

```javascript
// arrow function으로 감싸주기
const myButton = document.getElementById('myButton');
myButton.addEventListener('click', () => sniffles.talk());
```

**Factory Function**

Class를 Factory Function으로 바꿔보자!

```javascript
const dog = () => {
    const sound = 'woof';
    return {
        talk: () => console.log(sound)
    };
}
const sniffles = dog();
sniffles.talk(); //Outputs: 'woof'
```

this 키워드를 사용하지 않았기때문에 this가 바뀌든 말든 콘솔에는 sound의 값이 찍히게 된다.

