---
layout: post
title: "스위프트 개발 #3"
author: "Peter Park"
categories: daily
tags: [daily,swift,alamofire,concurrency]
image: view-0003.jpg
---

## 시작

스위프트의 [Modern Concurrency](https://developer.apple.com/documentation/swift/updating_an_app_to_use_swift_concurrency)를 이용하기로 마음먹고 alamofire 에 응용하기 위해 찾아보던 중 아래 블로그를 찾았다.

[Alamofire async / await 사용해보기](https://byeon.is/swift-concurrency-async-await-with-alamofire/)

그중 알라모파이어를 응용하여 예외처리까지 하는 여러 코드가 존재하지만 내 눈높이에서 가장 필요한 코드는 아래 코드였다.

```
func requestJSON<T: Decodable>(_ url: String,
                                 type: T.Type,
                                 method: HTTPMethod,
                                 parameters: Parameters? = nil) async throws -> T {

    return try await session.request(url,
                                 method: method,
                                 parameters: parameters,
                                 encoding: URLEncoding.default)
    .serializingDecodable()
    .value
}
```

해당 코드는 사전에 정의된 request URL(+ method) 을 이용해서 사전에 정의된 JSON 데이터를 가져오는 코드로 여러 형태의 결과값을 처리하기 위해 generic 처리가 되어 있고 알라모파이어를 동시성 프로그래밍 형태로 사용한다. 물론 예외처리도 들어있다.
저렇게 그냥 가져다 쓰면 만사 오케이이긴 하지만 공부를 하는 입장에서 그냥 넘어가기 아쉬워서 하나하나 뜯어 보았다.

## T

```
func foo<T: Any>() -> T {
    return T
}
```

제네릭을 접해봤다면 아주 흔하디 흔한 코드이다. 다행이도 스위프트는 여타 제네릭과 별반 다르지 않게 지원한다.(물론 더 복잡한 기능을 제공하겠지만) 공식 메뉴얼 한글판에서 [지네릭](https://jusung.gitbook.io/the-swift-language-guide/language-guide/22-generics) 참고하면 좋을 것 같다.

여튼 흔히들 쓰는데 나는 보일러 플레이트 코드와 어느정도 트레이드오프 되더라도 잘 사용하지 않는다. 잘 안쓰는 이유는 이게 제대로 돌아가려면 말 그대로 코드가 주고 받은 데이터가 일반화 되어야 하는데 서버/클라이언트 서비스에서 클라이언트의 운명은 대부분 서버에 종속되어 있는지라 제네릭을 통해 보일러 플레이트 코드를 최소화 시켜놓더라도 시간이 지남에 따라(보통은 그 시간이 결코 멀지 않다) 제네릭 코드 유지를 위해 또다른 곳에서 보일러 플레이트 코드를 생산하는 일이 생기기 때문이다. 즉 한군데가 이뻐지기 위해 또 다른 어딘가가 지저분에 지는 현상이 발생한다. 이럴바엔 차라리 명령어에 맞게 타이트하게 만드는게 더 나은 최적화일 경우가 많다.

## try, throws

```
func foo() throws -> any {
    return try any()
}
```

사실 내가 뜯어보려고 마음먹은 이유가 이부분이다. 스위프트에 능숙하다면 모르겠으나 나는 공부하는 입장이기 때문에 한눈에 들어오지 않는 것은 당연한 것이다. 그리고 실제로 코드를 작성할 때도 간소화를 위해 고급언어의 기술?을 이용해서 간소화시키는 것을 피하는 편인다. 이유는 단순하다. 지금 내가 겪고 있는 이 상황처럼 한눈에 못알아볼 가능성이 점점 높아지기 때문이다. 만약 협업의 과정에서 이걸 어설프게 따라하면 엄청 창조적인 코드가 생산되기 때문에 너도 나도 손잡고 같이 나락으로 갈 바에야 차라리 알아보기 쉽게 만들어서 생산성 끌어올리는게 유익한 방법이라고 생각한다.(삼천포...)

여튼 try 거는거 좋다. throws 로 exception 던지는거 좋다. 그런데 저 코드의 원래 뜻은 무엇인가? any() 가 리턴할 값이 여러가지면 어떻게 처리해야 하나? exception을 만들어야 하는 상황이면 어떻게 처리해야 하나? 저 코드 그대로 쓰면 문제야 없겠지만 내 식대로 응용으로 하려면 처음 봤을 때는 이해하기 어려웠다.

그래서 일단 참고할 블로그를 보고 공부를 했는데 아래와 같다.

1. [try? try! try](https://zetal.tistory.com/entry/swift-try-try-try)
2. [예외처리 (throws, do-catch, try) 하기](https://twih1203.medium.com/swift-%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC-throws-do-catch-try-%ED%95%98%EA%B8%B0-c0f320e61f62)
3. [Error Handling](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html)

스위프트는 모든게 옵셔널 체이닝인데 이전에 try 잘못 써서 뻗게 만들어본 경험이 있어서 인지 유심히 봤다. 그러고 위 내용은 풀어 써보았다.

```
func foo() throws -> any {
    do {
        let data = try any()
        return data
    } catch {
        throw Error.error()
    }
}
```

```
func foo() -> any {
    return try any()
}
```

먼저 위에 함수는 정석대로 do ~ catch 를 쓰고 throw로 Error를 던져본 것이고 밑에 함수는 throws 를 빼보았다. 결과는 위에는 정상 처리 되기 하지만 any()가 만약 exception을 만들경우 그 내용과 상관없이 내가 별도로 만든 error를 던질 것이고 밑에 함수는 try 처리가 미흡하다고 xcode에서 빌드 전에 에러를 표시해준다.

이렇게 만들어보니 약간 이해가 되었다. 다음 차례로 블로그에서 제공한 try? try! 등을 사용해보니 처리는 가능하지만 처음 결과처럼 중간에 exception을 유실할 가능성이 생긴다. 결국 do catch 없이 구성해야 함수 내부에서 예외처리를 하지 않고 함수 밖으로(함수를 호출하는) 보내 처리할 수 있다.

## async, await, task

```
func requestJSON<T: Decodable>(_ url: String,
                                 type: T.Type,
                                 method: HTTPMethod,
                                 parameters: Parameters? = nil) async throws -> T {

    return try await session.request(url,
                                 method: method,
                                 parameters: parameters,
                                 encoding: URLEncoding.default)
    .serializingDecodable()
    .value
}
```

해당 코드에서 만약 try 처리가 빠진다면 async, await 처리 구간은 바로 워닝이 뜬다. try 처리를 하라고... 참 모든게 연결된 코드이긴 하다.

그런데 여기서 가장 중요한게 빠졌다. 코루틴의 경우 xxxScope 라는 클래스를 이용해서 스코프 처리를 하는데 async/await 형태의 코드는 어디서 실행을 하는 것인가? 이게 참 눈에 띄질 않는다. 코드는 아래와 같다.

```
func foo() {
    Task {
      try await fetch()
    }
}
```

Task 라는 scope로 처리하면 되는데 이게 잘 안보여서 한참을 봤었다. 이제 여기서 위해 말한 것들을 모두 응용을 하게 되면 throws 로 던진 예외도 처리해주기 위해 아래와 같이 처리해주면 최종 scope에서 처리가 가능해진다.

```
func foo() {
    Task {
        do {
          try await fetch()
        } catch {
        }
    }
}
```

여기서 또 애매해지는게 자바 계열은 exception 처리를 하기 위해 catch 문에서 매게변수를 받게 되는데 스위프트는 그게 없다. 읭??? 에러 메세지라도 찍고 싶은데 어쩌지? 이러고 있다가 이것저것 뒤져보니 그냥 아래 처럼 하면 되더라.

```
func foo() {
    Task {
        do {
            try await fetch()
        } catch {
            print("Unexpected error: \(error).")
        }
    }
}
```

스위프트 클래스가 error라는걸 이미 들고 있는 구조인 것이다. foundation에 들어 있거나....

아무튼 이과정을 모두 알아야 맨위 코드가 왜 저모양인지 알게 된다.

## concurrency alamofire

동시성 처리를 위한 코드는 좋다. 그럼 동시성 처리를 하기 위한 명령어는 무엇인가? 나는 알라모 파이어를 동시성 프로그래밍으로 처리하기 위해 이 짓을 하고 있는 것이다. 그래서 알라모 파이어도 동시성 프로그래밍 형태로 구성해야 한다. 나는 기존에 콜백 방식을 비동기식이라고 불렀기 때문에 이를 동기식이라고 부르긴 했는데 맞는 표현인지 모르겠다.

아무튼 예를 들어 기존 처럼 알라모 파이어를 쓴다면

```
func requestCommand(request: Request, callback: @escaping () -> Void) {
        AF.request(request.httpURL)
            .validate(statusCode: 200..<500)
            .validate(contentType: ["application/json"])
            .response { response in
                switch response.result {
                    case .success(let _data):
                        self.content = _data
                    case .failure(let error):
                        print("AlamofireSession requestCommand() Error [\(error.localizedDescription)]")
                }
                callback()
            }
    }
```

보통 위와 같이 생긴다. @escaping 을 콜백 클로저 함수로 콜백해서 람다(클로저) 방식으로 구현체를 만드는데 이게 많아지면 콜백지옥이 되는 것이다.

이걸 동시성으로 처리하려니 AF.reqeust() 여기까진 좋은데 그다음 response() 따로 존재하지 않았다. 읭??? 잠시 당황을 하고 좀 찾아보다가 아래와 같은 깃헙 메세지를 찾았다.

```
Alamofire 5.5's concurrency support supports awaiting the full response, result, or throwing value of a request.

let task = AF.request(...).serializingDecodable(Type.self)
let response = await task.response
let result = await task.result
let value = try await task.value

Convenience methods have also been added for the various asynchronous state producers, like onURLSessionTaskCreation.

```

처음 let task 변수를 보게 되면 AF.request().serializingDecodable(Type.Self) 라고 되어 있는데 이게 핵심이었다. 참고로 serializingXXXX는 몇가지 함수를 제공하는데 나는 처음에 string으로 놓고 테스트를 해서 확인하였다. 참고로 Decodable은 JSON과 같은 데이터의 인코딩/디코딩을 위한 함수이다.  

그리고 보통 validate()라는걸 써서 에러 범위라든지 값을 미리 체크하는데 이게 되려면 위와 같이 reponse.result 에서 성공/실패를 구분해야 한다.

그런데 그런것들 없이 value로 바로 던져버린다. 동시성 알라모 파이어에서는... 사실 이게 다 확인이 되는지는 좀더 봐야 겠지만 나는 기존에 확인한 코드와 내가 이해한 방향을 더해 중간점을 찾은 코드를 만들었다.

```
let response = await AF.request(requestUrl,
                                method: HTTPMethod.get,
                                parameters: nil,
                                encoding: URLEncoding.default)
                    .validate(statusCode: 200..<500)
                    .validate(contentType: ["application/json"])
                    .serializingDecodable(Type.self)
                    .response

switch response.result {
    case .success(let data):
        return data
    case .failure:
        throw Error.error()
}
```

## 결론

프로그래밍의 방법은 정석이 없다고 생각한다. 다만 모두가 협의하여 상황과 현실에 맞게 만들면 그게 최적화이고 그게 답이라고 생각한다. 이번에 내가 공부해본 것들이 정답이 아닐 순 있다. 정답을 완전히 빗겨나간 결과일 수도 있으나 원래 프로그래밍은 이러면서 배우는거다.

몇년 후에 이글을 보며 이불킥을 날릴것인가??