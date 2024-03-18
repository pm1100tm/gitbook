# Grid

## Grid 에 대해서 알아보자

그리드의 개념을 살표보기 전에 알아야 할 몇 가지 용어가 있다.

## Grid Container

- 그리드가 적용되는 요소이다.
- 모든 그리드 항목의 직접적인 부모이다.
- 이 예제에서 컨테이너는 그리드 컨테이너이다.

```html
<div class="container">
  <div class="item item-1"></div>
  <div class="item item-2"></div>
  <div class="item item-3"></div>
</div>
```

## Grid Item

- 그리드 컨테이너의 자식(즉, 직계 자손)이다.
- 여기서 항목 요소는 그리드 항목이지만 하위 항목은 그렇지 않다.

```html
<div class="container">
  <div class="item"></div>
  <div class="item">
    <p class="sub-item"></p>
  </div>
  <div class="item"></div>
</div>
```

## Grid line

- 격자의 구조를 구성하는 구분선

![grid-line](/asset/grid/grid-line.png)

## Grid Cell

- 인접한 두 행과 인접한 두 열 격자선 사이의 공간

![grid-line](/asset/grid/grid-cell.png)

## Grid Track

- 인접한 두 그리드 선 사이의 공간.
- 그리드의 열 또는 행으로 생각할 수 이다.
- 다음은 두 번째 행과 세 번째 행 그리드 선 사이의 그리드 트랙이다.

![grid-line](/asset/grid/grid-track.png)

## Grid Area

- 4개의 격자 선으로 둘러싸인 전체 공간이다.
- 그리드 영역은 그리드 셀의 수에 관계없이 구성할 수 있다.
- 다음은 행 그리드 라인 1과 3, 열 그리드 라인 1과 3 사이의 그리드 영역이다.

![grid-line](/asset/grid/grid-area.png)

## 그리드 컨테이너에서 사용하는 CSS 프로퍼티

### display

```css
.container {
  display: grid | inline-grid;
}
```

### grid-template-columns, grid-template-rows

- 공백으로 구분된 값 목록으로 그리드의 열과 행을 정의한다.
- 값은 트랙 크기를 나타내며, 값 사이의 공백은 그리드 선을 나타낸다.
- 그리드 선에는 양수가 자동으로 할당되며, 맨 마지막 행의 경우 -1이 대체된다.

```css
.container {
  grid-template-columns: ... ...;
  /* e.g.
      1fr 1fr
      minmax(10px, 1fr) 3fr
      repeat(5, 1fr)
      50px auto 100px 1fr
  */
  grid-template-rows: ... ...;
  /* e.g.
      min-content 1fr min-content
      100px 1fr max-content
  */
}
```

![grid-number](/asset/grid/grid-number.png)

### grid-template-areas

- 그리드 영역 속성으로 지정된 그리드 영역의 이름을 참조하여 그리드 템플릿을 정의한다.
- 그리드 영역의 이름을 반복하면 콘텐츠가 해당 셀에 걸쳐 표시된다.
- 마침표는 빈 셀을 나타냅니다.
- 구문 자체는 그리드 구조의 시각화를 제공합니다.
- 명시한 이름으로 item 을 매칭시킨다.

```css
.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

.container {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: auto;
  grid-template-areas:
    'header header header header'
    'main main . sidebar'
    'footer footer footer footer';
}
```

### grid-template

- grid-template-rows, grid-template-column 를 한번에 선언한다.
- 이 속성은 grid-auto-columns, grid-auto-rows, and grid-auto-flow 을 재설정하지
  않음으로 grid 속성을 쓰기를 권장한다.

```css
.container {
  grid-template: none | <grid-template-rows> / <grid-template-columns>;
}
```

### column-gap, row-gap, grid-column-gap, grid-row-gap

- grid-column-gap, grid-row-gap 은 예전에 사용되던 프로퍼티이다.
- column-gap, row-gap 사용을 권장한다.

```css
.container {
  /* standard */
  column-gap: <line-size>;
  row-gap: <line-size>;

  /* old */
  grid-column-gap: <line-size>;
  grid-row-gap: <line-size>;
}
```

![gap](/asset/grid/gap-1.png)

### gap, grid-gap

- row-gap 과 column-gap 을 한번에 선언한다.
- row-gap 이 지정되지 않는다면, column-gap 과 동일한 값이 평가된다.

```css
.container {
  /* standard */
  gap: <grid-row-gap> <grid-column-gap>;

  /* old */
  grid-gap: <grid-row-gap> <grid-column-gap>;
}
```

### justify-items

- 인라인(행) 축을 따라 그리드 항목을 정렬한다.
- 값은 컨테이너 내부의 모든 그리드 항목에 적용된다.

```css
.container {
  justify-items: start | end | center | stretch;
}
```

![start](/asset/grid/justify-start.png)
![end](/asset/grid/justify-end.png)
![center](/asset/grid/justify-center.png)
![stretch](/asset/grid/justify-stretch.png)

### align-items

- 열 축을 따라서 그리드 항목을 정렬한다.
- justify-items 와 반대

```css
.container {
  align-items: start | end | center | stretch;
}
```

![start](/asset/grid/align-items-start.png)
![end](/asset/grid/align-items-end.png)
![center](/asset/grid/align-items-center.png)
![stretch](/asset/grid/align-items-stretch.png)

### place-items

- align-items, justify-items 를 동시에 선언한다.

```css
.center {
  display: grid;
  place-items: center;
}
```

### justify-content

일단 SKIP

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between
    | space-evenly;
}
```

### align-content

일단 SKIP

```css
.container {
  align-content: start | end | center | stretch | space-around | space-between |
    space-evenly;
}
```

### place-content

일단 SKIP

```css

```

### grid-auto-columns, grid-auto-rows

일단 SKIP

```css
.container {
  grid-template-columns: 60px 60px;
  grid-template-rows: 90px 90px;
}

.item-a {
  grid-column: 1 / 2;
  grid-row: 2 / 3;
}
.item-b {
  grid-column: 5 / 6;
  grid-row: 2 / 3;
}
```

![auto-columns-and-rows](/asset/grid/auto-column.png)

### grid-auto-flow

일단 SKIP

```css

```

### grid

일단 SKIP

```css

```

---

## 그리드 아이템에서 사용하는 CSS 프로퍼티

### grid-column-start, grid-column-end, grid-row-start, grid-row-end

- 특정 그리드 선을 참조하여 그리드 내에서 그리드 항목의 위치를 결정합니다

```css
.item-a {
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start;
  grid-row-end: 3;
}
```

![grid-column-start1](/asset/grid/grid-column-start1.png)

```css
.item-b {
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2;
  grid-row-end: span 2;
}
```

![grid-column-start1](/asset/grid/grid-column-start2.png)

### grid-column, grid-row

- grid-column-start + grid-column-end, and grid-row-start + grid-row-end

```css
.item-c {
  grid-column: 3 / span 2;
  grid-row: 3 / 4;
}
```

### grid-area

일단 SKIP

```css

```

### justify-self

```css
.item {
  justify-self: start | end | center | stretch;
}
```

### align-self

```css
.item {
  align-self: start | end | center | stretch;
}
```

### place-self

align-self 와 justify-self 를 한번에 선언한다.

```css
.item-a {
  place-self: center;
}
```

## fr

그리드에서 1fr과 같은 분수 단위를 많이 사용하는데, 분수 단위는 기본적으로 "남은 공간의 일부"를 의미한다.

따라서 아래의 정의는 25% 75%를 의미한다. 단, 이러한 백분율 값은 분수 단위보다 훨씬 더 명확하다.

```css
grid-template-columns: 1fr 3fr;
```

## sizing keyword

- px, rem, %
- min-content: 콘텐츠의 최소 크기
- max-content: 콘텐츠의 최대 크기
- auto: fr 과 유사하지만, 남은 공간을 할당할 때 fr 단위와의 우선순위에서 밀린다.

## sizing function

- fit-content(): 사용 가능한 공간을 사용. 최소 콘텐츠보다 작거나 최대 콘텐츠 보다 크지 않음.
- minmax(): 최소 값과 최대 값을 설정. 상대 단위와 함께 사용할 때 유용함.
- min(): 최소 값 설정
- max(): 최대 값 설정
- repeat(): repeat 함수는 auto-fill, auto-fit 함수와 함께 사용하면 더욱 유용하다.

```css
grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;

/* easier: */
grid-template-columns: repeat(8, 1fr);

/* especially when: */
grid-template-columns: repeat(8, minmax(10px, 1fr));

grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
```

## subgrid

```css
.parent-grid {
  display: grid;
  grid-template-columns: repeat(9, 1fr);
}
.grid-item {
  grid-column: 2 / 7;

  display: grid;
  grid-template-columns: subgrid;
}
.child-of-grid-item {
  /* gets to participate on parent grid! */
  grid-column: 3 / 6;
}
```

## 참조

- [complete-guide-grid](https://css-tricks.com/snippets/css/complete-guide-grid/#aa-grid-item)
