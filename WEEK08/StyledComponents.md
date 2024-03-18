# Styled Components

## 🎮 Styled Components 를 써보자

Styled Components 는 React 컴포넌트 시스템에서의 복잡한 CSS를 어떻게 개선할 수 있을지에 대한 결과이다. 다음과 같은 이점이 있다.

- 렌더링되는 컴포넌트를 추적하여 해당 스타일만 삽입하고 수정한다.
- 코드를 분할하여 최소한의 코드만 로드할 수 있다.
- 고유한 클래스 이름을 생성해주기 때문에 클래스명에 버그가 없다.
- 따라서, 하나의 컴포넌트가 삭제되면, 해당하는 모든 스타일을 삭제할 수 있다.
- 컴포넌트의 props 와 글로벌 테마를 기반으로 스타일을 조정하여 직관적으로 관리한다.
- 컴포넌트에 영향을 미치는 스타일을 찾기 위해서 여러 파일을 조사하지 않아도 된다.
- 표준에 맞게 CSS를 작성하고 나머지는 Styled Components 가 처리하도록 한다. tailwind 와 같은
  경우에는 표준에 맞는 CSS는 아니기 때문에, 진입 장벽이 있다.

## 🎸 Styled Components 설치

npm 으로 설치하되, type 까지 설치해준다.

```shell
npm install styled-components
npm i -D @types/styled-components @swc/plugin-styled-components
```

SWC로 Babel 을 대체한다.

```shell
npm i -D @swc/plugin-styled-components
```

> React 에서 SWC란?
>
> - Speedy Web Compiler의 약자
> - Rust로 작성된 초고속 자바스크립트/타입스크립트 컴파일러
> - 바벨과 타입스크립트를 대체할 수 있도록 설계되어 훨씬 빠른 컴파일 시간을 제공
> - SWC는 컴파일과 번들링 모두에 사용 가능

컴파일의 경우, 최신 JavaScript 기능을 사용하여 JavaScript/TypeScript 파일을 가져와 모든
주요 브라우저에서 지원되는 유효한 코드를 출력한다.

### .swcrc 파일 작성하기

```json
{
  "jsc": {
    "experimental": {
      "plugins": [
        [
          "@swc/plugin-styled-components",
          {
            "displayName": true,
            "ssr": true
          }
        ]
      ]
    }
  }
}
```

## VSCODE 확장 프로그램 설치(중요)

vscode 에서 편리하게 사용하기 위해서 **vscode-styled-components** extension 을 설치한다.

---

## 👏 사용해보기

### 1. 기본적인 사용방법

```javascript
import styled from 'styled-components';

const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

export default function Test() {
  <Wrapper>
    <Title className={className}>title</Title>
  </Wrapper>;
}
```

### 2. 여러가지 스타일을 정의한 후 넘겨받아 사용하기

```javascript
import styled from 'styled-components';

const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

const BigTitle = styled(Title)`
  font-size: 3em;
`;

const RedBigTitle = styled(BigTitle)`
  font-size: 3em;
  color: red;
`;

export default function Test() {
  <Wrapper>
    <RedBigTitle>title</RedBigTitle>
  </Wrapper>;
}
```

### 3. return 코드를 따로 빼서 className 으로 대체하여 사용하기

```javascript
import styled from 'styled-components';

const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

const BigTitle = styled(Title)`
  font-size: 3em;
`;

const RedBigTitle = styled(BigTitle)`
  font-size: 3em;
  color: red;
`;

function TitleCSS({ className }: React.HtmlHTMLAttributes<HTMLElement>) {
  return (
    <Wrapper>
      <RedBigTitle className={className}>title</RedBigTitle>
    </Wrapper>
  );
}

const SmallTitle = styled(TitleCSS)`
  font-size: 0.8rem;
`;

export default function Test() {
  return <SmallTitle />;
}
```

> 알아둘 것: styled. 의 역할은 클래스를 추가하는 것이다.

### 3. props 값을 사용하여 동적으로 CSS 속성 값을 변경

#### version 1

```tsx
import { styled } from 'styled-components';
import { useBoolean } from 'usehooks-ts';

type ButtonProps = {
  active?: boolean;
};

function background(props: ButtonProps) {
  return props.active ? '#00FF' : '#FFF';
}

const Button = styled.button<ButtonProps>`
  background: ${(props) => background(props)};
`;

export default function Switch() {
  const { value: active, toggle } = useBoolean(false);

  return (
    <Button type='button' onClick={toggle} active={active}>
      On/Off
    </Button>
  );
}
```

#### version 2

```tsx
import { styled } from 'styled-components';
import { useBoolean } from 'usehooks-ts';

type ButtonProps = {
  active?: boolean;
};

const Button = styled.button<ButtonProps>`
  font-size: ${(props: ButtonProps) => (props.active ? '2rem' : '1rem')};
  background: #fff;
`;

export default function Switch() {
  const { value: active, toggle } = useBoolean(false);

  return (
    <Button type='button' onClick={toggle} active={active}>
      On/Off
    </Button>
  );
}
```

### 4. attr 설정

```tsx
import { css, styled } from 'styled-components';
import { useBoolean } from 'usehooks-ts';

type ButtonProps = {
  type?: 'button' | 'submit' | 'reset';
  active?: boolean;
};

const Button = styled.button.attrs<ButtonProps>((props) => ({
  type: props.type ?? 'button',
}))<ButtonProps>`
  background: #fff;

  ${(props: ButtonProps) =>
    props.active &&
    css`
      background: red;
    `}
`;

const PrimaryButton = styled(Button)`
  background: black;
  color: white;

  ${(props: ButtonProps) =>
    props.active &&
    css`
      background: red;
    `}
`;

export default function Switch() {
  const { value: active, toggle } = useBoolean(false);

  return (
    <PrimaryButton type='submit' onClick={toggle} active={active}>
      On/Off
    </PrimaryButton>
  );
}
```

---

## Theme

jest manual mock 문서 보기
