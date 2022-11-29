---
title: "Typescript + ReactQuery + Axios 에서의 토큰처리"
toc: true
toc_sticky: true
categories: 

- dev

tags:

- TypeScript
- ReactQuery
- Axios

---



Axios를 사용해 오셨던 분들은 JWT 토큰을 관리함에 있어 axios가 제공하는 interceptors를 활용하였을 것입니다. 저 또한 그렇고요.     
하지만 React Query로 상태 관리 라이브러리를 교체한 지금, 기존의 형태로 JWT 토큰을 처리할 시 약간의 이슈가 발생하였습니다.

조금 어려움을 겪었지만 스스로 해결 할 수 있었기에, 타입스크립트 환경에서 제가 얻었던 해결 방법을 공유하고자 합니다.

 

## 1. 문제

문제에 앞서 axios로 기존의 reissue를 처리하던 방식은 다음과 같습니다.

```tsx
axiosInstance.interceptors.request.use(
    function (config) {
        const headers = {
            ...config.headers,
            Authorization: 'Bearer ' + accessToken,
        };

        config.headers = headers;
        return config;
    },
);

axiosInstance.interceptors.response.use(
    (response) => {
        return response;
    },
    async error => {
        const originalRequest = {...error.config, noRepeat: true};

        if ( 인증오류 && error.config.noRepeat !== true) {
            await axiosReissue()
                .then(response) => {
                        if (response.status === 200) {
                            return axiosInstance(originalRequest);
                        }
                    }
                )
        }

        return Promise.reject(error);

    },
);

```

key가 만료되었는지 판단하는 것 같이 자잘한 과정은 빠져있지만 구조는 비슷하리라 생각됩니다.    
실제로 잘 작동하기도 하고요.

해당 코드의 흐름은 다음과 같습니다.

1. 요청에서 실패 발생
2. 인증 오류라면 reissue를 요청하여 AccessToken 갱신
3. 실패한 기존의 요청을 재 요청

하지만 ReactQuery에서 위와 같은 방식을 사용한다면, 기존의 요청을 다시 요청하여 성공한다 해도.  
useQuery와 useMutation의 옵션인 Success 함수는 성공 시에도 작동하지 않고,  
반환 값인 data는 여전히 갱신되지 않는 따위의 문제들이 발생하게 됩니다.

어째서 이런일이 발생할까요

## 2. 이유

결론부터 말하자면, 재요청을 한다고 해도 서로가 다른 Query기 때문입니다.     
더 정확히 말하면 한쪽이 Query가 아닌 것이죠

ReactQuery는 Query라고 하는 uniqueKey에 의해서 관리되는 단위를 기준으로 돌아가기 때문에,       
axios로 같은 api 요청을 날린다고 해도 기존의 Query 요청은 실패로 남아있게 됩니다.

## 3. 해결

코드를 바꿔보기에 앞서 우리는 typescript를 사용하고 있다는 사실을 인지해야 합니다.     
다음과 같이 type을 선업해줍시다.

```tsx
type CustomAxiosRequestConfig = AxiosRequestConfig & {
  noRepeat?: boolean,
	queryKey?: string[],
};

interface CustomAxios extends AxiosInstance{
    interceptors: {

    request: AxiosInterceptorManager<CustomRequestConfig>; 
    response: AxiosInterceptorManager<AxiosResponse>; 

    };

    getUri(config?: CustomAxiosRequestConfig): string;
    request<T>(config: CustomAxiosRequestConfig): Promise<T>;
    get<T>(url: string, config?: CustomRequestConfig): Promise<T>;
    delete<T>(url: string, config?: CustomRequestConfig): Promise<T>;
    head<T>(url: string, config?: CustomRequestConfig): Promise<T>;
    options<T>(url: string, config?: CustomRequestConfig): Promise<T>;
    post<T>(url: string, data?: any, config?: CustomRequestConfig): Promise<T>;
    put<T>(url: string, data?: any, config?: CustomRequestConfig): Promise<T>;
    patch<T>(url: string, data?: any, config?: CustomRequestConfig): Promise<T>;
}

constaxiosInstance: CustomAxios = axios.create({
    withCredentials: true,
    baseURL: `${process.env.REACT_APP_BASE_URL}`,
    timeout: 20000,
});
```

noRepeat는 reissue의 실패시 무한 재요청을 멈추기 위한, 그리고 reissue를 필요로 하지 않는 요청들을 위한 목적입니다.    
queryKey는 ReactQuery 장점을 활용하여 재요청을 하기위한 수단이고요.

이제 코드의 핵심 부분을 다음과 같이 바꿔보겠습니다.

```tsx
...

if (response.status === 200) {
    if (error.config.queryKey) {
        queryClient.refetchQueries(error.config.queryKey)
    } else {
        return axiosInstance(originalRequest);
    }
}

...
```

request의 config에 querykey가 포함되어 있다면 queryClient를 통해 해당 요청을 다시 refetch하는 것이죠.    
새로운 axios 요청이아닌 기존의 요청을 refetch 하는 것 입니다.       
그러나 이번에도 Reactquery 제대로 작동하지 않습니다.

왜냐하면 ReactQuery는 한번 실패한 쿼리의 reissue는 자동으로 실패 처리를 하거든요       
따라서 기존의 쿼리 결과를 한번 초기화 해줄 필요성이 있습니다.

완성된 코드는 다음과 같습니다.

```tsx
...

if (response.status === 200) {
    if (error.config.queryKey) {
        queryClient.resetQueries(error.config.queryKey).then(
            () => queryClient.refetchQueries(error.config.queryKey)
        );
    } else {
        return axiosInstance(originalRequest);
    }
}

...
```

## 4. 결론

해당 문제는 ReactQuery에 대한 이해도 부족으로 생겨난 것이 였습니다.     
사용법이 간단할지라도 제대로 이해하고 사용하는 것이 중요하다는 것을 다시금 느낍니다.
