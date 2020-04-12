---
layout: post
title:  "enum class"
date:   2020-04-12 23:00:00 +0900
categories: Android
---

프로그래밍을 하다보면, 상태를 구분하기 위한 상수들을 사용할 때가 있습니다.
몇 개 안될 떄는 크게 상관없지만, 계속해서 상수들이 늘어나게 될 경우 가독성이 떨어지게 되는데, 이게 은근히 거슬립니다 :(

그래서 이번 기회에 그러한 코드들을
enum으로 바꿔보았고, 굉장히 만족합니다 :)


# enum 사용 이유

1. 코드가 단순해지며 가독성이 좋음
2. enum은 다른 클래스를 상속할 수가 없지만, 다양한 인터페이스들을 상속 할 수 있다
3. 내부적으로 public static final 으로 정의
4. 키워드 enum을 사용하기 때문에 구현의 의도가 열거임을 분명하게 나타낼 수 있습니다.


## 간단한 예제 

```
    // 기존
    public static final String ROCK = "ROCK";
    public static final String PAPER = "PAPER";
    public static final String SCISSORS = "SCISSORS";
    

    // enum을 사용
    enum RPS {
    ROCK,PAPER,SCISSORS
}

```

### 20200412 회고
- 프로젝트 리팩토링간 왜 그동안 enum을 사용하지 않았을까? 란 생각이 들었을 정도로 유용하게 사용중이다.
- 혹시 목적에 맞지 않게 사용하지는 않는지 고민이 필요할 것 같다. ( 맹목적인 것은 지양 하지만, 단점이 있을까?)