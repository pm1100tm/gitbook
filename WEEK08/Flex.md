# Flex

## Flex Box

- Flexbox는 모던 웹을 위하여 제안된 layout 방식이다.
- Flexbox 레이아웃은 flex-container(부모요소) 와 flex-item(자식 요소들)로 구성된다.
- Flexbox를 사용하기 위해서 HTML 부모 요소의 display 속성에 flex를 지정한다.
- 부모 요소가 inline 요소인 경우 inline-flex을 지정한다.
- Flex 는 flex-container 속성과 flex-item 속성으로 나뉜다.

## flex container 에 적용하는 속성

- display: flex
- flex-direction
- flex-wrap
- flex-flow
- jutify-content
- align-items
- align-content

### display: flex

컨테이너를 플랙스 컨테이너로 만든다.

![flex-container](/asset/flex/flex-container.png)

### flex-direction

플랙스 컨테이너의 배치 축을 결정한다.

```css
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

![flex-direction](/asset/flex/flex-direction.png)

### flex-wrap

플랙스 컨테이너 안의 아이템의 줄바꿈을 지정한다. 즉, 컨테이너의 너비보다, item들 너비의 합이 클 경우,
아이템들이 줄바꿈 되는 것을 지정한다. nowrap 에 overflow: auto 를 지정하면 가로로 스크롤이 생긴다.
컨테이너를 넘치지 않게 된다.

```css
.container {
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

![flex-wrap](/asset/flex/flex-wrap.png)

### flex-flow

flex-direction 과 flex-wrap 속성을 한 번에 지정한다.

```css
.container {
  flex-flow: row | column || nowrap | wrap;
}
```

### justify-content

플랙스 컨테이너의 주축 방향으로 플랙스 아이템들의 배치를 결정한다.

```css
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around
    | space-evenly | start | end | left | right... + safe | unsafe;
}
```

![justify-content](/asset/flex/justify-content.png)

### align-items

플랙스 컨테이너의 주축의 반대에서, 플랙스 아이템들의 정렬을 지정한다.

```css
.container {
  align-items: stretch | flex-start | flex-end | center | baseline | first
    baseline | last baseline | start | end | self-start | self-end +... safe |
    unsafe;
}
```

![justify-content](/asset/flex/align-items.png)

### align-content

wrap이 적용되어 multiline 인 경우에만 적용되며, 한 줄만 있는 상태에서는 효과를 보이지 않는다.

```css
.container {
  align-content: flex-start | flex-end | center | space-between | space-around |
    space-evenly | stretch | start | end | baseline | first baseline | last
    baseline +... safe | unsafe;
}
```

![justify-content](/asset/flex/align-content.png)

### gap

플랙스 아이템들 사이의 간격을 명시적으로 제어한다. 바깥쪽 가장자리에 있지 않은 항목 사이에만
간격을 적용한다. gap 은 Grid나 다중 열 레이아웃에서도 작동한다.

```css
.container {
  display: flex;
  ...
  gap: 10px;
  gap: 10px 20px; /* row-gap column gap */
  row-gap: 10px;
  column-gap: 20px;
}
```

![justify-content](/asset/flex/gap.png)

---

## flex item 에 적용하는 속성

float, clear, vertical-align 속성은 flex item에 영향을 주지 않는다.

- order
- flex-grow
- flex-shrink
- flex-basis
- align-self

### order

아이템의 배치 순서를 지정하는데, 배치의 순서는 컨테이너에 추가된 순서이다. 정수 값(음수 가능)으로
지정하며 기본 값은 0이다.

![justify-content](/asset/flex/order.png)

### flex-grow

아이템의 너비에 대한 확대 인자를 지정한다. 양의 정수 값을 사용하며 기본 값은 0이다.
모든 아이템이 동일한 flex-grow 값이라면 동일한 너비를 갖는다. (flex-grow: 1)

### flex-shrink

아이템의 너비에 대한 축소 인자를 지정한다. 기본 값은 1이고 음수 값은 올 수 없다.
0을 지정하면 축소가 해제되어 원래의 너비를 유지한다. 컨테이너가 작아질 때 작동한다.

### flex-basis

아이템의 너비 기본 값을 px 또는 %로 지정한다. 기본 값은 auto 이다.

![justify-content](/asset/flex/grow-shrink-basis.png)

### flex

flex-grow, flex-shrink, flex-basis 속성을 동시에 지정한다. 기본 값은 0 1 이다.

### align-self

아이템 개별적으로 align-items 속성을 적용한다. 기본 값은 auto 이다.

- auto, flex-start, flex-end, center, baseline, stretch

![justify-content](/asset/flex/align-self.png)

## Flex Box Image

![FlexBox](https://css-tricks.com/wp-content/uploads/2022/02/css-flexbox-poster.png)

## 🏓 Tricks

### holly grail layout 하나의 방법

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="index2.css" />
    <title>Document</title>
  </head>
  <body>
    <div class="wrapper">
      <header class="header">Header</header>
      <article class="main">
        <p>
          Pellentesque habitant morbi tristique senectus et netus et malesuada
          fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae,
          ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam
          egestas semper. Aenean ultricies mi vitae est. Mauris placerat
          eleifend leo.
        </p>
      </article>
      <aside class="aside aside-1">Aside 1</aside>
      <aside class="aside aside-2">Aside 2</aside>
      <footer class="footer">Footer</footer>
    </div>
  </body>
</html>
```

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  padding: 2em;
}

.header {
  background: tomato;
}

.footer {
  background: lightgreen;
}

.main {
  text-align: left;
  background: deepskyblue;
}

.aside-1 {
  background: gold;
}

.aside-2 {
  background: hotpink;
}

.wrapper {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  font-weight: bold;
  text-align: center;
}

.wrapper > * {
  padding: 10px;
  flex: 1 100%;
}

@media all and (min-width: 600px) {
  .aside {
    flex-grow: 1;
    flex-shrink: 0;
    flex-basis: 0;
  }
}

@media all and (min-width: 800px) {
  .main {
    flex: 3 0px;
  }
  .aside-1 {
    order: 1;
  }
  .main {
    order: 2;
  }
  .aside-2 {
    order: 3;
  }
  .footer {
    order: 4;
  }
}
```

![holly-grail-layout](/asset/flex/holly-grail-layout.png)

---

ETC

- [using-flexbox-and-text-ellipsis-together](https://css-tricks.com/using-flexbox-and-text-ellipsis-together/)
- [flexbox-truncated-text](https://css-tricks.com/flexbox-truncated-text/)
- [designing-a-product-page-layout-with-flexbox/](https://css-tricks.com/designing-a-product-page-layout-with-flexbox/)
- [flexbox-and-absolute-positioning](https://chenhuijing.com/blog/flexbox-and-absolute-positioning/#%F0%9F%96%8A)

### Flexbox Bugs

[flexbugs](https://github.com/philipwalton/flexbugs)

### More Information

[digitalocean-cheatsheets-flexbox](https://www.digitalocean.com/community/cheatsheets/css-flexbox?utm_medium=content_acq&utm_source=css-tricks&utm_campaign=&utm_content=awareness_bestsellers)

## 참고

[a-guide-to-flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
[poiemaweb.com/css3-flexbox](https://poiemaweb.com/css3-flexbox)
[studiomeal.com/archives/197](https://studiomeal.com/archives/197)
