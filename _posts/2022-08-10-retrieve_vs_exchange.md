---
title: "Webclient에서 retrieve와 exchange의 차이점"
toc: true
toc_sticky: true
categories:

- dev

tags:

- Spring WebFlux
- WebClient

---


# retrieve vs exchange

---

# webclient에서 retrieve와 exchange는 어떻게 다를까

> `exchange`는 현재 *deprecated* 되어 본문의 예제에서는 `exchangeToMono`를 사용합니다.
> 

프로젝트 중 외부 API를 호출할 일이 생겨 요청모듈을 만들고 있었는데 추후 응답 값의 헤더를 참조할 필요가 있어  
webclient에서 헤더 값을 추출하는 방법을 찾고있었습니다.   
retrieve가 아니라 exchange를 사용하면 된다는 글을 보았는데 그렇다면 이 둘은 서로 무엇이 다르고 어떤 차이가 있을까 궁금하여 찾아보게 되었습니다.

공식문서에 따르면 

## retrieve

> Perform the HTTP request and retrieve the response body.
> 
> 
> ...
> 
> This method is a shortcut to using exchange() and decoding the response body through ClientResponse.
> 
`retrieve`는 `ClientResponse`를 통해 응답 바디를 디코딩한 `exchange`의 숏컷이다.


## exchange

- *주의) exchange는 현재 deprecated되었고 exchangeToMono와 exchangeToFlux 사용을 권장합니다.*

> Perform the HTTP request and return a ClientResponse with the response status and headers. You can then use methods of the response to consume the body:
>
`exchange`는 상태코드와 헤더와 함께 `ClientResponse`를 반환합니다.  
<br/>

제가 이해한대로 풀어보자면 `retrieve`는 응답 바디를 바로 받아볼 수 있는 `exchange`의 편의 메소드이며 `exchange`는 응답 바디뿐만 아니라 헤더와 상태코드까지 받아볼 수 있는 메서드라는 것인데

그렇다면 `retrieve` 에서는 헤더와 상태코드를 받아볼 수 없는 것일까요

테스트를 위해 간단한 컨트롤러를 만들었습니다.

# 테스트

아래는 version이라는 쿼리스트링을 받으며 이 값에 따라 상태코드와 응답바디를 다르게 리턴하는 엔드포인트입니다.

```java
// 쿼리의 version값에 따라 다른 응답코드와 바디를 리턴하는 엔드포인트
@GetMapping("/return")
public ResponseEntity<?> returnData(@RequestParam(required = false) int version) {

    Map<String, String > queryString = new HashMap<>();
    queryString.put("page", "123");
    queryString.put("size", "1231");
    queryString.put("size", "1231");

    Map<String, String > credential = new HashMap<>();
    credential.put("aaaa", "aaaaa");
    credential.put("bbbb", "bbbbb");

    HttpHeaders headers = new HttpHeaders();
    headers.add("AAAAAAAA", "fklsdnafg7i234gyuisv684");
    headers.add("BBBBBBBB", "gv3jkigh98vwb0");
    headers.add("CCCCCCCC", "0000000000090000000");

    switch (version) {
        case 2:
            return ResponseEntity
                .status(200)
                .headers(headers)
                .body(credential);
        case 3:
            return ResponseEntity
                .status(300)
                .headers(headers)
                .body(credential);
        case 4:
            return ResponseEntity
                .status(400)
                .headers(headers)
                .body(queryString);
        case 5:
            return ResponseEntity
                .status(500)
                .headers(headers)
                .body("gnio4hwg8902349030kzmamamavjih890w44gkuzd");
        default:
            return ResponseEntity
                .status(200)
                .headers(headers)
                .body(queryString);
    }
}

```

## Retrieve

같은 컨트롤러에 다른 엔드포인트를 하나 더 만들어 /return으로 요청하여 값을 검증해보겠습니다.

```java
@GetMapping("/testRetrieve")
public String testRetrieve(@RequestParam(required = false) int version) {

    Mono<ResponseEntity<Object>> objectMono = WebClient.create("http://localhost:8090")
        .get()
        .uri("/return?version=" + version)
        .retrieve()
        .toEntity(Object.class);

    ResponseEntity<Object> response = objectMono.block();
    System.out.println("version : " + version);
    System.out.println("response.getHeaders() = " + response.getHeaders());
    System.out.println("response.getStatusCodeValue() = " + response.getStatusCodeValue());
    System.out.println("response.hasBody() = " + response.hasBody());
    System.out.println("response.getBody() = " + response.getBody());

    return "test Ok";
}
```

![img1]({{site.url}}/assets/images/jmb/retrieve_exchange/img1.png)

정상적으로 값을 받아오는걸 볼 수 있습니다.

400과 500은 Bad Request와 Internal Server Error로 Webclient가 오류를 띄워주네요.

`retrieve` 에서도 정상적으로 상태코드와 헤더를 받아볼 수 있었습니다.

그렇다면`retrieve` 에서도 상태코드, 헤더와 같은 정보를 받아올 수 있는데 `exchange` 와는 어떤 점이 다른걸까 더 불분명해진 상황이었습니다.

자료를 더 찾아본 결과 `exchange`는 응답에 따른 유연한 상태처리가 가능하다는 것이었는데요

하지만 응답에 따른 상태처리는 `retrieve` 도 `onStatus` 함수로 처리할 수 있습니다.

`onStatus`의 인터페이스를 확인해보면 매개변수로 Throwable을 상속받은 객체를 Mono로 감싸 넘겨줄 수 있습니다. 

![img2]({{site.url}}/assets/images/jmb/retrieve_exchange/img2.png)

## Exchange

그러면 `exchange` 는 어떤지 살펴보았습니다.

똑같이 /return으로 요청을 보내고 처리만 `exchange`로 진행하였습니다.

이번에는 `exchangeToMono`의 인터페이스 먼저 살펴본 후 넘어가겠습니다.

![img3]({{site.url}}/assets/images/jmb/retrieve_exchange/img3.png)

Throwable을 상속받은 객체만 Mono형태로 리턴할 수 있는 `onStatus`와 달리 Mono를 상속받은 아무 객체나 리턴시킬 수 있습니다.

이제 테스트를 진행해보겠습니다.

```java
// /return에 요청을 보내 상태코드에 따라 응답 값을 처리하는 엔드포인트
@GetMapping("/testExchange")
public String testExchange(@RequestParam(required = false) int version) {

    Mono<Object> objectMono = WebClient.create("http://localhost:8090")
        .get()
        .uri("/return?version=" + version)
        .exchangeToMono(res -> {
						// 헤더를 통한 컨트롤 또한 가능하다.
						// HttpHeaders httpHeaders = res.headers().asHttpHeaders();
            if (HttpStatus.OK.equals(res.statusCode())) {
                return res.bodyToMono(Object.class);
            } else if (HttpStatus.MULTIPLE_CHOICES.equals(res.statusCode())) {
                return Mono.empty();
            } else {
                return res.createException().flatMap(Mono::error);
            }
        });

    System.out.println("version : " + version);
    if(!objectMono.hasElement().block()){
        System.out.println("objectMono NULL");
    }else{
        System.out.println("objectMono.block().toString() = " + objectMono.block().toString());
    }

    return "test Ok";
}
```

![img4]({{site.url}}/assets/images/jmb/retrieve_exchange/img4.png)

오류의 형태로만 컨트롤할 수 있던 `retrieve` 와 달리 `exchange` 는 리턴 값의 형태를 바꾸거나 오류로 치환하는 등의 더 유연한 처리가 가능했습니다.

# 마무리

1. `retrieve`는 `exchange` 를 이용한 편의 메서드이다.
2. `retrieve` 에서도 충분히 상태코드와 헤더를 받아볼 수 있다.
3. `retrieve` 에서도 `onStatus`를 이용하여 상태코드에 따른 에러처리를 할 수  있으나 에러의 형태로만 처리가 가능하다.
4. `exchange`는 상태코드 뿐만 아니라 헤더를 통한 처리 등 `retrieve` 보다 훨씬 유연한 처리가 가능하다.

결과적으로 간단한 처리만 하는 경우에는 `retrieve`

응답 상태 및 결과에 따른 에러발생, 데이터 변경 등 다양한 처리가 필요하다면 `exchange` 를 사용

추가적으로 Mono나 Flux의 메소드 중 do…, on… 의 여러 메소드들이 존재하여 에러 발생 시 특정 값이 넘기거나 에러를 스킵하고 동작, 성공 시 처리, 종료 시 처리 등의 또 다른 처리들이 가능하지만 아직은 공부가 부족하여 좀 더 공부한 후에 다뤄보는것이 보고싶습니다.

> 내용에 틀리거나 지적할 부분이 있을 시 알려주신다면 감사하겠습니다.
> 

> 참조:
[https://stackoverflow.com/questions/58410352/spring-boot-webclients-retrieve-vs-exchange](https://stackoverflow.com/questions/58410352/spring-boot-webclients-retrieve-vs-exchange)
>