---
title: "Intelij .http / HttpClient"
toc: true
toc_sticky: true
categories:

- dev

tags:

- Intelij

---

보통 http 리퀘스트를 테스트할 때 주로 postman을 많이 사용합니다. 저 역시 그렇습니다.

이번에 postman의 라이선스가 만료되어 다음 라이선스를 결제할 때까지 잠시 비로그인으로 사용하거나 다른 방법을 찾아야하는 경우가 생겼습니다.

postman은 라이선스가 만료되니 계정 자체가 잠겨버리더군요.

직장 동료분이 intelij에서 `.http`파일에서 리퀘스트를 날릴 수 있다고 이야기해주어 한 번 사용을 해보았는데 상당히 괜찮은 기능이었습니다.

그래서 intelij ultimate의 내장 httpClient에 대해 알아보았습니다.

> Intelij httpClient는 Ultimate(유료)에서 지원되는 기능입니다.
> 

# 장점

## 공식문서

![img]({{site.url}}/assets/images/jmb/intelij_http_client/img.png)
- 코드 하이라이팅
- `OpenAPI`를 통해 정의된 호스트, 메소드 유형, 헤더 필드 및 엔드포인트에 대한 코드 자동완성
- 요청, 해당 부분 및 응답 처리기 스크립트에 대한 코드 접기
- 요청 헤더 필드 및 문서 태그에 대한 인라인 문서
- HTTP 요청 구조 보기
- 요청 메시지 본문 내 웹 언어의 언어 주입
- 리팩토링 이동
- 라이브 템플릿
- Http proxy 구성

## 개인적 의견

- git을 통한 파일공유 가능
- 파일내 요청 일괄실행 기능으로 반복요청 등의 처리 가능
- intelij 툴 하나에서 코딩과 테스트 가능
- postman보다 가벼움

# 단점

- ECMA6가 아닌 5.1로 ES6 문법을 스크립트에서 사용불가 (ex. import, export, arrow func etc…)
- 응답명세는 상세히 볼 수 있으나 요청명세(request header, body etc…)는 보여주지 않음
- 헤더를 일일히 붙여줘야함

# 요청 생성

기본적인 형태는 아래와 같습니다.

```
### [comment]
Method Request-URI HTTP-Version
Header-field: Header-value

Request-Body
```

- `###` : 각각의 요청을 구분합니다.  주석처럼 뒤에 설명을 적을 수 있습니다.
- `Method`: 요청 방식을 입력합니다.
- `Request-URI`: 요청 uri를 입력합니다.
- `HTTP-Version`: http version을 입력합니다. 생략가능
- `Header-field: Header-value`: 헤더를 입력합니다. 생략가능
- `Request-Body`: 요청바디를 입력합니다. 생략가능

## 자동생성

컨트롤러 엔드포인트에서 요청을 자동 생성할 수 있습니다.

![img1]({{site.url}}/assets/images/jmb/intelij_http_client/img1.png)

![img2]({{site.url}}/assets/images/jmb/intelij_http_client/img2.png)

# 기능

postman에서 지원하는 일반적인 기능들은 거의 지원이 됩니다.

일부 편의기능이나 약간 미흡한 부분이 있지만 충분히 만족스럽고 사용해볼만하다고 생각합니다.

## 자동완성 및 코드 하이라이트

IDE에서 제공되는 기능인만큼 자동완성과 코드 하이라이트가 제공됩니다.

쿼리스트링이 존재하는 엔드포인트는 `@RequestParam`로 등록된 요소의 자동완성을 제공해줍니다.  

코드 하이라이트의 경우 기본 설정 외에 `Preferences`에서 직접 설정도 가능합니다.

## Http, gRPC, Websocket, Graphql 지원

기본적으로 http말고도 gRPC, Websocket, Graphql 요청방식을 지원합니다.

> gRPC, Websocket, Graphql의 사용법은 공식문서를 참조해주세요.
[https://www.jetbrains.com/help/idea/http-client-in-product-code-editor.html#grpc-requests](https://www.jetbrains.com/help/idea/http-client-in-product-code-editor.html#grpc-requests)
> 

### 변수처리

요청 시 많은 부분 변수처리가 가능합니다.

변수는 `{ {} }`의 형태로 사용할 수 있으며 변수 사용가능한 목록은 다음과 같습니다.

- 호스트
- 포트
- 경로
- 쿼리매개변수 또는 값
- 헤더 값
- 요청바디: `“{ {} }”`의 형태로 사용가능

기본제공 변수

- $uuid: uuid(UUID-v4)를 생성한다
- $timestamp: 현재 UNIX timestamp를 생성한다
- $randomInt: 랜덤한 숫자 0 ~ 1000 사이의 값을 생성한다.

![img3]({{site.url}}/assets/images/jmb/intelij_http_client/img3.png)

## ECMA 5.1스크립트 기반 response 핸들링

스크립트 기반으로 response를 핸들링 할 수 있습니다.

response의 결과를 가지고 테스트 코드를 작성하거나

인증토큰처럼 데이터를 가져와 저장한 뒤 다른 요청에서 사용하는 등의 처리도 가능합니다.

`client.global.set("key", "value")`로 값을 변수로 저장 가능하며 해당 변수는 Intelij가 종료될 때까지 보존됩니다.

> *The `client` object stores the session metadata, which can be modified inside the script. The `client` state is preserved until you close IntelliJ IDEA. Every variable saved in `client.global` as `variable_name` is accessible to subsequent HTTP requests as `{{variable_name}}`.*
> 

기본적으로 인라인으로 사용할 수 있으며 js파일을 불러와 사용할 수도 있습니다.

아직은 ES6를 지원하지 않기 때문에 js파일에서 다른 모듈을 불러와 사용하거나 중복코드를 분리하는 등의 처리는 할 수 없습니다.

- 인라인으로 사용
    
    `{ % % }` 형태로 작성할 수 있습니다.


![img4]({{site.url}}/assets/images/jmb/intelij_http_client/img4.png)

- js를 불러와 사용

![img5]({{site.url}}/assets/images/jmb/intelij_http_client/img5.png)

### 라이브러리 적용

인라인 스트립트의 경우 라이브러리가 자동적용 되어있지만 js파일을 참조하는 경우 직접 적용해줘야 합니다.

적용하지 않을 경우 자동완성이 되지않을 뿐 실행에는 영향을 끼치지 않습니다.

![img6]({{site.url}}/assets/images/jmb/intelij_http_client/img6.png)

![img7]({{site.url}}/assets/images/jmb/intelij_http_client/img7.png)

![img8]({{site.url}}/assets/images/jmb/intelij_http_client/img8.png)

### response 테스트 지원

스크립트에서 assert를 이용한 테스트 또한 진행할 수 있습니다.

`function`은 역시 화살표 함수 형태로 사용할 수 없습니다ㅠㅠ

![img9]({{site.url}}/assets/images/jmb/intelij_http_client/img9.png)

## 환경변수

환경변수 파일에서 환경을 세트로 지정할 수 있습니다.

local, dev, prod 별로 환경을 구성하는 등의 처리에 유용합니다.

생성 시 `regular` 와 `private` 중 선택 가능하며 `regular`는 커밋목록에 포함되는 파일이며 `private` 는 커밋목록에서 제외되는 파일입니다.

민감정보와 같은 경우 `private`에 저장하여 사용하면 외부에 공유되는 것을 방지할 수 있습니다.

실행 시 정의된 환경변수 중 선택하여 실행할 수 있습니다.

![img10]({{site.url}}/assets/images/jmb/intelij_http_client/img10.png)

![img11]({{site.url}}/assets/images/jmb/intelij_http_client/img11.png)

## 응답쿠키 자동저장 (최대 300개)

 `.idea/httpRequests/http-client.cookies`파일에 응답 쿠키를 최대 300개 자동 저장합니다.

쿠키의 이름과 값은 만료되지 않은 경우 지정된 도메인 및 경로와 일치하는 각 후속 요청에 자동으로 포함됩니다.

![img12]({{site.url}}/assets/images/jmb/intelij_http_client/img12.png)

자동으로 로그를 남기기 원치 않는다면 `@no-cookie-jar`로 로그를 저장하지 않을 수 있습니다.

```
###
// @no-cookie-jar
GET http://localhost:8080
```

## 최근 50개 요청로그 저장

 `.idea/httpRequests/http-requests-log.http`파일에 최근 요청 최대 50개를 자동 저장합니다.

![img13]({{site.url}}/assets/images/jmb/intelij_http_client/img13.png)

![img14]({{site.url}}/assets/images/jmb/intelij_http_client/img14.png)

자동으로 로그를 남기기 원치 않는다면 `@no-log`로 로그를 저장하지 않을 수 있습니다.

```
###
// @no-log
GET http://localhost:8080
```

## 응답 비교

두 개 이상의 응답결과가 존재할 시 응답을 비교할 수 있습니다.

![img15]({{site.url}}/assets/images/jmb/intelij_http_client/img15.png)

- scratch 파일에서 비교
    
    scratch 파일에서 실행된 요청에 대한 응답은 자동으로 요청 하단에 추가됩니다.


![img16]({{site.url}}/assets/images/jmb/intelij_http_client/img16.png)

- 클립보드와 비교
    
    요청 결과를 복사한 뒤 다음 요청의 결과와 비교 할 수 있습니다.
    
    과정이 비교적 수월하여 가장 편한 방식이라고 생각됩니다.


![img17]({{site.url}}/assets/images/jmb/intelij_http_client/img17.png)

- 로그에서 비교할 파일 선택

![img18]({{site.url}}/assets/images/jmb/intelij_http_client/img18.png)

## client 인증서 설정

`private` 환경파일을 사용하여 클라이언트 인증서를 설정할 수 있습니다.

```json
{
    "dev": {
        "MyVar": "SomeValue",
        "SSLConfiguration": {
            "clientCertificate": "cert.pem",
            "clientCertificateKey": "MyFolder/key.pem"
        }
    }
}
```

> 더 자세한 내용은 공식문서를 참조해주세요
[https://www.jetbrains.com/help/idea/http-client-in-product-code-editor.html#ssl_certificate](https://www.jetbrains.com/help/idea/http-client-in-product-code-editor.html#ssl_certificate)
> 

## cURL 변환

http request, curl 간의 변환이 가능합니다.

오른쪽 상단의 convert버튼으로 변환을 진행할 수 있습니다.

![img19]({{site.url}}/assets/images/jmb/intelij_http_client/img19.png)

- http request ⇒ curl

![img20]({{site.url}}/assets/images/jmb/intelij_http_client/img20.png)

- curl ⇒ http request

![img21]({{site.url}}/assets/images/jmb/intelij_http_client/img21.png)

![img22]({{site.url}}/assets/images/jmb/intelij_http_client/img22.png)