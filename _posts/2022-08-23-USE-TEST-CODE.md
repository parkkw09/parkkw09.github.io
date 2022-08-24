---
layout: post
title: "테스트 기능을 이용한 구현"
author: "Peter Park"
categories: tech
tags: [tech]
image: view-0005.jpg
---

## 시작

예전 같으면 HTTP API로 동작하는 기능을 구현할 때 보통 서버가 나중에 나오기 때문에 주고 받을 데이터(보통 JSON) 협의와 함께 화면 구성을 시작했다.
이마저도 나는 API가 어느정도 나오면 진행하는 축에 속하는데 어떤 이들은 말 듣자마자 이미 만들어 놓오고 API 쏘는 것만 기다리는 분들도 있었다.
가장 중요한 것은 구현중이든 아니든 API가 정상적으로 동작이 되는지 보려면 작동시키는 UI가 필요한데 보통은 UI를 다 뽑아내고 그 다음 실행시켜보곤 했다.

최근에 테스트에 관심이 좀 생기면서 이 테스트 코드를 응용하는 방식을 자주 사용하게 되는데 이게 아주 쏠쏠하다.

코드를 만들어 놓고 다시 만들게 아니라면 테스트를 진행하면서 구현한 구현체를 그대로 사용해야 하는데 클린아키텍처는 계층형 구조이기 때문에 이럴때 써먹기 아주 좋다.
물론 클린 아키텍처를 좋아하시는 분들이 알게되면 놀라 자빠질 사용법일 수도 있으나 연장을 어떻게 쓰던지 잘써먹으면 그만이다.

## Clean Architecture

먼저 요즘 내가 특별한 일이 없으면 구성하는 파일 구조이다.

```bash
Project/
├── Data                 # Data Layer
|  ├── Entity            # Entities
|  └── Source            # Api, DB, File, Etc...
├── Domain               # Domain Layer
|  └── Usecase           # Business logic
└── Presentation         # View Layer
```

그 유명한 [도넛](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) 까진 아니지만 의존성 관계를 유지하여 구성을 한다.
이러면 결국 나는 위에서 이야기한 HTTP API 테스트를 위해 전체(UI 까지)를 온전히 다 만들지 않고 Data Layer만 잘 만들게 되면 테스트가 가능하다.
(위 표 보다는 좀 더 세분화 하여 사용한다.)

## Test in XCode

나는 안드로이드를 주로 다뤘기 때문에 기존에 Test는 Android Studio로 진행했었다. 이번에 IOS 프로젝트를 진행하게 되면서 XCode로도 한번 진행해보고자 마음먹었다.
안드로이드 스튜디오에서 유닛테스트 하듯 잘 되어 있을 것이라 예상 내가 바보....아니 역시 간단하지만은 않았다.

일단 기본적인 사용법은 너무 복잡하니 내가 참고한 링크로 대체 하기로 하고

1. [TDD와-Swift-XCTest-활용하기](https://velog.io/@minni/TDD%EC%99%80-Swift-XCTest-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)
2. [Unit-Test-사용해보기](https://velog.io/@chagmn/Unit-Test-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0)

나는 Android Studio(이하 AS)와 비교해서 특이했던 것들을 공유하고자 한다.

```
import XCTest

class UnitTest: XCTestCase {

    override func setUpWithError() throws {
        // Put setup code here. This method is called before the invocation of each test method in the class.
    }

    override func tearDownWithError() throws {
        // Put teardown code here. This method is called after the invocation of each test method in the class.
    }

    func testExample() throws {
        // This is an example of a functional test case.
        // Use XCTAssert and related functions to verify your tests produce the correct results.
        // Any test you write for XCTest can be annotated as throws and async.
        // Mark your test throws to produce an unexpected failure when your test encounters an uncaught error.
        // Mark your test async to allow awaiting for asynchronous code to complete. Check the results with assertions afterwards.
    }

    func testPerformanceExample() throws {
        // This is an example of a performance test case.
        self.measure {
            // Put the code you want to measure the time of here.
        }
    }

}
```

먼저 위와 같은 기본 코드를 만들 수 있게 되는데 AS는 어노테이션등을 통해 테스트 케이스 이름이나 조건 등을 꾸밀 수 있는데 특별히 이름 제약은 없다.
하지만 Xcode는 특이하게 함수이름이 testXXXX로 되어 있어야 한다. 그렇지 않으면 Xcode에서 인식을 하지 않아서 케이스가 동작하지 않는다.

구동을 하고 종료하는 부분은 이름만 다를 뿐 AS의 그것과 크게 다르지 않았다. 측정을 위한 함수도 별도로 존재하는 듯 했다.

AS에서는 해당 테스트 코드가 사용되는 것이라 가정했을 때 그냥 파일을 우클릭하는 것만으로 테스트 메뉴를 부를 수 있지만 xcode는 좌측에 메뉴에서 테스트 전용 탭을 선택해야 한다.

그 무엇보다 가장 크게 다른 점은 AS는 유닛테스트를 진행할때는 안드로이드에 종속된 라이브러리가 아니라면(종속된 것들도 목업으로 대체 가능) 로컬에서 작동시킬 수 있다.
물론 UI 테스트는 앱이 단말기(혹은 에뮬레이터)에 설치되고 난 이후 작동한다. Xcode는 내가 설정을 몰라서 그럴 수도 있으나 UNIT / UI 테스트 모두 단말(에뮬레이터)에서 작동한다.

## HowTo

사실 별거 없다. 테스트 코드 자체가 독립으로 작동이 가능한 main()함수로 생각하면 되는 것이다.

이 과정을 통해 정석으로 테스트 코드도 만들어보면 좋겠지만 간단하게 런을 하기 위해

1. Android Activity의 onCreate()
2. IOS ViewController의 viewDidLoad()

같은 곳에 억지러 객체 생성하고 호출하는 것 보다는 나이스하지 않은가?

```
import XCTest
@testable import Seoul

class SeoulTests: XCTestCase {

    var repository: SeoulRepository? = nil

    override func setUpWithError() throws {
        // Put setup code here. This method is called before the invocation of each test method in the class.
        repository = SeoulRepositoryImpl(remote: SeoulApi())
    }

    override func tearDownWithError() throws {
        // Put teardown code here. This method is called after the invocation of each test method in the class.
    }

    func testGetSeoul() throws {
        Task {
            do {
                if let data = try await repository?.getSeoul() {
                    print("testGetSeoul() result[\(data)]")
                } else {
                    throw SeoulError.requestFailure(message: "repository is NULL")
                }
            } catch {
                print("Unexpected error: \(error).")
            }
        }
    }

}
```

뭐 이렇게?

## 결론

빨리빨리 결과물이 나와서 고객(혹은 그분들)에게 보여주어야 하는 상황이라면 테스트 케이스는 고려 사항이 아닐 것이다.
하지만 서버와의 개발 일정이 겹치는 상황이거나 서버가 존재하는 상황에서 새로 클라이언트를 구성하는 것이라면 유닛(혹은 UI) 테스트를 이용한 개발이 큰 도움이 될 것이라고 생각한다.
물론 이걸 하려면 클린 아키텍처까진 아니더라도 의존성 흐름을 일정하게 유지하는 계층형 구조로 앱을 설계해야 한다.

두번 만들게 아니라면...