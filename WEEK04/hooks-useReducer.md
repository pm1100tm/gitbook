# hooks - useReducer

## ❓What is **useReducer**

- 상태 값을 관리하는 useState와는 다른 hook
- 상태 업데이트 로직을 컴포넌트와 분리 가능
- 주로 복잡한 상태를 관리할 때 사용(간단한 값은 useState 사용)
- 앱이 복잡하고 확장성이 있는 경우 useReducer를 이용한 상태관리가 좋다.
  나중에 action 만 추가하고, dispatch 함수만 자식 컴포넌트에 전달하기만 하면 되기 때문이다.
- 로직이 분리가 되니 테스트 코드도 짜기 쉬움

### useReducer 의 형태

```js
const reduer = (state, action) => {
  switch (action.type) {
    case 'action1':
      return 'return from some logic';

    case 'action2':
      return 'return from some logic';

    default:
      return state;
  }
};

export default function SomeComponent() {
  const [state, dispatch] = useReducer(reducer, initialState);

  // some code ...

  return (
    <div>
      <button type='button' onClick={() => dispatch({ type: 'action1' })}>
        some event button1
      </button>
      <button type='button' onClick={() => dispatch({ type: 'action2' })}>
        some event button2
      </button>
    </div>
  );
}
```
