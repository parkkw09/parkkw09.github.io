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

나는 안드로이드를 주로 다뤘기 때문에 기존에 Test는 Android Studio로 진행했었다. 이번에 IOS 프로젝트를 진행하게 되면서 

```
func main() {
   foo(); 
}
```