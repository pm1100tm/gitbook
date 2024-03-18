# Tips

---

## margin 과 padding 의 차이

### margin

- border 의 바깥쪽의 영역을 차지한다.
- 주변 요소와의 여백(거리)를 두기 위한 영역이다.
- 음수 값이 허용된다.
- auto 값이 허용된다.

### padding

- object 내의 내부 여백이다.
- content 와 border 사이의 여백을 나타내는 영역이다.
- padding 도 content 의 영역이라고 볼 수 있다.
- 음수와 auto 값이 허용되지 않는다.

---

## display - block, inline, inline-block

### block

- 새로로 나열되며, 기본적으로 width 의 남은 여백을 전부 차지한다.
- 따라서, width 100% 를 지정할 필요가 없다.

### inline

- 옆으로 나열되며, 크기 조정을 할 수 없다. 크기는 content 에 맞게 자동으로 적용된다.

### inline-block

- 옆으로 나열되며, 크기 조정이 가능하다.

## rem, em 차이?

일단 기본적으로 사이즈를 잡는데에 절대 값으로 사이즈를 잡을 때는 px 단위를 사용한다.
그에 반하여, 상대적으로 사이즈를 잡을 때는 %, v\*(view port), em, rem 을 사용한다.

- 부모요소에 따라서 사이즈가 변경이 되야 한다면 %, em
- 부모요소와 상관없이 브라우저 사이즈에 따라서 사이즈를 잡으려면 v\*, rem
- 요소의 너비와 높이에 따라서 사이즈가 변경이 되야 한다면 %, v\* 를 사용한다.
- 폰트의 크기에 따라서 사이즈가 변경되야 한다면 v\* 또는 rem 을 쓴다.

### em 의 예

em 은 부모요소에 영향을 받아 유동적인 값을 갖는다.

```html
<div class="div1">
  <p>div1</p>
  <div class="div1_1">
    <p>div1</p>
    <div class="div1_1_1">
      <p>div1</p>
    </div>
  </div>
</div>
```

```css
.div1 {
  font-size: 1em;
}

.div1_1 {
  font-size: 2em;
}

.div1_1_1 {
  font-size: 3em;
}
```

![em](/asset/em.png)

- 첫번 째 div: 1em 이기 때문에, 16px

- 두번 째 div: 부모요소 16px 의 2배이기 때문에 32px

- 세번 째 div: 부모요소 32px 의 3배이기 때문에 96px

### rem 의 예

rem 은 부모요소에 영향을 받지 않고 고정 값을 갖는다.

![em](/asset/rem.png)

- 첫번 째 div: 부모요소에 상관없이 1rem(16px)

- 두번 째 div: 부모요소에 상관없이 2rem(32px)

- 세번 째 div: 부모요소에 상관없이 3rem(48px)

### 결론

- em 은 가급적이면 사용을 지양하되, 꼭 필요할 경우에만 사용하자.
- 브라우저 사이즈가 변경됨에 따라 padding 이나 margin 값이 변해야 하는 경우,
  em 또는 rem 을 써야 한다.

---
