# ETC-lodash

[lodash](https://lodash.com)는 자바스크립트에서 객체를 비교하기 위해 사용되는
라이브러리이다.

lodash 는 두 객체 혹은 두 값을 정확하게 확인할 수 있는 isEqual(object1, object2) 함수가 있다.
이것이 가능한 이유는 isEqual 함수가 속성 기납 등가 비교 방식으로 구현되었기 때문이다.
속성 기반 등가 비교 방식은 객체의 각 속성을 비교한다.

다음의 예제에서 객체가 같은지 정확하게 비교하기 위해 각 속성을 비교한다.

```js
function isEquivalent(a, b) {
  // arrays of property names
  var aProps = Object.getOwnPropertyNames(a);
  var bProps = Object.getOwnPropertyNames(b);

  // If their property lengths are different, they're different objects
  if (aProps.length != bProps.length) {
    return false;
  }

  for (var i = 0; i < aProps.length; i++) {
    var propName = aProps[i];

    // If the values of the property are different, not equal
    if (a[propName] !== b[propName]) {
      return false;
    }
  }

  // If everything matched, correct
  return true;
}
isEquivalent({ hi: 12 }, { hi: 12 }); // returns true
```
