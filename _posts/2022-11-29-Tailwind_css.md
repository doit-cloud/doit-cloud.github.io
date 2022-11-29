---
title: "Tailwind CSS"
toc: true
toc_sticky: true
categories:

[//]: # (이 라인을 거거 하고 아래 중 하나 선택)
- dev

tags:

- css
- tailwind
- tailwind CSS

---

tailwind CSS? <br>
*HTML을 벗어나지 않고도 최신 웹사이트를 빠르게 구축할 수 있습니다 !*

> *A utility-first CSS framework packed with classes like **`flex`**, **`pt-4`**, **`text-center`** and **`rotate-90`** that can be composed to build any design, directly in your markup.*
>

프로젝트에 적용하기 전에 CSS Framework를 Research 했습니다.

이 문서에서는 Tailwind의 장점과 사용법에 대하여 소개하려고 합니다. 이 이야기를 하기 전에 먼저 기존 CSS 의 문제점에 대하여 언급하고 넘어가도록 하겠습니다.

## 기존 CSS 의 문제점

+ 우선순위때문에 발생하는 복잡한 선택자, 깊어지는 깊이(ex ul > li > a > ul > li).

+ *!important* 의 남발

+ 전역 Scope 를 사용하기에 때문 class naming 에 대해 많은 시간 소모됩니다.

## Tailwind 장점

* utillity-first 개념으로 만들어진 CSS framework 입니다.

* Class 이름을 고민하지 않아도 됩니다.

* 쉬운 반응형 페이지와 커스터마이징이 가능하다.

* 프로덕션용으로 빌드할 때 사용되지 않는 모든 CSS를 자동으로 제거하기 떄문에 최종 CSS 번들이 가능한 한 가장 작다.

## Tailwind 단점

* 코드의 가독성이 나쁩니다. 많은 스타일 요소를 사용할 경우 class 이름이 길어집니다.<br>
  (className="text-3xl font-bold underline")

* IE 를 지원하지 않습니. (version 1.9 는 지원함)

## Tailwind 설치법

- 설치

  1️⃣   Tailwind CSS 설치

  CRA를 통해

      yarn 을 통해 설치한 다음 init 명령을 실행하여 ‘tailwind.config.js’ 및 ‘postcss.config.js’ 파일을 만든다

    ```jsx
    yarn add -d tailwindcss postcss autoprefixer
    yarn tailwindcss init -p
    ```

  2️⃣   탬플릿 경로 구성

  파일의 모든 템플릿 파일데 대한 경로를 추가한다.  ‘tailwind.config.js’

    ```jsx
    module.exports = {
      content: [
        "./src/**/*.{js,jsx,ts,tsx}",
      ],
      theme: {
        extend: {},
      },
      plugins: [],
    }
    ```

  3️⃣  CSS에 Tailwind 지시문 추가

  @tailwind Tailwind의 각 레이어에 대한 지시문을 파일에 추가 합니다. ./src/index.css

    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    
    or
    
    @import "tailwindcss/base";
    @import "tailwindcss/components";
    @import "tailwindcss/utilities";
    ```

  IntelliJ IDEA > Preference > Plugins > Tailwind CSS (JetBrains) 다운받으면 자동완성을 지원한다.

  4️⃣   프로젝트에서 Tailwind 사용 시작

  Tailwind의 유틸리티 클래스를 사용하여 콘텐츠 스타일을 지정하세요.

    ```jsx
    export default function App() {
      return (
        <h1 className="text-3xl font-bold underline">
          Hello world!
        </h1>
      )
    }
    ```
## Tailwind 사용 후기
class naming을 신경쓰지 않아도 되서 스타일 구현이 빠르며, 팀원들 모두 tailwind css 를 만족하며 사용하고 있습니다. <br> 하지만 IE를 지원하지 않는 단점 아닌닌 단점 때문에 IE를 고려햐애 하는 팀에서는ㄴ 사용이 어려울 수 있겠다는 생각이 들었습니다.

