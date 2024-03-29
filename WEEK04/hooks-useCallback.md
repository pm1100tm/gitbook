# hooks - useCallback

## ❓What is **useCallback**

인자로 전달한 함수를 기억하여 해당 함수를 재사용한다.

### 예제 코드

```js
export default function Hello() {
  const [name, setName] = useState('');

  const handleOnChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setName(e.target.value);
  };

  const handleClick = () => {
    console.log(`handleClick 에서 name 의 값은? ${name}`);
  };

  useEffect(() => {
    console.log('handleOnChange 변경!');
  }, [handleClick]);

  return (
    <div>
      <input type='text' value={name} onChange={handleOnChange} />
      <button type='button' onClick={handleClick}>
        Click
      </button>
    </div>
  );
}
```

- 유저가 입력할 때마다 name 상태 값이 바뀌기 때문에, 입력할 때마다 컴포넌트의 리렌더링이 발생한다.
- 매 입력마다 컴포넌트가 리렌더링 되기 때문에 handleClick 함수도 매번 초기화된다.
- handleClick 함수가 매번 초기화되어, 함수 객체의 주소값이 달라지기 때문에, 매 입력마다 useEffect
  가 실행된다.

![usecallback](/WEEK04/asset/useCallback/useCallback1.png)

### handleClick 함수를 useCallback 으로 감싸기

```js
export default function Hello() {
  const [name, setName] = useState('기본 값');

  const handleOnChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setName(e.target.value);
  };

  const handleClick = useCallback(() => {
    console.log(`handleClick 에서 name 의 값은? ${name}`);
  }, []);

  useEffect(() => {
    console.log('handleOnChange 변경!');
  }, [handleClick]);

  return (
    <div>
      <input type='text' value={name} onChange={handleOnChange} />
      <button type='button' onClick={handleClick}>
        Click
      </button>
    </div>
  );
}
```

- handleClick 함수를 useCallback 으로 메모이제이션하고, 의존성배열에 빈배열을 설정하였다.
- 입력 값으로 컴포넌트가 매번 리렌더링 되어도, handleClick 함수는 변경되지 않는다.
- Input 에 문자를 입력한 후에 버튼을 클릭하면, '기본 값'이 출력된다.
- 의존성 배열에 빈배열이 들어가 있기 때문에, 컴포넌트 렌더링시 한 번만 호출되고, 그 후에는 아무리
  리렌더링되어도 handleClick 함수가 초기화되지 않고, 해당 주소 값이 남아있다.

![usecallback](/WEEK04/asset/useCallback/useCallback2.png)

### useCallback 의존성배열 수정

```js
export default function Hello() {
  const [name, setName] = useState('기본 값');

  const handleOnChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setName(e.target.value);
  };

  const handleClick = useCallback(() => {
    console.log(`handleClick 에서 name 의 값은? ${name}`);
  }, [name]);

  useEffect(() => {
    console.log('handleOnChange 변경!');
  }, [handleClick]);

  return (
    <div>
      <input type='text' value={name} onChange={handleOnChange} />
      <button type='button' onClick={handleClick}>
        Click
      </button>
    </div>
  );
}
```

- 입력 값에 따라서 name 이라는 상태 값이 변경된다.
- useCallback의 의존성 배열에 name 값을 설정하였음으로, 입력 값에 따라서 함수가 초기화된다.
- 따라서 버튼을 클릭했을 때 입력한 값이 그대로 콘솔에 표시된다.

![usecallback](/WEEK04/asset/useCallback/useCallback3.png)

---

여기까지 보면 useCallback을 사용할때와 사용하지 않을 때의 큰차이가 없음을 알 수 있다.

그러면, 이벤트와 상태를 하나 더 추가해보자!

### 상태 값과 이벤트 추가

```js
export default function Hello() {
  const [name, setName] = useState('기본 값');
  const [nickName, setNickName] = useState('');

  const handleOnChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setName(e.target.value);
  };

  const handleOnChange2 = (e: React.ChangeEvent<HTMLInputElement>) => {
    setNickName(e.target.value);
  };

  const handleClick = useCallback(() => {
    console.log(`handleClick 에서 name 의 값은? ${name}`);
  }, [name]);

  const handleClick2 = useCallback(() => {
    console.log(`handleClick 에서 nickName 의 값은? ${nickName}`);
  }, [nickName]);

  useEffect(() => {
    console.log('handleOnChange 변경!');
  }, [handleClick]);

  return (
    <div>
      <div>
        <input type='text' value={name} onChange={handleOnChange} />
        <button type='button' onClick={handleClick}>
          Click
        </button>
      </div>
      <div>
        <input type='text' value={nickName} onChange={handleOnChange2} />
        <button type='button' onClick={handleClick2}>
          Click
        </button>
      </div>
    </div>
  );
}
```

- 새로운 상태 값 nickName 을 만들고 이벤트를 연결하여, 입력해보았다.
- nickName 값을 아무리 변경해도 handleClick 함수는 변경되지 않는 것을 확인할 수 있다.

![usecallback](/WEEK04/asset/useCallback/useCallback4.png)

---

### 다른 예제를 살펴보자

아래 2개의 컴포넌트가 있다.

```js
// /src/component/Hello.tsx
import { useState } from 'react';

import AnotherComponent from './AnotherCount';

export default function Hello() {
  const [count, setCount] = useState < number > 0;
  const [isDark, setIsDark] = useState < boolean > false;

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) =>
    setCount(Number(e.target.value));

  const handleDark = () => setIsDark((prev) => !prev);

  return (
    <div
      style={{
        background: isDark ? 'black' : 'white',
      }}
    >
      <input type='number' value={count} onChange={handleChange} />
      <button type='button' onClick={handleDark}>
        Dark-or-White
      </button>
      <AnotherComponent handleDark={handleDark} />
    </div>
  );
}

// /src/component/AnotherComponent.tsx
import { useEffect } from 'react';

export default function AnotherComponent({
  handleDark,
}: {
  handleDark: () => void;
}) {
  useEffect(() => {
    console.log('Change count in AnotherCount');
  }, [handleDark]);

  return <div>div</div>;
}
```

- Hello 컴포넌트 안에 AnotherComponent 컴포넌트가 있다.
- Input 에서 숫자 값을 올려도, 버튼을 클릭하여 불리언 값을 바꿔도, AnotherComponent 의
  useEffect 는 계속하여 호출된다.
- count 값이나 isDark 값이 변경되면, 상위 Hello 컴포넌트가 리렌더링 된다.
- 하위의 AnotherComponent 의 useEffect의 의존성배열은 handleDark 함수가 설정되어있는데,
  count 나 isDark 값이 변경될 때 마다, 컴포넌트가 리렌더링되기 때문에, handleDark 함수도
  계속해서 초기화되는 것이 원인이다.
- 해결책은 isDark 함수를 useCallback으로 감싸주는 주고, isDark 값을 의존성배열에 추가하는 것이다.

```js
import { useCallback, useState } from 'react';

import AnotherComponent from './AnotherCount';

export default function Hello() {
  const [count, setCount] = useState < number > 0;
  const [isDark, setIsDark] = useState < boolean > false;

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) =>
    setCount(Number(e.target.value));

  const handleDark = useCallback(() => setIsDark((prev) => !prev), [isDark]);

  return (
    <div
      style={{
        background: isDark ? 'black' : 'white',
      }}
    >
      <input type='number' value={count} onChange={handleChange} />
      <button type='button' onClick={handleDark}>
        Dark-or-White
      </button>
      <AnotherComponent handleDark={handleDark} />
    </div>
  );
}
```

### Ref

- [React Hooks에 취한다 - useCallback 짱 쉬운 강의 | 리액트 훅스 시리즈](https://www.youtube.com/watch?v=XfUF9qLa3mU&list=PLZ5oZ2KmQEYjwhSxjB_74PoU6pmFzgVMO&index=7)
