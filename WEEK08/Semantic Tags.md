# Semantic Tags(시맨틱 태그)

## 의미있는 마크업(Semantic Tags)

시맨틱 마크업이라고도 하는 시맨틱 HTML은 그 안에 포함된 콘텐츠의 의미 또는 시맨틱을 전달하는 HTML
태그를 사용하는 것을 말한다. 시맨틱으로 작성하면 페이지의 여러 부분의 역할과 상대적 중요성을 정의하는 데
도움이 되는 추가 정보를 제공할 수 있다.

웹 사이트를 만들 때 모든 것을 div 태그로 할 수 있지만, div 태그로만 작성하다보면 화면의 어느 부분이
코드에 어떤 부분인지 찾기 모호할 수 있다. 그래서 되도록이면 의미있는 마크업을 사용하자는 이야기다.

예를 들어, \<header>, \<article>, \<footer> 와 같은 태그는 시맨틱 HTML 태그이다.
이러한 태그는 포함된 콘텐츠의 역할을 명확하게 나타낸다.

반면에 \<div> 및 \<span> 과 같은 태그는 비시맨트 태그의 대표적인 예이다.
이러한 태그는 콘텐츠 보유자 역할만 할 뿐, 포함된 콘텐츠의 유형이나 페이지에서 해당 콘텐츠가 어떤 역할을
하는지에 대한 정보를 제공하지 않는다.

## 왜 시맨틱 태그를 사용해야 하는가

코드를 검토하는 개발자가 이해하기 쉽다는 명백한 이유 외에도 항상 시맨틱 태그를 지향해야 하는 이유가 있다.

### Accessibility

일반 사용자의 경우 웹페이지의 다양한 부분을 쉽게 식별할 수 있다. 머리글, 바닥글, 주요 콘텐츠가 모두
시각적으로 즉시 눈에 들어온다.

하지만 시각 장애가 있거나 스크린 리더에 의존하는 사용자에게는 쉽지 않다. HTML 시맨틱 태그를 올바르게
사용하면 화면 리더가 콘텐츠를 더 정확하게 전달하기 때문에 이러한 독자가 콘텐츠를 더 잘 이해할 수 있다.

### SEO

시맨틱 HTML 태그는 태그 내에서 콘텐츠의 역할을 나타내므로 SEO(검색 엔진 최적화)에 중요하다.

이 정보를 통해 Googlebot과 같은 검색 엔진 크롤러는 콘텐츠를 더 잘 이해할 수 있다.
따라서 콘텐츠가 검색 엔진 결과 페이지(SERP)에서 관련 키워드에 대한 순위 후보로 선정될 가능성이
높아진다. 즉, 시맨틱 HTML이 올바르게 구현된 페이지는 그렇지 않은 페이지에 비해 SEO에서 유리하다.

**_[Site Audit](https://www.semrush.com/siteaudit/)_** 와 같은 도구를 사용하여 SEO에
영향을 미칠 수 있는 HTML 태그 문제를 찾을 수 있다.

![site-audit-1](/asset/site-audit-1.png)
![site-audit-2](/asset/site-audit-2.png)
![site-audit-3](/asset/site-audit-3.png)
![site-audit-4](/asset/site-audit-4.png)

## 시맨틱 태그의 두가지 범주

### 구조(레이아웃) 전달

레이아웃을 전달하는 태그는 아래와 같다.

- \<header>
- \<nav>
- \<main>
- \<article>
- \<section>
- \<area>
- \<aside>
- \<footer>

### 택스트 전달

- \<h1>: heading
- \<h2~h6>: subheading
- \<p>: paragraph
- \<a>: anchor
- \<ol>: ordered list
- \<ul>: unordered list
- \<q>: for long, multiline quotations
- \<blockquote>: for shorter, inline quotations
- \<em>: emphasis
- \<strong>: strong emphasis
- \<code>: A block of computer code.

그 외로

- b, summary, time, address, video, audio, img, legend, picture, button
  dl, dt, dd, caption..

## 참고

[Semantic HTML: What It Is and How to Use It Correctly](https://www.semrush.com/blog/semantic-html5-guide/)
