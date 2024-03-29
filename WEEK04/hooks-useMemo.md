# hooks - useMemo

## ❓What is **useMemo**

- 컴포넌트 최적화에 사용되는 대표적인 훅이며, 비슷한 훅으로는 useCallback이 있다.
- memo란? Memoization 을 뜻한다.
- 동일한 값을 리턴하는 함수를 반복적으로 호출해야 할 때, 가장 처음에 사용했던 값을 메모리에 저장한다.
- 즉, 자주 사용되는 **값**을 필요할 때 마다 캐시에서 가져와서 사용하는 것이다.
- 함수형 컴포넌트는 상태가 변경될 때 마다 리렌더링되고, 그 때 마다 내부 변수가 초기화된다.
- 꼭 필요할 때만 적절하게 사용하는 것이 성능에 좋다.

### 예제 코드

```js
function loadSomeData(value: number): number {
  for (let i = 0; i < 999999999; i++) {}
  return value;
}

export default function Hello() {
  const [countA, setCountA] = useState(0);
  const [countB, setCountB] = useState(0);

  const result = loadSomeData(countA);

  return (
    <div>
      <div>countA: {result}</div>
      <div>countB: {countB}</div>
      <div>
        <input
          type='number'
          value={result}
          onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
            setCountA(Number(e.target.value))
          }
        />
      </div>
      <div>
        <input
          type='number'
          onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
            setCountB(Number(e.target.value))
          }
        />
      </div>
    </div>
  );
}
```

- 컴포넌트가 렌더링되자마자 loadSomeData 함수가 실행되고 딜레이가 걸린다.
- 2개의 인풋 값 중에 어떤 것을 업데이트 하더라도 loadSomeData 함수가 실행되어 딜레이가 걸린다.

```js
const result = useMemo(() => loadSomeData(countA), [countA]);
```

- 위의 코드에서는 useMemo 를 사용하여 loadSomeData 함수에 대한 값을 메모이제이션하도록 수정한다.
- 그러면 두번째 input 값은 딜레이 없이 변경이 가능하다.
- 즉, loadSomeData 함수의 리턴 값이 된 countA useMemo의 의존성배열에 설정하여 같은 컴포넌트
  내에서 다른 상태 값이 변경이 되어도, countA의 값은 변경되지 않아 딜레이가 발생하지 않는다.

---

### 값이 아니라 객체라면?

```js
export default function Hello() {
  const [count, setCount] = useState(0);
  const [user, setUser] = useState < User > { name: '', age: '' };

  const handleOnChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setCount(Number(e.target.value));
  };

  const handleFormChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setUser((prevUser) => ({ ...prevUser, [e.target.name]: e.target.value }));
  };

  const nameAndAge = { ...user, nameAndAge: `${user.name}-${user.age}` };

  useEffect(() => {
    console.log('call!');
  }, [nameAndAge]);

  return (
    <div>
      <div>
        <input type='number' value={count} onChange={handleOnChange} />
      </div>
      <div>
        <span>{JSON.stringify(nameAndAge)}</span>
      </div>
      <hr />
      <div>
        <label htmlFor='name'>Name</label>
        <input
          type='text'
          name='name'
          value={user.name}
          onChange={handleFormChange}
        />
      </div>
    </div>
  );
}
```

- 상태 값이 2개가 있다. 하나는 원시값을 갖는 count, 또 다른 하나는 user 라는 객체
- user 객체의 키 값을 추가하여 리턴하도록 하고, useEffect를 추가하여 의존성배열에 nameAndAge 설정
- 이렇게 했을 때 기대하는 동작은, user 의 값이 바뀌었을 때만 useEffect가 호출되고,
  count 값이 바뀌없을 떄는 useEffect가 호출되지 않아야 한다.
- 그러나 useEffect 는 count 값이 바뀌던지 user 객체의 값이 바뀌던지 언제나 호출된다.
- 즉, nameAndAge 라는 객체가 매번 새롭게 생성되기 때문이다.

아래의 코드와 같이 수정하자.

```js
const nameAndAge = useMemo(() => {
  const data = { ...user, nameAndAge: `${user.name}-${user.age}` };
  return data;
}, [user.name]);

useEffect(() => {
  console.log('call!');
}, [nameAndAge]);
```

이렇게 하면 user 의 name 값이 useMemo의 의존성배열에 설정되어, useEffect는 user의 name 값이
변경될 때에만 실행되게 된다.
