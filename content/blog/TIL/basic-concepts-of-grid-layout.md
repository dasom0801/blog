---
title: (TIL) Basic concepts of grid layout
date: 2020-05-18 20:05:16
category: til
draft: false
---

> https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Grid_Layout/Basic_concepts_of_grid_layout
>

### Grid Container

`display: grid` 또는 `diplay: inline-grid`를 선언하여 만든다. 해당 요소 밑에 있는 모든 자식 요소는 Grid Item이 된다.

### Grid Track

그리드의 행과 열. `grid-template-columns`나 `grid-tempalte-rows`로 트랙의 크기를 정의해주면, 자식 아이템의 그리드 셀 위에 배치된다. 트랙은 모든 종류의 길이 단위를 써서 정의할 수 있다.

**fr단위**

- 유연한 크기의 그리드 트랙을 생성하는 데 사용할 수 있는 단위

- 그리드 컨테이너에 남아 있는 사용 가능한 공간의 일정 비율을 나타낸다.

- fr 적용 예

  ```css
  /* 남아 있는 공간에 따라 확장 및 축소되는 같은 너비의 트랙 3개를 생성*/
  .wrapper {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
  }
  
  /* 사용 가능한 공간을 4개로 나누고 공간 두 개는 첫 번째 트랙에, 나머지 공간 한 부분씩은 다음 투 트랙에 각각 제공 */
  .wrapper {
      display: grid;
      grid-template-columns: 2fr 1fr 1fr;
  }
  
  /* 고정 크기와 비율 단위 혼합 사용 */
  .wrapper {
      display: grid;
      grid-template-columns: 500px 1fr 2fr;
  }
  ```

**repeat()**

+ 많은 트랙을 포함하는 커다란 그리드는 `repeat()` 를 사용하여 트랙의 전체 또는 일부분을 반복해서 나열할 수 있다.

  ```css
  /* grid-template-columns: 1fr 1fr 1fr; 를 repeat로 바꾸면 */
  .wrapper {
      display: grid;
      grid-template-columns: repeat(3, 1fr); 
  }
  ```

+ 트랙의 목록 중 일부분에만 사용 가능

  ```css
  /* 20픽셀 크기의 트랙을 생성하고 다음에 1fr 크기의 트랙을 6번 반복해서 채운 후 마지막에 20픽셀 트랙을 붙여서 그리드를 완성 */
  .wrapper {
      display: grid;
      grid-template-columns: 20px repeat(6, 1fr) 20px;
  }
  ```

+ 트랙의 반복 패턴 사용

  ```css
  /* 10개의 트랙으로 구성되며, 1fr 크기의 트랙 다음에 2fr 크기 트랙이 위치하고, 이 형태가 5회 반복 */
  .wrapper {
      display: grid;
      grid-template-columns: repeat(5, 1fr 2fr);
  }
  ```

**잠재적 그리고 명시적 그리드**

+ 명시적 그리드:  `grid-template-columns` 와 `grid-template-rows`로 직접 정의한 행과 열로 이루어진 그리드 

+ 명시적으로 정의된 그리드 밖에 무언가를 배치할 땐, 늘어난 콘텐츠 양 때문에 더 많은 그리드 트랙이 필요하고, 그리드는 잠재적 그리드에 새로운 행과 열을 만든다. 

+ 잠재적 그리드에서 생성된 트랙의 크기는  기본적으로 자동으로 정해지고, 트랙 내부의 내용물에 따라 크기가 조정된다. `grid-auto-rows` 및 `grid-auto-columns` 프로퍼티를 써서 지정해줄 수도 있다.

  ```css
  /* 잠재적 그리드에서 생성된 트랙의 높이가 반드시 200필셀이 되도록 grid rows를 사용함*/
  .wrapper {
     display: grid;
     grid-template-columns: repeat(3, 1fr);
     grid-auto-rows: 200px;
  }
  ```

**트랙 크기 조정과 minmax()**

+ 행이나 열의 크기를 정의할 때, 트랙의 최소 크기를 정해도 나중에 추가되는 콘텐츠에 맞게 늘어나도록 함

  ```css
  /* 자동으로 생성된 행의 높이를 최소 100픽셀이고 최댓값 auto, auto로 지정하면 크기는 콘텐츠의 크기를 사피게 됨. 가로 행에 있는 가장 높은 셀의 크기만큼 자동으로 늘어나서 부족한 공간을 메꾼다.*/
  .wrapper {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-auto-rows: minmax(100px, auto);
  }
  ```

### 그리드 라인

그리드 트랙을 정의하면 그리드 아이템을 배치할 때 쓸 수 있게 번호가 메겨진 라인을 자동으로 제공한다. 

**라인을 이용한 아이템 배치**

- `grid-column-start`, `grid-column-end`, `grid-row-start`, `grid-row-end` 프로퍼티로 배치.

    ```html
    <div class="wrapper">
      <div class="box1">One</div>
      <div class="box2">Two</div>
      <div class="box3">Three</div>
      <div class="box4">Four</div>
      <div class="box5">Five</div>
    </div>
    ```

    ```css
    .wrapper { 
        display: grid; 
        grid-template-columns: repeat(3, 1fr); 
        grid-auto-rows: 100px; 
    } 

    /* 세로 열 방향 1번 라인 ~ 세로 열 방향 4번 라인. 가로 행 라인 1번 ~ 3번*/
    .box1 { 
        grid-column-start: 1; 
        grid-column-end: 4; 
        grid-row-start: 1; 
        grid-row-end: 3; 
    }
    /* 세로 열 방향 1번 ~ 트랙 하나만. 가로 행 라인 3번 ~ 5번 (트랙 두 개)*/
    .box2 { 
        grid-column-start: 1; 
        grid-row-start: 3; 
        grid-row-end: 5; 
    }

    /* 나머지 아이템은 그리드 빈 자리에 자동으로 배치된다*/
    ```

### 그리드 셀

그리드에 있는 가장 작은 구성원. 테이블에 있는 셀과 비슷한 개념. 부모 요소에 그리드를 정의하면 자식 아이템은 지정된 그리드 셀에 각자 하나씩 배치됨.

### 그리드 영역

아이템은 행 또는 열 방향 어느 쪽으로든 하나 이상의 셀에 걸쳐있을 수 있으며 이렇게 해서 그리드 영역을 만든다. 그리드 영역은 직사각형이어야 함.

### 그리드 갭

`grid-column-gap` 및 `grid-row-gap` 프로퍼티를 지정해서 그리드 셀 사이의 여백을 지정할 수 있다. 이 공간에는 아무것도 배치할 수 없다.

### 중첩 그리드

그리드 아이템은 자기 자신이 그리드 컨테이너가 될 수도 있다. 중첩된 그리드는 상위 부모 요소의 그리드와 아무런 관계가 없다. 부모 요소의 grid-gap을 물려받지도 않고, 중첩된 그리드의 라인은 부모 요소의 그리드 라인과도 일렬로 정렬되지 않는다.

**서브 그리드**

- 부모 요소에 있는 그리드 트랙의 정의를 중첩된 그리드에도 적용해서 생성할 수 있도록 함
- 모든 브라우저에서 구현되지 않았고 나중에 표준이 변경될 가능성 있음
- 그리드 아이템에 `diplay: grid` 대신 `display: subgrid`를 써준다. 부모 요소의 그리드 트랙을 그대로 참고해서 아이템을 배치하기 때문에 트랙 정의는 필요없음.