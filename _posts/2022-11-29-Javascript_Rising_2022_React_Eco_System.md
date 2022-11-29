---
title: "JavaScript 라이징스타 2021-React Eco System"
toc: true
toc_sticky: true
categories:

- dev

tags:

- javascript

---

안녕하세요. 2022년 JavaScript RisingStar 2022를 정리하기 전에 먼저 작년의  JavaScript RisingStart 2021을 먼저 소개하겠습니다.

이 포스트에서는 React Eco System의 top10 에 대해서 순위순으로 리서치 하였습니다.

![img1]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_1.png)

#Top 1

## Next.js
![img2]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_2.png)

*Next.js는 하이브리드 정적 및 서버 렌더링, TypeScript 지원, 스마트 번들링, 경로 미리 가져오기 등 프로덕션에 필요한 모든 기능을 통해 최고의 개발자 경험을 제공합니다. 구성이 필요하지 않습니다!*

### Next.js 장점

+ SEO(Search Engine Optimization, 검색엔진최적화)를 위한 Server-Side Rendering(SSR)을 가능하게 합니다.

+ 직관적인 페이지 기반 `라우팅` 시스템 <br>페이지 기반 라우팅 시스템 제공합니다.

+ 페이지간의 빠르고 매끄러운 전환을 위한 client-side navigation <br> \<Link />를 통해 페이지간의 빠르고 매끄러운 이동을 가능하게 합니다.

+ 코드 스프라이딩(Code Splitting, 코드분활) <br>번들을 여러 조각으로 조각내서 처음에 필요한 부분만 전송해주는 방식 → 어플리케이션 로드 타임을 줄여줍니다.

+ 내장 CSS 및 Sass 지원, CSS-in-JS 라이브러리 지원합니다.


### Nest.js 단점

- 서버 부하가 CSR에 비해서 많은 편입니다.

- 페이지 이동할때마다 새로운 html 파일을 불러오는 방식때문에 사용자는 화면이 깜빡거린다고 느낄 수 있습니다.

---

#TOP2

## Ant Design
![img3]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_5.png)<br>
*엔터프라이즈급 제품을 위한 디자인 시스템입니다. 효율적이고 즐거운 업무 경험을 만드십시오.*

알리바바 그룹(중국) 에서 개발한 UI 프레임워크입니다.

### Ant Design  장점

+ 쉬운 레이아웃 잡기 <br> 공식 홈페이지에서는 3분만에 레이아웃을 잡을 수 있다고 설명합니다..

+ 직관적이고 깔끔한 UX 컴포넌트 지원합니다.

---

# TOP3

## MUI, Meterial UI
![img4]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_6.png)<br>


*MUI는 새로운 기능을 더 빨리 제공하는 데 도움이 되는 포괄적인 UI 도구 모음을 제공합니다. 완전히 로드된 구성 요소 라이브러리인 Material UI로 시작하거나 자체 설계 시스템을 생산 준비 구성 요소로 가져오세요!*

### MUI  장점

+ 쉬운 사용법, 다양한 아이콘을 제공합니다.

+ 다양한 컴포넌트 제공, API 제공합니다.

+ 많은 사용자들이 이용하므로 레퍼런스가 다양합니다.

### MUI 단점

- styled-component 사용시 주의가 필요합니다.

- Ant Design 과 비교하여 비교적 학습 시간이 걸립니다.

---

# Top4.

## Remix

![img5]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_7.png)

*웹 표준 및 최신 웹 앱 UX 에 중점을 두고 더 나은 웹사이트를 구축할 수 있습니다 !*

### Remix 장점

+ 노드대신 Web Fetch API를 기반으로 구축되어 어디에서나 실행할 수 있습니다.

+ 서버리스 및 기존 node.js 환경을 지원합니다.

+ 더 나은 사용자 경험을 추구하 UX 강점이 있습니다.

+ 서버에서 데이터를 병렬로 로드하고 완전한 HTML 문서를 보내는 방식으로 훨씬 더 빠르고 버벅거림이 없습니다.

+ Nested Router를 사용하면 Remix가 앱 을 최대한 빠르게 만들 수 있습니다.

+ Remix는 사용자가 링크를 클릭하기 전에 모든 것을 병렬로 미리 가져올 수 있습니다.

+ 기본적으로 SSR을 지원합니다.

### Remix 단점

- reference 가 mui, ant design 에 비하여 상대적으로 적습니다.

---

# Top5
## React-use

Collection of essential React Hooks

필수 react hook 모음입니다. 

[https://streamich.github.io/react-use/?path=/story/components-usekey--demo](https://streamich.github.io/react-use/?path=/story/components-usekey--demo)

---

# Top6
# Chakra UI

![img6]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_8.png)

빠른 속도로 접근 가능한 React 앱 만들기, Chakra UI는 React 애플리케이션을 빌드하는 데 필요한 빌딩 블록을 제공하는 간단하고 모듈식이며 액세스 가능한 구성 요소 라이브러리입니다.

### Chakra UI 장점

+ WAI_ARIA 를 엄격히 준수하여 접근성이 훌륭하면서도 개발이 빠릅니다.

+ 다양한 테마 지원하여 desigh needs 에 맞게 컴포넌트 커스터마이징이 쉽습니다.

+ 컴포넌트 결합을 염두해두고 설계되 컴포넌트를 쉽게 구성 가능아합니다.

+ light, dark UI 지원합니다.


### Chakra UI 단점

- 공식 홈페이지에 컴포넌트별 예시가 적은 편입니다.

- Data Picker, Search 등의 기능이 없습니다.

- reference 가 mui, ant design 에 비하여 상대적으로 적습니다.

---

# Top7
## Headerless UI
![img7]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_9.png)

Tailwind CSS와 아름답게 통합되도록 설계된 완전히 스타일이 지정되지 않고 완전히 액세스 가능한 UI 구성 요소입니다.

Tainlwind css로 구현된 UI Component 모음입니다. 몇가지의 UI 컴포넌트를 지원합니다.


---
# Top9
## React Hook Form

![img8]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_12.png)

사용하기 쉬운 유효성 검사를 통해 성능이 뛰어나고 유연하며 확장 가능한 형식입니다.

### React Hook Form 장점

+ 직관적이고 완전한 기능을 갖춘 API 입니다.

+ HTML 마크업 표준을 지원합니다.

+ 재랜더링 횟수 최소화, 유효성 검사 계산 최소화 합니다.

+ 최고의 사용자 경험을 제공하기 위해 노력하고 일관된 검증 전략을 가져옵니다.

+ validation, error 를 처리해줍니다.

---

# Top10
## React Query

![img9]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_10.png)

****React를 위한 강력하고 성능 좋은 데이터 동기화,****

"전역 상태"를 건드리지 않고 React 및 React Native 애플리케이션에서 데이터를 가져오고, 캐시하고, 업데이트합니다.

### React Query장점

+ `서버 상태` 관리 라이브러리
  (redux-sata, redux-thunk 는 서버 데이터를 다루는 것에 적합하지 않다고 주장 → 클라리언트 상태 관리에 가깝다)

+ 클라이언트 상태와 서버 상태를 명확히 구분하기 위해서 만들어진 라이브러리입니다.

+ 비동기 작업과 관련된 대부분의 귀찮은 작업들을 간단한 코드로 안전하게 커리할 수 있습니다.

+ 캐싱을 지원합니다.

### React Query단점

- redux-saga 에 비해 커뮤니티 규모가 작아 레퍼런스가 상대적으 부족합니다.

---
# Top10
## Docusaurus

![img10]({{site.url}}/assets/images/csb/javascript_risingStart/rising_1_11.png)

페이스북 오픈 커뮤니티에서 관리하는 웹사이트 생성 도구입니다.

### Docusaurus 장점

+ MarkDown, MDX  형식으로 문서와 블로그 포스트를 쉽게 작성하고 웹 사이트로 퍼블리싱 가능

+ 리액트 기반, swizzling 으로 리액트 컴포넌트를 쉽게 커스터마이징 가능

---

reference

next.js

[https://velog.io/@syoung125/Next.js-기본-개념-1-Next.js-란-Next.js를-왜-사용할까-Next.js의-장점은](https://velog.io/@syoung125/Next.js-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-1-Next.js-%EB%9E%80-Next.js%EB%A5%BC-%EC%99%9C-%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C-Next.js%EC%9D%98-%EC%9E%A5%EC%A0%90%EC%9D%80)

ant design

[https://velog.io/@yellowish/리액트-UI라이브러리-앤트디자인Ant-Design-사용후기](https://velog.io/@yellowish/%EB%A6%AC%EC%95%A1%ED%8A%B8-UI%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%95%A4%ED%8A%B8%EB%94%94%EC%9E%90%EC%9D%B8Ant-Design-%EC%82%AC%EC%9A%A9%ED%9B%84%EA%B8%B0)

[https://jeonghwan-kim.github.io/2018/10/13/ant-design-101.html](https://jeonghwan-kim.github.io/2018/10/13/ant-design-101.html)

mui

[https://mui.com/](https://mui.com/)

[https://velog.io/@leeeunbin/TIL-React-UI-라이브러리-5가지](https://velog.io/@leeeunbin/TIL-React-UI-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-5%EA%B0%80%EC%A7%80)

remix

[https://yceffort.kr/2022/02/new-react-framework-remix](https://yceffort.kr/2022/02/new-react-framework-remix)

react-use

[https://github.com/streamich/react-use](https://github.com/streamich/react-use)
[https://chakra-ui.com/](https://chakra-ui.com/)

chakra UI

[https://velog.io/@leeeunbin/TIL-React-UI-라이브러리-5가지](https://velog.io/@leeeunbin/TIL-React-UI-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-5%EA%B0%80%EC%A7%80)

react hook form

[https://velog.io/@kihyun/React-Hook-Form-사용하기](https://velog.io/@kihyun/React-Hook-Form-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

react-query

[https://velog.io/@jkl1545/React-Query](https://velog.io/@jkl1545/React-Query)

[https://techblog.woowahan.com/6339/](https://techblog.woowahan.com/6339/)

[https://fe-developers.kakaoent.com/2022/220224-data-fetching-libs/](https://fe-developers.kakaoent.com/2022/220224-data-fetching-libs/)

[https://kyounghwan01.github.io/blog/React/react-query/basic/#사용하는-이유](https://kyounghwan01.github.io/blog/React/react-query/basic/#%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB-%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%B2)

[https://velog.io/@woohm402/react-query](https://velog.io/@woohm402/react-query)

docusaurus

[https://younho9.dev/docusaurus-manage-docs-1](https://younho9.dev/docusaurus-manage-docs-1)
