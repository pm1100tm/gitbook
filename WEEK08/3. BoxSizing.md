# BoxSizing

## box-sizing 설정

box-sizing은 CSS의 속성 중 하나로, 박스 모델(box model)에서 요소의 크기를 계산하는 방법을
지정한다.

기본적으로 CSS의 박스 모델은 요소의 크기를 결정할 때 요소의 콘텐츠,

- 안쪽 여백(padding)
- 테두리(border)
- 바깥 여백(margin)

을 고려한다.

box-sizing 속성은 이러한 크기 계산 방법을 변경할 수 있고, 다음 두 가지 값을 가질 수 있다.

### content-box

기본값으로, 요소의 크기를 콘텐츠의 크기에만 적용한다.

즉, 패딩(padding)과 테두리(border)는 요소의 크기에 추가된다.
이 경우에는 요소의 실제 크기는 콘텐츠 영역의 크기에만 영향을 받는다.

### border-box

요소의 크기를 콘텐츠, 안쪽 여백, 테두리를 포함한 크기로 적용한다.
따라서 요소의 크기에 패딩과 테두리가 포함된다.
이러한 설정은 레이아웃을 작성할 때 편리하며, 요소의 크기를 지정할 때 테두리와 안쪽 여백을 고려할 필요가
없어진다.

### 실습 with border-box

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="index.css" />
    <title>Document</title>
  </head>
  <body>
    <div class="div1">div1</div>
    <div class="div2">div2</div>
    <div class="div3">div3</div>
  </body>
</html>
```

```css
* {
  box-sizing: border-box; /* content-box 이 부분만 바꿔서 테스트 */
}

div {
  padding: 10px;
  margin: 10px;
}

.div1 {
  width: 200px;
  height: 40px;
  background-color: lightblue;
}

.div2 {
  width: 200px;
  height: 40px;
  background-color: lightcoral;
}

.div3 {
  width: 200px;
  height: 40px;
  background-color: lightcyan;
}
```

### content-box 를 적용한 모습

아래의 연두색으로 보이는 부분이 content 의 크기이다.

위 아래, 왼쪽 오른쪽으로 padding 을 10px 을 먹였기 때문에, 컨텐트의 width, height 값은
각각 220px 60px 이 된다.

content-box 는 이렇게 html 컨텐트를 중심으로 padding 값을 바깥으로 밀어내어 영역을 차지한다.

div 에 지정한 padding 은 안쪽으로 들어가며, margin 값은 컨텐트의 바깥으로 나오게 된다.

![border-box](/asset/content-box.png)

### border-box 를 적용한 모습

아래의 연두색으로 보이는 부분이 content 의 크기이다.

위 아래, 왼쪽 오른쪽으로 padding 을 10px 을 먹였음에도 불구하고, 컨텐트의 width, height 값은
각각 200px 40px 이 된다. 실제 컨텐트의 크기는 180px, 20px 이 된다.

이와 같이 border-box 는 padding 값이 컨텐트를 중심으로 안으로 들어오게 된다.

![border-box](/asset/border-box.png)

#### 결론

**content-box** 를 사용하면 컨텐트가 지정한 가로 세로의 값보다 더 크게 될 가능성이 있게 된다.

**border-box** 를 사용하는 것이 사이즈를 계산하여 레이아웃을 잡는데에 훨씬 수월하다.

border-box 는 padding 과 border 의 사이즈가 컨텐트 영역의 안으로 들어오게 된다.

content-box, border-box 에 상관없이 margin 의 사이즈는 항상 컨텐트의 바깥으로 사이즈를 잡는다.
