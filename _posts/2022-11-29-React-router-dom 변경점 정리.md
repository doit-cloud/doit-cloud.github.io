---
title: "React-router-dom 변경사항 정리"
toc: true
toc_sticky: true
categories: 

- dev

tags:

- React
- react-router-dom

---



# 0. 개요

해당 문서는 **React Router v6**의 변경점에 대해 정리합니다

(**주의**!)  **React Router v6**는 **React 16.8** 이상과 호환 됩니다.

# 1. Switch  ⇒  Routes

 

매칭 방식을 관리해주는 `Switch`가 `Routes`로 변경되었습니다. **변경 사항**은 다음과 같습니다.

**1) `path`가 정확하게 일치할 때만 매칭 시켜주던 `exact` 옵션이 삭제되었습니다.**    
 
정확히는, `exact`가 기본 적용되기 때문에 **중첩 라우팅**을 위해서는 후술할 다른 방식을 사용해야 합니다.


**2) `<Route>`에 컴포넌트를 연결할 때 `component`, `render` 대신 `element`를 사용합니다.**  
  
v5에서는 `<Route>`에서 `props`를 전달할 때, `component`내에서 **arrowFunction**을 사용하거나       
`render` 속성을 사용해야 했지만, `element`는 직접 전달이 가능하기에 더 직관적으로 되었습니다!


```jsx
v5

<Route path="/" component={Main} />
<Route path="/Page1" component={() => <Page1 data={data}/>} />  //props 전달
<Route path="/Page2" render={() => <Page2 data={data} />}/>  //render
```


```jsx
v6

<Route path="/" element={<Main />} />
<Route path="/Page1" element={<Page1 data={data} />} />   //props 전달
```

**3) `<Route>`는 `<Routes>`의 직속 자식이어야만 합니다.**

`<Switch>`의 자식일 필요는 없었던 v5와 달리,         
v6에서는 `Route`를 사용하려면 반드시 `Routes`로 감싸주어야 합니다.

**4) `useRouteMatch`가 사라지고 `path`의 상대 경로를 쓸 수 있게 되었습니다**

기존의 `path="/Page:id"` 에서 `path=":id"` 로 **상대경로** 지정,        
즉 현재 매치된 **Route**의 경로를 사용 할 수 있게되었습니다.         
이 외에도, `path="."` / `path=".."` 등으로 상대경로를 표현합니다.


```jsx
v6

<Route path="/" element={<Main/>} >
	<Route path=":id" element={<Page1/>} />
<Route />
```

# **2. 중첩 라우팅**

앞에서 언급했듯 중첩 라우팅의 방식이 변경되었습니다.       
중첩에서 제외할 Router에  `exact={true}`  컴포넌트를 쓰던 방식에서     
중첩 하고자하는 **부모 Router**의 `path`에  `/*`를 붙여주는 것으로 사용합니다.

자세한 방법은 2가지로 다음과 같습니다.

**1) `<Outlet>` 컴포넌트 사용**

`<Router>`들을 한곳에 모아놓고, 중첩 라우팅이 필요한 컴포넌트에서 `<Outlet>` 를 사용합니다.       
라우팅 요소들을  한곳으로 모아놓기 때문에 구조가 직관적이고 알아보기 좋습니다.



```jsx
Router.js

...

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="web/*" element={<Web />}>
          <Route path=":id" element={<WebPost />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```


```jsx
Web.js

...

const Web = () => {
  return (
    <div>
      <h1>This is Web</h1>
      <Outlet />
    </div>
  );
};

export default Web;
```

**2) 부모 Router에 직접 기재**

상대 경로를 사용함으로써 `Outlet` 없이 곧바로 `Routes` , `Route`로 기재할 수 있습니다.       
기존의 방식과 크게 다르지 않습니다.


```jsx
Router.js


...

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="web/*" element={<Web />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```


```jsx
Web.js

...

const Web = () => {
  return (
    <div>
      <h1>This is Web</h1>
      <Routes>
        <Route path=":id" element={<WebPost />} />
      </Routes>
    </div>
  );
};

export default Web;
```

# 3. Hook

**React Router v6**에서는 많은 기능들이 React Hook으로 전환되었습니다.

**1) location  ⇒  `useLocation`** 

`useLocation`은  기존에 `Router`의 `defaultProps`로 받아오던 `Location` 을 대체합니다.      
받아 올 수 있는 데이터들은 다음과 같습니다.

- `hash`: 현재 페이지의 hash 값
- `pathname`: 현재 페이지의 경로
- `search`: 현재 페이지의 hash 값

```jsx
const { hash, pathname, search } = useLocation();
```

**2) match.parms  ⇒  `useParams`**

`useparams`는 기존의 `Router`의 `defaultProps`로 받아오던 `match.Params`를 대체합니다.      
URL 매개변수를 받아올 수 있으며, 이를 통한 동적 라우팅을 구현할 수 있습니다.

**3) history  ⇒  useHistory  ⇒  `useNavigate`**

`useNavigate`는 이전의 `useHistory`를 대체합니다. (React.suspense와의 연계를 위해 등장)        
**Page** 이동을 가능하게 하며, 이는 `Link`와 마찬가지로 **history stack**에 반영됩니다.        
**replace** 또한 계속해서 지원하며, 이는 **history stack** 가장 위에 쌓는 **push**와는 달리       
**TOP**를 자신으로 대체합니다.

마지막으로 **history stack**에 가고자하는 **index**를 전달함으로서 해당 페이지로 이동 할 수 있습니다.

```jsx
...

const navigate = useNavigate();

function handleClick() {

    navigate("/home");  //Home으로 이동
		navigate("../Page1");  //상대경로 이동	
		navigate("/home", {replace: true});  //이동과 함께 history stack의 top를 대체

		navigate(-1);
		navigate(2);  //index를 넣음으로 해당 history stack 위치 page로 이동

  }
```

**1) react-router-config  ⇒   `useRoutes`**

`useRoutes`이 **react-router-config**를 대체합니다.        
별도의 패키지 설치를 요구하던 기존과 달리 Hook을 이용해 routes를 구성할 수 있게 되었습니다.

```jsx
function App() {
  const element = useRoutes([
    { path: "/", element: <Home /> },
    { path: "detail", element: <Detail /> },
    {
      path: "auth",
      element: <Auth />,
      children: [
        { path: ":signup", element: <Signup /> },
        { path: "login", element: <Login /> },
      ],
    },
    { path: "*", element: <Notfound /> },
  ]);

  return element;
}
```

# 4. 기타 변경사항

**NavLink**

- `<NavLink exact>`  ⇒   `<NavLink end>`
- `activeClassName`, `activeStyle`  ⇒  `className`, `style`

참고

---

[https://whales.tistory.com/140](https://whales.tistory.com/140)

[https://velog.io/@peacesong/react-router-v6](https://velog.io/@peacesong/react-router-v6)