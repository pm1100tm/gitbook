# hooks - useState

## ❓What is **useState**

- 상태 값을 저장하고 업데이트 할 수 있는 도구를 제공한다.
- 일반 변수를 선언하고 값을 변경 후, 변경한 값을 화면에 출력하려고 할 때 일반적으로는 변경된 값이
  표시되지 않는다.

### useState 의 형태

#### 원시 상태 값 저장

```js
const [count, setCount] = useState(0);
```

#### 객체 상태 값 저장

```js
type User = {
  id: number,
  name: string,
};

const nullUser = {
  id: 0,
  name: '',
};

export default function SomeComponent() {
  const [user, setUser] = useState(nullUser);

  // some logic
}
```
