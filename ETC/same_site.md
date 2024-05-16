# SAME SITE

- SameSite 쿠키 속성은 웹 보안을 강화하기 위해 도입된 기능 중 하나입니다.
- 이 속성은 쿠키가 어떤 조건에서만 전송되도록 제한하는 역할을 합니다.
- 주로 CSRF (Cross-Site Request Forgery) 공격을 방지하는 데 사용됩니다.

SameSite 속성은 크게 세 가지 값으로 설정할 수 있습니다.

## None

쿠키가 모든 요청에 대해 동일한 사이트에서만 전송됩니다. 보안 연결 (HTTPS)에서만 동작하며,
Third-party 쿠키를 허용합니다.

## Strict

쿠키가 동일한 사이트에서만 요청에 포함됩니다. 모든 Cross-Site 요청에서는 쿠키를 전송하지 않습니다.

## Lax

기본적으로 Strict와 같지만, 일부 상황에서 Cross-Site 요청에 대해 쿠키를 전송할 수 있습니다.
예를 들어, \<a href="..."> 링크를 클릭했을 때만 쿠키가 전송됩니다.

### node.js 에서 사용하는 방법

```js
const express = require('express');
const cookieParser = require('cookie-parser');

const app = express();

app.use(cookieParser());

app.get('/', (req, res) => {
  res.cookie('my_cookie', 'cookie_value', {
    sameSite: 'Strict', // 또는 'None' 또는 'Lax'
    secure: true, // HTTPS에서만 쿠키 전송
    httpOnly: true, // JavaScript에서 쿠키에 접근 불가능
  });
  res.send('Cookie set with SameSite attribute!');
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```
