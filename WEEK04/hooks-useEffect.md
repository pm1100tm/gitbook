# hooks - useEffect

## ❓What is **useEffect**

- 렌더링 후 일부 코드를 실행하여 컴포넌트에 이벤트를 걸거나 외부 시스템과 React 동기화

  - 특정 컴포넌트가 마운트/언마운트 될 때 채팅 서버 연결/중지
  - 특정 컴포넌트가 마운트 될 때 서버에서 데이터 가져오기
  - 특정 컴포넌트가 화면에 표시될 때 분석 로그 전송

- 기본적으로 렌더링 또는 리렌더링 할 때마다 실행
- 렌더링 될 때 React 는 화면을 업데이트 한 후에, useEffect 내부의 코드를 실행
- 즉, 렌더링이 브라우저에 반영될 때 까지, useEffect의 실행은 **지연**

### ❓렌더링이란?

- 컴포넌트의 최상위 레벨
- 값 또는 상태를 가져와서 반환 후, 화면에 표시할 JSX를 반환
- 렌더링 코드는 순수하게 결과만 계속하고 다른 작업은 수행하지 않아야 함

### 🏒 사용방법: 기본 코드 및 주의 사항

아래의 코드는 useEffect 의 기본 코드이다.

```javascript
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    // Code here will run after _every_ render
  });

  return <div />;
}
```

다음의 코드는 에러를 발생시킨다. 렌더링 중 DOM 노드 수정과 같은 작업은 하면 안된다.

해결 방법은 렌더링이 끝난 후 DOM 조작을 해주면 되는 것인데 **useEffect** 를 사용하면 된다.
DOM 업데이트를 useEffect 로 감싸면 React는 화면을 먼저 업데이트 하고나서 useEffect 내부의
코드를 실행한다.

```javascript
import { useState, useRef, useEffect } from 'react';

function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  if (isPlaying) {
    ref.current.play(); // Calling these while rendering isn't allowed.
  } else {
    ref.current.pause(); // Also, this crashes.
  }

  return <video ref={ref} src={src} loop playsInline />;
}

export default function App() {
  const [isPlaying, setIsPlaying] = useState(false);
  return (
    <>
      <button
        onClick={() => {
          setIsPlaying(!isPlaying);
        }}
      >
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <VideoPlayer
        isPlaying={isPlaying}
        src='https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4'
      />
    </>
  );
}
```

![img-useEffect_error01](/asset/img-useEffect_error01.png)

### 🏒 종속성 지정

- 기본적으로 useEffect는 모든 렌더링 후에 실행된다.
- 같은 컴포넌트 내에서 어떤 상태 값이 변경되면 컴포넌트는 리렌더링이 된다.
- 이펙트 내부의 함수는 계속해서 호출된다.
- 이런 경우와 같이 매번 실행되길 원하지 않는 경우가 더 많다.
- 이 떄 종속성을 사용한다. **종속성은 지정할 수 있고, 기본 배열([]) 값을 설정할 수도 있다.**

아래의 예제는 종속성으로 빈 배열([])을 지정하였다.
따라서, console.log('test')는 컴포넌트가 오직 처음 마운트 될 때에만 실행된다.

```js
import { useState, useEffect } from 'react';

export default function App() {
  const [count, setCount] = useState < number > 0;

  useEffect(() => {
    console.log('test');
  }, []);

  return (
    <>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        Increase - {count}
      </button>
    </>
  );
}
```

---

- 아래의 코드는 버튼을 클릭했을 때 아무런 동작이 발생하지 않는다.
- 문제는 이펙트 내부의 코드가 **isPlaying** 값에 의존하여 수행할 작업을 결정하는데,
  이 값이 의존성배열에 없기 때문이다.
- 이 문제를 해결하려면 종속성 배열에 **isPlaying** 을 추가하면 된다.
- 의존성 배열에는 여러 개의 값이 들어갈 수 있다.
- React는 지정한 모든 종속성의 값이 이전 렌더링 시와 정확히 동일한 경우에만, 이펙트를 다시 실행하는
  것을 Skip 한다.

```js
import { useState, useRef, useEffect } from 'react';

function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  useEffect(() => {
    if (isPlaying) {
      console.log('Calling video.play()');
      ref.current.play();
    } else {
      console.log('Calling video.pause()');
      ref.current.pause();
    }
  }, []); // >> This causes an error

  return <video ref={ref} src={src} loop playsInline />;
}

export default function App() {
  const [isPlaying, setIsPlaying] = useState(false);
  const [text, setText] = useState('');
  return (
    <>
      <input
        value={text}
        onChange={(e) => {
          setText(e.target.value);
        }}
      />
      <button
        onClick={() => {
          setIsPlaying(!isPlaying);
        }}
      >
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <VideoPlayer
        isPlaying={isPlaying}
        src='https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4'
      />
    </>
  );
}
```

> 알아둘 것
>
> React는 Object.is 비교를 사용하여 종속성의 값을 비교한다.
> 지정한 종속성이 Effect 내부의 코드를 기반으로 React가 예상하는 것과 일치하지 않으면 린트
> 오류가 발생한다.

여기까지 정리

```js
useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount _and also_ if either a or b have changed since the last render
}, [a, b]);
```

### 클립업 코드

아래의 코드는 3rd Party 채팅 서버를 연결하는 코드이다.

```javascript
useEffect(() => {
  const connection = createConnection();
  connection.connect();
});
```

이펙트는 기본적으로 컴포넌트가 마운트 될 떄(리렌더링 될 때) 계속해서 실행되기 때문에, 빈배열([])을
종속성에 추가하자.

```javascript
useEffect(() => {
  const connection = createConnection();
  connection.connect();
}, []);
```

하지만, 이것도 좋은 방법이라고 할 수 없다.

#### 왜? 🧐

해당 컴포넌트가 큰 앱의 일부라고 가정하고, 해당 컴포넌트가 화면에 표시되어 이펙트가 발생한다.
그 후, 뒤로 가기 버튼을 클릭하고, 다시 컴포넌트로 들어온다. 이 사용자 동작에서의 이슈는, 컴포넌트가
마운트 되고 해제 되었을 때, 채팅 서버 연결이 계속 남아있어 쌓이게 된다는 것에 있다.

이 문제를 해결하기 위해서는 클린업 코드를 추가해야 한다.

```javascript
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

export default function ChatRoom() {
  useEffect(() => {
    const connection = createConnection();
    connection.connect();

    return () => connection.disconnect(); // clean up
  }, []);
  return <h1>Welcome to the chat!</h1>;
}
```

React는 Effect가 **다시 실행되기 전에 매번 클린업 함수를 호출하고**,
**컴포넌트가 마운트 해제(제거)될 때 마지막으로 한 번 호출한다.**
**연결을 끊었다가 다시 연결하는 것이 올바른 동작이다.**

### 개발 중에 이펙트가 두 번 실행되는 것을 어떻게 처리하는가

<React.StrictMode> 로 컴포넌트를 감싼다면, Effect 등을 두 번씩 실행한다.
이것은 React가 버그를 찾기 위해서 개발중에는 의도적으로 컴포넌트를 다시 마운트하는 것에서 기인한
동작이다. 두번 호출되지 않도록 하기 위한 방법은 **이펙트를 한 번 실행하는 방법**이 아니라
**이펙트를 다시 마운트 한 후에도 작동하도록 수정** 하는 방법이다.
즉, 클린업 함수를 구현하는 것이다.

> **_프로덕션 환경에서는 두번 호출되지 않는다._**

#### 내장된 dialog 요소의 showModal 메서드는 두 번 호출하면 에러 발생

- 클린업 함수 구현

```javascript
useEffect(() => {
  const dialog = dialogRef.current;
  dialog.showModal();
  return () => dialog.close();
}, []);
```

#### 구독하는 항목이 있는 경우 클린업 함수에서 구독 취소

```javascript
useEffect(() => {
  function handleScroll(e) {
    console.log(window.scrollX, window.scrollY);
  }
  window.addEventListener('scroll', handleScroll);
  return () => window.removeEventListener('scroll', handleScroll);
}, []);
```

#### 애니메이션이 적용된 경우 클린업 함수는 애니메이션 값을 초기 값으로 재설정

```javascript
useEffect(() => {
  const node = ref.current;
  node.style.opacity = 1; // Trigger the animation
  return () => {
    node.style.opacity = 0; // Reset to the initial value
  };
}, []);
```

#### 무언가를 가져오는 경우 정리 함수는 가져오기를 중단하거나 결과를 무시

```javascript
useEffect(() => {
  let ignore = false;

  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }

  startFetching();

  return () => {
    ignore = true;
  };
}, [userId]);
```

이미 발생한 네트워크 요청을 실행 취소 할 수 없기 때문에, 또한 네트워크 요청이 늦어질 수 있기 때문에,
클린업 함수를 사용하여 더 이상 관련 없는 실행이 영향을 미치지 않도록 제어해준다.

## 챌린지

이 카운터 컴포넌트는 매초마다 증가해야 하는 카운터를 표시한다. 마운트 시에는 setInterval을 호출하는데
그러면 onTick이 매초마다 실행되고, 2씩 증가시킨다.

```javascript
import { useState, useEffect } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    function onTick() {
      setCount((c) => c + 1);
    }

    setInterval(onTick, 1000);
  }, []);

  return <h1>{count}</h1>;
}
```

- 원인
  - strict mode 인 경우 React는 각 컴포넌트를 한 번씩 다시 마운트한다.
    이로 인해 카운트가 2씩 증가된다. 원인은 컴포넌트가 2번 마운트 되는 것이 아니라, 이펙트 실행에서
    클린업 함수를 제공하고 있지 않기 때문이다.
- 해결방법
  - clearInterval 함수를 사용하여 클린업 함수 제공

```javascript
import { useState, useEffect } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    function onTick() {
      setCount((c) => c + 1);
    }

    const intervalId = setInterval(onTick, 1000);
    return () => clearInterval(intervalId);
  }, []);

  return <h1>{count}</h1>;
}
```

---

아래의 컴포넌트는 선택한 사람의 정보를 표시한다. 셀렉트 박스로 사람을 선택할 때마다 컴포넌트는 리렌더링
되고 비동기 함수를 호출하여 선택한 사람의 정보를 표시한다.

여기서의 버그는 셀렉트 박스의 값을 충분히 빠르게 바꿔가면서 선택하면, 다른 사람의 정보가 표시되는 것이다.

```javascript
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);

  useEffect(() => {
    setBio(null);
    fetchBio(person).then((result) => {
      setBio(result);
    });
  }, [person]);

  return (
    <>
      <select
        value={person}
        onChange={(e) => {
          setPerson(e.target.value);
        }}
      >
        <option value='Alice'>Alice</option>
        <option value='Bob'>Bob</option>
        <option value='Taylor'>Taylor</option>
      </select>
      <hr />
      <p>
        <i>{bio ?? 'Loading...'}</i>
      </p>
    </>
  );
}
```

- 원인
  - 버그의 원인은 두개의 비동기 연산이 서로 경쟁(race condition)하고 있다는 것에 있다.
  - strict mode 인 경우 React는 각 컴포넌트를 한 번씩 다시 마운트한다.
    로 인해 카운트가 2씩 증가된다. 원인은 컴포넌트가 2번 마운트 되는 것이 아니라, 이펙트 실행에서
    클린업 함수를 제공하고 있지 않기 때문이다.
- 해결방법
  - race condition 버그를 수정하기 위해서 클린업 함수를 추가한다.

```javascript
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);
  useEffect(() => {
    let ignore = false;
    setBio(null);
    fetchBio(person).then((result) => {
      if (!ignore) {
        setBio(result);
      }
    });
    return () => {
      ignore = true;
    };
  }, [person]);

  return (
    <>
      <select
        value={person}
        onChange={(e) => {
          setPerson(e.target.value);
        }}
      >
        <option value='Alice'>Alice</option>
        <option value='Bob'>Bob</option>
        <option value='Taylor'>Taylor</option>
      </select>
      <hr />
      <p>
        <i>{bio ?? 'Loading...'}</i>
      </p>
    </>
  );
}
```

각각의 렌더링의 useEffect 에는 고유한 변수 ignore 가 존재한다. 처음 기본 값은 false이다.
하지만, 다른 사람을 선택하는 경우 ignore 값은 true 가 된다.

이제 요청이 완료되는 순서는 중요하지 않고, 마지막에 선택한 사람의 ignore 값만 false 가 되고,
해당 사람의 정보만 표시한다.

- 'Bob'을 선택하면 fetchBio('Bob')가 트리거
- 'Taylor'를 선택하면 fetchBio('Taylor')가 트리거되고 이전 (Bob의) 이펙트가 정리
- 'Bob'을 가져오기 전에 'Taylor' 가져오기가 완료
- 'Taylor' 렌더의 이펙트는 setBio('이것은 Taylor의 바이오입니다')를 호출
- 'Bob' 불러오기 완료

'Bob' 렌더링의 이펙트는 ignore 플래그가 true로 설정되었기 때문에 아무 작업도 수행하지 않는다.
오래된 API 호출의 결과를 무시하는 것 외에도 AbortController를 사용하여 더 이상 필요하지 않은 요청을
취소할 수도 있다. 그러나 이것만으로는 race condition을 방지하기에 충분하지 않다.

AbortController를 사용하기 위해서는 더 많은 비동기 단계가 연쇄적일 가능성이 있기 때문에, ignore
와 같은 명시적 플래그를 사용하는 것이 더 안정적인 방법이다.

## React가 effect를 정리(clean-up)하는 시점은 정확히 언제인가

컴포넌트가 마운트 해제되는 시점에 정리 함수를 실행한다. 이펙트는 렌더링 될 떄 마다 실행된다.
따라서 다음 차례의 이펙트를 실행하기 전에 이전의 렌더링에서 파생된 이펙트를 정리한다.
이것은 메모리 누수도 방지하며, 버그를 방지하는데 도움이 된다.

## 정리

- 이벤트와 달리 useEffect는 특정 상호작용이 아닌 렌더링 자체로 인해 발생
- useEffect를 사용하면 컴포넌트를 외부 시스템(타사 API, 네트워크)과 동기화 할 수 있음
- 기본적으로 useEffect는 모든 렌더링(초기 렌더링 포함) 후에 실행
- 모든 종속성이 마지막 렌더링 때와 동일한 값을 가진다면 useEffect가 실행되지 않음
- 빈 의존성 배열([])은 컴포넌트 마운팅, 즉 화면에 추가되는 것에 해당됨
- Strict 모드에서 React는 컴포넌트를 두 번 마운트하여(개발 중일 때만!) Effect를 스트레스 테스트
- 다시 마운트하는 과정에서 이펙트가 중단되면 정리 함수를 구현해야 한다. (CleanUp)
- React는 다음 번에 이펙트가 실행되기 전과 마운트 해제 중에 정리 함수를 호출한다.

### 읽어보기

[A Complete Guide to useEffect](https://overreacted.io/a-complete-guide-to-useeffect/)
