---
title: "ReactQuery와 TypeScript"
toc: true
toc_sticky: true
categories: 

- dev

tags:

- TypeScript
- ReactQuery
- React

---



React Query는 정말 좋은 상태 관리 라이브러리입니다.  
서버 데이터의 상태관리에 대해서는 Redux를 대체하기에 전혀 모자람이 없죠. 

물론 출시된지 얼마되지 않는 여느 라이브러리들이 그렇듯,     
관련 커뮤니티의 규모와 자료들은 아직 부족한 편입니다.

기본적으로 사용법이 쉬운편이라  문제 될 정도는 아니지만, **TypeScript**와 함께하면 얘기가 좀 달라집니다.  
모호한데다가 자료까지 부족하거든요.  with TypeScript는 언제나 공부가 필요한것 같습니다.
 
실무에서 사용하면서 애매했던 점을 정리해봤습니다.

## 1. useQuery

React Query에서  통신을 위해 대표적으로 사용되는 Hook은 2가지 입니다.     
useQuery는 데이터 조회를 위해 사용되고,  
useMutation는 데이터를 생성 / 업데이트 / 삭제 할 때 사용됩니다.

각 Hook의 Type과 사용법에 대해 알아보겠습니다.

---

### (1) **useQuery의 Type**

useQuery의 **제네릭** 타입은 다음과 같이 선언되었습니다.

```tsx
export function useQuery<
  TQueryFnData = unknown,
  TError = unknown,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey
>
```
<br/>

1. **TQueryFnData**

    ```tsx
    const { data } = useQuery<AxiosResponse<결과타입>>(
    ...
    ```

    `queryFuntion`의 실행 결과의 반환 타입을 명시적으로 지정해주는 제네릭 타입입니다.        
    서버 통신 이후  `Promise`의 반환 타입이라고 생각하면 됩니다.

    예를 들어 axios를 이용해 서버에 API를 요청하고 있다면,             
    타입 지정은 `<AxiosResponse>`가 될 것 입니다.  
    물론 `<AxiosResponse<Type>>`의 형태로 반환 타입을 지정하는 것도 가능합니다.

    개인적으로는 해당 타입을 제네릭으로 지정하여, 어느정도 형식을 유지하면서 반환 타입을 지정해주는 것이 좋은 사용 방법이라 생각합니다. 

    해당 타입은 option인 `onSuccess`의 매개 변수인 `response`와,         
    반환 값인 `data`에서 사용하실 수 있습니다.

    <br/>

2. **TError**
    
    ```tsx
    const { error } = 
    useQuery<AxiosResponse<결과타입>, AxiosError<에러타입>>(
    ...
    ```
    
    오류 발생시 Error의 반환 타입을 명시적으로 지정해주는 제네릭 타입입니다.     
    단순히 Error로 지정해도 무방하지만, status 같이 디테일한 데이터를 원한다면     
    통신 라이브러리가 제공하는 반환 타입을 사용하는 것이 좋습니다.
    
    예를 들어 axios를 이용해 서버에 API를 요청하고 있다면,
    타입 지정은 `<AxiosError>`가 될 것 입니다.  `TQueryFnData`처럼 Error의 반환 타입을 지정하는 것도 가능합니다.
    
    해당 타입은 option인 `onError`의 매개 변수인 `error`와,      
    반환 값인 `error`에서 사용하실 수 있습니다.
    
    <br/>
    
3. **TData**

    ```tsx
    const { data } = 
    useQuery<AxiosResponse<결과타입>, AxiosError<에러타입>, 반환타입>(
    ...
    ```

    data의 **실질적** 반환 타입을 명시적으로 지정해주는 제네릭 타입입니다.     
    `TQueryFnData`는 반환 직후의 타입이지만,       
    `TData`는 반환 후 select option으로 가공 뒤의 타입입니다.      
    
    다시말해  반환된 데이터를 가공하지않는다면,  타입은  `TQueryFnData`과 동일합니다.

    <br/>

4. ****TQueryKey****

    ```tsx
    useQuery<AxiosResponse<결과타입>, AxiosError<에러타입>, 반환타입, 쿼리키타입>(
    ...
    ```

    useQuery의 첫 번째 인자인,  `queryKey`의 타입을 명시적으로 지정해주는 제네릭 타입입니다.     
    Query Function의 매개변수로 반환 받는 `queryKey`의 타입에도 영향을 줍니다.

    뒤에서 좀 더 자세히 설명하겠지만, `queryKey`는 각 query의 구분을 위해 사용됩니다.      
    대체로 지정되는 타입은 string, Array<string>과 Object입니다.

    `queryKey`는 계층구조이기 때문에 이를 활용하기 위해서  Array<string>로 사용하는게 좋지만,  하나의 객체를 key로 사용하는 것도 나쁘지 않습니다.



---



설명을 바탕으로 작성한  TypeScript에서 useQuery의 기본적인 사용법은 다음과 같습니다.

```tsx
useQuery<AxiosResponse, AxiosError, AxiosResponse, string[]>(
    ['keyName'], //queryKey
    (): Promise<any> => API() //queryFn
    {
        options, //options
    },
),
```

해당 형태로 사용해도 무방하지만, Hook이나 Service 형식으로 분리해서 사용하는 것이 깔끔합니다.



### (2) **useQuery의 데이터 갱신**

다음은 데이터 갱신입니다.          
useQuery는 데이터 갱신의 방법도 다양하고 역할도 서로 달라 따로 설명할 필요성을 느꼈습니다.

useQuery는 기본적으로 호출되는 순간, 한번 쿼리를 실행합니다.  
다시 말해 데이터를 갱신하여 화면에 반영하기 위해서는 쿼리의 요청이 필요합니다.

방법은 크게 세가지로 나눠집니다.

<br/>

1. **refetchInterval**

    ```tsx
    ...
	    {
			    refetchInterval: 500 //500ms마다 요청

	    }, //options
    ...
    ```

    쿼리가 일정 주기마다 실행되도록하는 `refechInterval` 옵션을 사용합니다.     
    `setInterval`과 `refetch` 함수를 합쳐놓은 느낌이죠. 

    지속적인 요청과 갱신이 필요할 때 효과적입니다.

    단점으로는 사용자가 임의로 갱신 시점을 조정할 수 없고,     
    계속해서 서버에 요청을 날리기에 효율적이지도 않습니다.

    <br/>

1. **refetch**

    ```tsx
    const {refetch} = 
    useQuery<AxiosResponse, AxiosError, AxiosResponse, string[]>(
    ...
    ```

    일정 간격으로 update를 하지 않고 특정 사용자 액션에 대한 응답으로 쿼리 결과를 갱신합니다. 사용자가 원하는 타이밍에 갱신 가능합니다.

    단점으로는 데이터를 변형할 때 그것을 위한  코드가 선행되어야 합니다. 하지만  update 반영도 제대로되지 않을 뿐더러 코드가 아래와 같은 형태가 되어버려 보기에도 좋지 않습니다.

    ```tsx
    갱신();
    refetch();
    ```

    근본적으로 refetching 작업에 대해 명령형으로 실행하고자 하는건 잘못된 방법이기에,      
    다음에 나올 방법과 같이 사용하는 것이 좋습니다.

    <br/>
    
1. **uniqueKey**
    
    ```tsx
    const [queryString, setQueryString] = useState<string>('');
    
    useQuery<AxiosResponse, AxiosError, AxiosResponse, string[]>(
    	['keyName', queryString], //queryKey
    ...
    ```
    
    앞서 말한대로 unique Key는 계층 구조입니다.       
    또한 useQuery는 Key가 변경 될 때 마다 트리거 되어 자동으로 `refetching`하기 때문에,     
    데이터를 변경하는 state를 key에 저장하기만 하면 됩니다. 모범적인 사용법이라 할 수 있죠.

    그렇다곤 해도 데이터를 변형하지 않으면서 갱신해야 하는 경우도 있기 때문에,  
    `refetch`와 같이 사용하는 것이 좋습니다.

    마지막으로 아까 말했듯 useQuery는 조회에 사용되는 경우가 많기 때문에,     
    대체로 queryString으로 queryKey를 구성하는 편입니다.

    <br/>

1. **결론**

    데이터가 변형되는 경우에는 uniqueKey에 등록해놓은 state를 변경하고,        
    변형되지 않는 경우에는 `refetch`를 사용하는 것이 좋습니다.

---

## 2. useMutation

 useMutation은 주로 추가/삭제/수정 요청에 주로 사용됩니다.

물론 useQuery에는 무조건 get method를 사용하고  
useMutation에는 post/put/delete method를 사용하는 것은 아니지만      

대체로 useMutation은 bodyData를 필요로 할때가 많고,  
반대로 queryString을 사용하는 경우가 적기 때문에, 그에 따른 타입과 사용법이 조금씩 다릅니다.

---

### (1) useMutaion**의 Type**

useMutaion의 타입은 아래와 같이 선언되어 있습니다.

```tsx
export function useMutaion<
  TData = unknown,
  TError = unknown,
  TVariables = void,
  TContext = unknown
>
```

<br/>

1. **TData**

    ```tsx
    const { data } = 
    useMutaion<AxiosResponse<결과타입>>(
    ...
    ```

    useMuation에 넘겨준 mutation 함수의  반환 타입을 명시적으로 지정해주는 제네릭 타입입니다.         
    useMuation에는 TQueryFnData이 없기 때문에 TData가 실행 결과 타입이자, 실질적인 반환 타입이 되는 것이죠.

    useQeury의 TQueryFuntion와 유사한 부분이 많기 때문에, 자세한 설명은 생략하겠습니다.

    <br/>
    
1. **TError**

    ```tsx
    const { error } = 
    useMutation<AxiosResponse<결과타입>, AxiosError<에러타입>>(
    ...
    ```

    useMutation에 넘겨준 mutation 함수의 에러 결과 타입을 명시적으로 지정해주는 제네릭 타입입니다.

    useQeury의 TError와 유사한 부분이 많기 때문에 자세한 설명은 생략하겠습니다.

    <br/>
1. **TVariables**

    ```tsx
    const { mutate } = 
    useMutation<AxiosResponse<결과타입>, AxiosError<에러타입>, bodyType>(
    ...
    (bodyData: bodyType): Promise<any> => API(), //mutationFn
       {   //options

           onSuccess: (res, bodyData: bodyType) => {},
           onError: (err, bodyData: bodyType) => {},
           onSettled: (res, err, bodyData: bodyType) => {},
       },
    );

    mutate(bodyData);
    ```

    `mutate` 함수에 전달할 인자를 지정하는 제네릭 타입입니다.    
    쉽게 말해 bodyData에 해당한다고 할 수 있죠.

    해당 타입은 `mutate`와 Callback 함수인, `onSuccess`, `onError`의 두번째 인자   
    `onSettled`의 세번째 인자로 사용 할 수 있습니다.
    
    <br/>
    
1. ****TContext****
    
    ```tsx
    const { mutate } = 
    useMutation<AxiosResponse, AxiosError, bodyType, callbackType>(
    ...
   
       {   //options   
           onMutate: bodyData => something(bodyData),   //callbackType
    
           onSuccess: (res, bodyData, callbackType) => {},
           onError: (err, bodyData, callbackType) => {},
           onSettled: (res, err, bodyData, callbackType) => {},
       },
    );
    ```
    
    TContext는 앞서 말했던 callback 함수들의 마지막 인자의 타입이자,    
    `onMutate`의 결과 값의 타입을 지정하는 제네릭 타입입니다.
    
    솔직히 해당 타입은 저도 제대로 사용해본 적이 적어 어떤 용도로 사용되는지는 정확히 알지 못하기에,         
    더 설명 드릴점은 없습니다.
    

---

설명을 바탕으로 작성한  TypeScript에서 useMutation의 기본적인 사용법은 다음과 같습니다.

```tsx
useMutation<AxiosResponse, AxiosError, bodyType, callbackType>(
['keyName'], //mutationKey
  (bodyData: bodyType): Promise<any> => API() //mutationFn
    {
        onMutate: bodyData => something(bodyData),   //callbackType

        onSuccess: (res, bodyData, callbackType) => {},
        onError: (err, bodyData, callbackType) => {},
        onSettled: (res, err, bodyData, callbackType) => {},
	}, //options
),
```

또한 데이터 갱신에 대해 첨언하자면 useMutation도 마찬가지로 queryKey를 통한 데이터 갱신이 가능하지만,  
기본적으로  mutate를 통해 쿼리를 호출하는 것이 바람직 하다고 볼 수 있습니다.

---

## 결론

react-query는 타입 지정을 통해서 꽤나 안정적으로 사용할 수 있었습니다.

타입 가드를 도입하기 보다는 이처럼 빡빡한 타입 지정을 해두는 것이,          
더 비동기 상태들의 타입 안정성을 확보하는데 도움이 될 수 있지 않을까 싶네요

<br/>

참고

**react-query에 typescript 적용하기 - 리액트 쿼리, 타입스크립트**

[https://gusrb3164.github.io/web/2022/01/23/react-query-typescript/](https://gusrb3164.github.io/web/2022/01/23/react-query-typescript/)

****React Query Key 관리****

[https://www.zigae.com/react-query-key/](https://www.zigae.com/react-query-key/)