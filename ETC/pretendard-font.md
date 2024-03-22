# Pretendard font 사용하기

## React 에서 Pretendard font 사용하기

### Step1

아래의 코드를 CSS 파일에 붙여넣는다.

```css
@font-face {
  font-family: 'Pretendard';
  font-weight: 900;
  src: url(pre/Pretendard-Black.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 800;
  src: url(pre/Pretendard-ExtraBold.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 700;
  src: url(pre/Pretendard-Bold.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 600;
  src: url(pre/Pretendard-SemiBold.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 500;
  src: url(pre/Pretendard-Medium.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 400;
  src: url(pre/Pretendard-Regular.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 300;
  src: url(pre/Pretendard-Light.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 200;
  src: url(pre/Pretendard-ExtraLight.woff) format('woff');
}
@font-face {
  font-family: 'Pretendard';
  font-weight: 100;
  src: url(pre/Pretendard-Thin.woff) format('woff');
}
```

### Step2

쓰임에 맞게 font-family 의 값을 변경한다.

```css
@font-face {
  font-family: 'pretendard-400';
  src: url('https://cdn.jsdelivr.net/gh/Project-Noonnu/noonfonts_2107@1.1/Pretendard-Regular.woff')
    format('woff');
  font-weight: 400;
  font-style: normal;
}
```

### Step3

아래와 같이 변경한 값으로 font-family 에 설정한다.

```css
body {
  font-family: pretendard-400;
  font-size: 1.6rem;
  color: white;
}
```
