# 기술기록(RX)

[![GPL2](https://img.shields.io/badge/license-GPL2-yellowgreen.svg)](https://github.com/parkkw09/parkSync/edit/master/LICENSE)

```
#rx노트 5
데이터베이스 쓰기가 겁나 편해졌어용~
아...생각해보니 thread처리 안했네 ㅋㅋㅋㅋ
2018. 10. 17. 오후 5:40
```

```
#rx노트 4
3콤보 정돈 처리해줘야 rx지.
1) 데이터A를 가져온다.
2) 데이터 A를 저장한다.
3) 데이터 A의 B를 가져온다.
4) 데이터 B를 저장한다.
4-1) 저장이 완료될 때 까지 기다린다.
5) 데이터 A를 꺼내면서 동시에 데이터 B를 꺼낸다.
6) 데이터 A와 B의 조합으로 데이터 C를 만든다.
7) 데이터 C를 저장한다.
7-1) 데이터 C가 저장이 완료가 될 때 까지 기다린다.
2018. 10. 1. 오후 11:09
```

```
#rx노트 3
하란대로 안하고 내 맘대로 만들었더니 메모리가 줄줄세네ㅠㅠ
2018. 8. 7. 오후 12:37
```

```java
#rx노트 2
reduce, bifunction
var sales = ArrayList<Pair<String, String>>()
sales.add("TV" to "2500")
sales.add("Camera" to "300")
sales.add("TV" to "1600")
sales.add("Phone" to "800")
sales.add("TV" to "400")

val tvSales: Maybe<String> = Observable.fromIterable(sales)
.filter{sale -> "TV" == sale.first}
.map{sale -> sale.second}
.reduce{
sale1, sale2 -> "[[$sale1] + [$sale2] = [${sale1 + sale2}]]"
}
tot = [[[[2500] + [1600] = [25001600]]] + [400] = [[[2500] + [1600] = [25001600]]400]]

A = [[2500] + [1600] = [25001600]]
tot = [[A] + [400] = [A]400]]

이해하기 무척이나 어렵군요 ㅎㅎㅎㅎㅎㅎㅎㅎㅎ
2018. 6. 22. 오후 1:12
```

```
#rx노트 1
장난나랑지금하냐
2018. 6. 21. 오후 6:15
```
