---
title: "JavaScript 라이징스타 2021-State Management"
toc: true 
toc_sticky: true 
categories:

- dev

tags:

- javascript

---

안녕하세요. 2022년 JavaScript RisingStar 2022를 정리하기 전에 먼저 작년의 JavaScript RisingStart 2021을 먼저 소개하겠습니다.

이 포스트에서는 많이 사용되는 redux 를 제외한 State Management 의 5가지에 대해서 리서치 하였습니다.

![img1]({{site.url}}/assets/images/csb/javascript_risingStart/rising_3_0.png)

---

## Zustand

![img2]({{site.url}}/assets/images/csb/javascript_risingStart/rising_3_1.png)

독일어로 `‘상태'`라는 뜻입니다ㅏ.

### Zustand 장점

+ 특정 라이브러리에 엮이지 않습니다. (그래도 React와 함께 쓸 수 있는 API는 기본적으로 제공합니다.)

+ 한 개의 중앙에 집중된 형식의 스토어 구조를 활용하면서, 상태를 정의하고 사용하는 방법이 단순합니다.

+ [Context API](https://ko.reactjs.org/docs/hooks-reference.html#usecontext)를 사용할 때와 달리 상태 변경 시 `불필요한 리랜더링을 일으키지 않도록 제어`하기 쉽습니다.

+ React에 직접적으로 의존하지 않기 때문에 자주 바뀌는 상태를 직접 제어할 수 있는 방법도 제공합니다. [(Transient Update라고 한다.)](https://github.com/pmndrs/zustand#transient-updates-for-often-occuring-state-changes)

+ 핵심 로직의 코드 줄 수가 약 42줄밖에 되지 않기 때문에 동작을 이해하기 위해 알아야 하는 코드 양이 아주 적습니다.  (VanillaJS 기준)

---

## XState

![img3]({{site.url}}/assets/images/csb/javascript_risingStart/rising_3_2.png)

상태 머신, 상태차트의 개념을 활용하여 이벤트 드리븐, 선언적 상태 관리를 실현하는 라이브러리입니다.

### XState장점

+ 상태 차트 시각화, 상태 기반 테스트 자동 생성(Model Based Testing)이라는 장점이 있습니다.

+ 전역 스토어를 사용하거나 전체 프로그램을 Provider로 래핑할 필요가 없습니다 (redux, context 와 다릅니다)

---

## Jotai

![img4]({{site.url}}/assets/images/csb/javascript_risingStart/rising_3_3.png)

Jotai is pronounced "joe-tie" and means "state" in Japanese.

Context의 re-rendering 문제를 해결하기 위해 만들어진 React 특화 상태관리 라이브러리입니다.

### Jotai 장점

+ Minimalistic API

+ No string keys

+ TypeScript oriented

---

## recoil

![img5]({{site.url}}/assets/images/csb/javascript_risingStart/rising_3_4.png)

*A state management library `for React`*

### Recoil 장점

+ redux 에 비하면 매주 적은 러닝 커브가 발생합니다.

+ 컴포넌트가 사용하는 데이터 조각만 사용할 수 있고, 계산된 selector를 선언할 수 있으며, 비동기 데이터 흐름을 위한 내장 솔루션까지 제공합니다.

### Recoil 단점

- 아직 미흡한 DevTools, 그리고 Snapshot

---

## Mobx

![img6]({{site.url}}/assets/images/csb/javascript_risingStart/rising_3_5.png)

*Simple, scalable state management.*

### Mobx 장점

+ redux 에 비하면 매주 적은 러닝 커브

+ 객체지향적

+ Decorator 데코레이터 제공하여 보일러플레이트 코드가 사라짐

+ 불변성 유지를 위한 노력이 불필요합니다.

+ Mobx Configuration 설정으로 State를 오직 메소드를 통하여 변경할 수 있도록 Private하게 관리 할 수 있습니다.

---

reference

**zustand**

[https://ui.toast.com/weekly-pick/ko_20210812](https://ui.toast.com/weekly-pick/ko_20210812)

[https://velog.io/@giyeon/React-js-상태관리-Zustand](https://velog.io/@giyeon/React-js-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-Zustand)

[https://ridicorp.com/story/how-to-use-redux-in-ridi/](https://ridicorp.com/story/how-to-use-redux-in-ridi/)

**xState**

[https://itchallenger.tistory.com/484?category=1095223](https://itchallenger.tistory.com/484?category=1095223)

[http://blog.hwahae.co.kr/all/tech/tech-tech/6707/](http://blog.hwahae.co.kr/all/tech/tech-tech/6707/)

[https://xstate.js.org/docs/recipes/react.html](https://xstate.js.org/docs/recipes/react.html)

**jotai**

[http://blog.hwahae.co.kr/all/tech/tech-tech/6099/](http://blog.hwahae.co.kr/all/tech/tech-tech/6099/)

[https://jotai.org/docs/basics/comparison](https://jotai.org/docs/basics/comparison)

[https://velog.io/@ggong/상태-관리를-위한-라이브러리-jotai](https://velog.io/@ggong/%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC%EB%A5%BC-%EC%9C%84%ED%95%9C-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-jotai)

[https://programming119.tistory.com/263](https://programming119.tistory.com/263)

**recoil**

[https://velog.io/@juno7803/Recoil-Recoil-200-활용하기](https://velog.io/@juno7803/Recoil-Recoil-200-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)

[https://ui.toast.com/weekly-pick/ko_20200616](https://ui.toast.com/weekly-pick/ko_20200616)

Mobx

[https://www.howdy-mj.me/mobx/mobx6-intro/](https://www.howdy-mj.me/mobx/mobx6-intro/)

[https://techblog.woowahan.com/2599/](https://techblog.woowahan.com/2599/)