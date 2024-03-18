# ❓Vendor Prefix 란?

CSS3 표준으로 확정되기 이전 또는 브라우저 개발사가 실험적으로 제공하는 기능을 사용하기 위해서는
벤더 프리픽스(Vendor Prefix)를 사용하여야 한다.

대표적으로 user-select 속성은 모든 브라우저에서 벤더 프리픽스를 사용해야 한다.

그러나, 브라우저의 버전이 올라감에 따라 벤더 프리픽스를 사용하지 않아도 될 수 있다.

```css
* {
  -webkit-user-select: none; /* Chrome all / Safari all */
  -moz-user-select: none; /* Firefox all */
  -ms-user-select: none; /* IE 10+ */
  user-select: none; /* Likely future */
}
```

많은 브라우저를 위한 벤더 프리픽스를 사용하는 것은 코드의 양을 늘게 하고 또한 브라우저는 거의 매달
업데이트가 이루어지고 있어 불필요한 벤더 프리픽스를 사용할 가능성이 크다.
사용하지 않아도 되는 벤더 프리픽스를 사용하는 것은 성능에도 영향을 주기 때문에 **Prefix Free**
라이브러리 를 사용하는 것은 매우 유용한 방법이다.

\>> [Prefix free](https://projects.verou.me/prefixfree/)

```javascript
<script src='prefixfree.min.js'></script>
```
