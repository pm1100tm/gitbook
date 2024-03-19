# GlobalStyle&Theme

## GlobalStyle 과 Theme

## 🎲 CSS Reset

\> [CSS Tools: Reset CSS](https://meyerweb.com/eric/tools/css/reset/)

모든 HTML 태그에는 기본 스타일이 적용되어있는데, 이러한 속성들은 스타일을 잡을 때 오히려 불편하기 떄문에
스타일을 초기화 시켜준다.

다시 말해, 이것은 시작점이지 손댈 수 없는 독립된 블랙박스가 아닙니다.

## npm styled-reset

\> [styled-reset github](https://github.com/zacanger/styled-reset#readme)

```shell
npm i styled-reset
```

공식 깃헙에서 보면 Styled Components 3.x or 2.x 버전인 경우에는, injectGlobal api
를 사용하라고 권장하고, styled-reset@1.7.1 를 설치하라고 권장한다.

현시점(2024년 3월) 사용하는 Styled Components 의 버전은 6.x 이다.

### 사용

```tsx
import { Reset } from 'styled-reset';

export default function App() {
  return (
    <div>
      {/* 최상위에 세팅해준다. */}
      <Reset />
    </div>
  );
}
```

### Global Style 적용

```tsx
import { Reset } from 'styled-reset';

import GlobalStyle from './styles/GlobalStyle';

export default function App() {
  return (
    <div>
      {/* 최상위에 세팅해준다. */}
      <Reset />
      <GlobalStyle />
    </div>
  );
}
```

```ts
// /src/styles/GlobalStyle.ts

import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  html {
    box-sizing: border-box;
  }

  *,
  *::before,
  *::after {
    box-sizing: inherit;
  }

  html {
    font-size: 62.5%; // 1rem = 10px
  }

  body {
    font-size: 1.6rem; // 1.6rem = 16px
  }

  :lang(ko) {
    h1, h2, h3 {
      word-break: keep-all;
    }
  }
`;

export default GlobalStyle;
```
