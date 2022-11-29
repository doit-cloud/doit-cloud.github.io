---
title: "JavaScript 라이징스타 2021-CSS in Javascript"
toc: true
toc_sticky: true
categories:

- dev

tags:

- javascript
- css in javascript

안녕하세요. 2022년 JavaScript RisingStar 2022를 정리하기 전에 먼저 작년의  JavaScript RisingStart 2021을 먼저 소개하겠습니다.

이 포스트에서는 CSS in Javascript의 top6 에 대해서 순위순으로 리서치 하였습니다.

![img1]({{site.url}}/assets/images/csb/javascript_risingStart/rising_2_0.png)

---

# TOP1
## Vanilla-extract

![img2]({{site.url}}/assets/images/csb/javascript_risingStart/rising_2_1.png)
*Zero-runtime Stylesheets in `TypeScript`*

TypeScript를 전처리기로 사용하십시오. 유형이 안전한 로컬 범위의 클래스, 변수 및 테마를 작성한 다음 빌드 시 정적 CSS 파일을 생성합니다.

### Vanilla-extract 장점

+ Type-Safe한 정적 CSS → 모든 스타일은 빌드 시 생성됩니다. (sass, less 와 같지만 `TypeScript`에 강력함)

+ TypeScript로 Class Style을 만들 수 있는 CSS 프레임워크입니다.

+ webpack, esbuild, Vite 및 Next.js에 대한 공식 통합이 제공됩니다.

### Vanilla-extract 담점

- 한국어 reference 부족합니다.


# TOP2
## styled Components

![img3]({{site.url}}/assets/images/csb/javascript_risingStart/rising_2_2.png)

*ES6 및 CSS의 장점을 사용하여 스트레스 없이 앱 스타일을 지정하세요*

### styled Components 장점

+ component 단위 스타일링 → selector 우선 순위 적용때문에 일어나는 css 혼선 방지합니다.

+ 조건부 스타일링 → Component의 props를 전달받아 사용하는 것이 가능합니다.

+ 확장 스타일링 → 기존의 Component에 스타일 추가 가능

  → 중복 코드 량이 줄어들어 분산된 스타일 일관적으로 관리 가능하며, 유지보수 비용 절감됩니다.

+ sass 의 중첩 스코프 규칙 사용 가능합니다.

### styled Components 단점

- 별도의 라이브러리 설치에 따른 번들 크기 증대

- CSS-in-CSS에 비해 느린 속도


# TOP3

## Stitches

![img4]({{site.url}}/assets/images/csb/javascript_risingStart/rising_2_3.png)

Near Zero Runtime, SSR, 다중 변형 지원 및 동급 최고의 개발자 경험을 제공하는 CSS-in-JS.

### Stitches 장점

+ 런타임 시 불필요한 소품 보간을 방지하여 다른 스타일 라이브러리보다 성능이 우수합니다.

+ 전용 React 라이브러리가 있지만 Vue, Svelte, 바닐라 HTML을 포함한 모든 프레임워크에서 작동합니다.

+ 테마, 스마트 토큰, 소품, 소품, 유틸리티 및 완전한 형식의 API와 같은 유용한 기능 제공합니다.

+ SSR 지원합니다.

### Stitches 단점

- reference 부족합니다.


# TOP4
## Twin

![img4]({{site.url}}/assets/images/csb/javascript_risingStart/rising_2_4.png)
*Twin은 Tailwind와 빌드시 css-in-js의 유연성을 결합합니다!*

# TOP5
## Emotion

![img5]({{site.url}}/assets/images/csb/javascript_risingStart/rising_2_5.png)
*소스 맵, 레이블 및 테스트 유틸리티와 같은 기능을 통해 뛰어난 개발자 경험과 함께 강력하고 예측 가능한 스타일 구성을 제공합니다. 문자열 및 개체 스타일이 모두 지원됩니다.*

### Emotion 장점

+ css-in-js 형식으로 스타일 사용 가능합니다.

+ className이 자동으로 부여됨 → 스타일이 겹칠 염려가 없습니다.
  
+ 재사용이 가능합니다.

+ props 조건 등에 따라 스타일을 지정 가능합니다.

+ vendor-prefixing, nested selectors, and media queries 등이 적용되어 작성이 간편합니다.(line style로 작성)

+ styled component 사용방식과 css prop 기능을 지원하여 확장에 용이합니다.

+ styled component보다 파일 사이즈가 작고, ssr시 서버 작업이 필요없습니다.

### Emotion 단점

- 파일마다 `/** @jsx jsx */` 라는 [JSX Pragma](https://emotion.sh/docs/css-prop#jsx-pragma)를 작성해야하는데 이를 설정하기 귀찮을 수 있습니다.

---
# TOP6
## Linaria

![img6]({{site.url}}/assets/images/csb/javascript_risingStart/rising_2_6.png)
*Zero-Runtime CSS in JS*

### Linaria 장점

+ css-in-js 형식으로 스타일 사용 가능합니다.

+ JS로 CSS를 작성하지만 **런타임이 0** 인 경우 CSS는 빌드 중에 CSS 파일로 추출됩니다.

+ 중첩과 같은 Sass를 사용한 친숙한 **CSS 구문**을 사용합니다.

+ **React 바인딩과 함께 동적 소품 기반 스타일**사용 , 장면 뒤에서 CSS 변수 사용합니다.

+ **CSS sourcemaps** 으로 스타일이 정의된 위치를 쉽게 찾을 수 있습니다.

+  [stylelint](https://github.com/stylelint/stylelint) 를 사용하여 JS에서 **CSS 린트**

+ **논리에 JavaScript를** 사용 하고 CSS 전처리기가 필요하지 않습니다.

+ 선택적 으로 Sass 또는 PostCSS와 같은 **CSS 전처리기를 사용합니다.**

### Linaria 단점

- css변수를 사용한 방식이라 브라우저 호환성에 문제가 있습니다.(IE 11에서는 CSS 지원하지 않는다)

- Babel & Webpack을 사용하다보니까 직접 webpack 설정을 변경해줘야 한다. 만약 CRA를 사용하고 있다면 eject를 해주고 loader를 설치해서 설정을 해줘야 하니 약간 번거로운 작업이 있습니다.
---

reference

**vanilla-extract**

[https://vanilla-extract.style/](https://vanilla-extract.style/)

[https://ui-box.tistory.com/107](https://ui-box.tistory.com/107)

[https://css-tricks.com/css-in-typescript-with-vanilla-extract/](https://css-tricks.com/css-in-typescript-with-vanilla-extract/)

[https://www.thisdot.co/blog/introduction-to-vanilla-extract-for-css](https://www.thisdot.co/blog/introduction-to-vanilla-extract-for-css)

**styled-components**

[https://dkje.github.io/2020/10/13/StyledComponents/](https://dkje.github.io/2020/10/13/StyledComponents/)

[https://kyounghwan01.github.io/blog/React/styled-components/basic/#styled-components란](https://kyounghwan01.github.io/blog/React/styled-components/basic/#styled-components%E1%84%85%E1%85%A1%E1%86%AB)

[https://velog.io/@gur0601/Styled-components와-SCSS-차이](https://velog.io/@gur0601/Styled-components%EC%99%80-SCSS-%EC%B0%A8%EC%9D%B4)

**Stitches**

[https://stitches.dev/docs/tutorials](https://stitches.dev/docs/tutorials)

[https://stitches.dev/blog/migrating-from-styled-components-to-stitches](https://stitches.dev/blog/migrating-from-styled-components-to-stitches)

**Twin**

**emotion**

[https://velog.io/@gytlr01/emotion-정리](https://velog.io/@gytlr01/emotion-%EC%A0%95%EB%A6%AC)

[https://brunch.co.kr/@kmongdev/17](https://brunch.co.kr/@kmongdev/17)

[https://velog.io/@bepyan/styled-components-과-emotion-도대체-차이가-뭔가](https://velog.io/@bepyan/styled-components-%EA%B3%BC-emotion-%EB%8F%84%EB%8C%80%EC%B2%B4-%EC%B0%A8%EC%9D%B4%EA%B0%80-%EB%AD%94%EA%B0%80)

Linaria

[https://velog.io/@hseoy/Zero-Runtime-CSS-in-JS-feat.-Linaria](https://velog.io/@hseoy/Zero-Runtime-CSS-in-JS-feat.-Linaria)

[https://velog.io/@asdhugh1/Styling-with-Linaria](https://velog.io/@asdhugh1/Styling-with-Linaria)