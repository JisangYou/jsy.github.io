---
layout: post
title:  "Data Binding (표현식)"
date:   2020-03-29 23:00:00 +0900
categories: Android
---

# 정의

>> The Data Binding Library is a support library that allows you to bind UI components in your layouts to data sources in your app using a declarative format rather than programmatically.

-> 레이아웃의 UI Components와 앱의 데이터를 연결하는 것을 지원하는 라이브러리

## xml 상에서 표현
- 아래와 같이 표현이 되는데, data binding 라이브러리를 사용하기 전과 다른 점은 
layout 루트 태그로 전체 binding을 하고자 하는 요소 들을 감싼다.

```
<?xml version="1.0" encoding="utf-8"?>
    <layout xmlns:android="http://schemas.android.com/apk/res/android">
       <data>
           <variable name="user" type="com.example.User"/>
       </data>
       <LinearLayout
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent">
           <TextView android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="@{user.firstName}"/>
           <TextView android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="@{user.lastName}"/>
       </LinearLayout>
    </layout>
```


# 클래스 내 사용 법

```
   override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val binding: ActivityMainBinding = DataBindingUtil.setContentView(
                this, R.layout.activity_main)

        binding.user = User("Test", "User")
    }
```

- 레이아웃의 뷰를 데이터 객체와 결합하는 데 필요한 클래스를 자동으로 생성한다.
- ex) 클래스 이름은 레이아웃 파일 이름 기반으로 하여 파스칼 표기법으로 변환하고 Binding 접미사를 추가하므로, 
activity_main.xml이므로 생성되는 클래스는 ActivityMainBinding이 된다. 
- User 데이터 객체는 아래에 정의

# xml과 클래스 간 관계

- 아래와 같이 정의 된 데이터 객체는

```
 data class User(val firstName: String, val lastName: String)

```

클래스 파일 내에서, 

```
중략...

 binding.user = User("Test", "User") 와 같이 데이터를 세팅하면,

```

xml 파일 내에서, data 태그는 클래스 파일에서 사용하고자 하는 속성을 세팅하고, 
아래와 같이 view 내 @{} 구문을 통해서, 연결이 한다.

```
 <data>
           <variable name="user" type="com.example.User"/>
</data>

- 중략

<TextView android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="@{user.firstName}"/> 
```

## 회고

### 2020/03/29
- 확실히 databinding을 적용하지 않았을 때의 프로젝트와 비교했을때, findviewbyid를 사용하지 않아서 보일러 코드는 줄어들 것이다라는 생각이 들었다.
- 추후 다루겠지만, mvvm 패턴과 결합시에는 정말 눈에 띄게 코드가 줄어든다는 것을 알았다. 
- 개인적으로 xml 태그를 활용해 프로그래밍을 하는 게 신선하다. 흡사, 웹 프론트앤드 프레임워크를 조금 만져 봤을 떄랑 비슷한 느낌?(리액트를 아주 조금 사용해 본 경험을 토대로ㅠ)

