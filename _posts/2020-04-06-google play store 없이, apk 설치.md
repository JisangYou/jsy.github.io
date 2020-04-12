---
layout: post
title:  "google play store 없이, apk 설치"
date:   2020-04-06 23:00:00 +0900
categories: Android
---

앱 개발을 하다보면, 인텐트는 필수적으로 사용하게 되는데요. 그만큼 중요하며 기능도 다양합니다.
다양한 기능들 중에, 이번에 이슈 처리간 기억에 남는 기능을 간단하게 소개할까 합니다. 


아래는 간단한 인텐트의 정보입니다.

## 인텐트의 주요 역할

- Activity 시작
- Service 시작
- Broadcast 시작

## 인텐트 유형

- 명시적
명시적 인텐트는 앱 내의 특정 액티비티나 서비스 등 특정한 앱 구성 요소를 시작하는 데 사용함

- 암시적
암시적 인텐트는 작업을 지정하여 기기에서 해당 작업을 수행할 수 있는 모든 앱을 호출할 수 있도록 함.

# issue : google play store 없이, 프로젝트 업데이트하기.
- 업무 간 위와 같은 요청을 처음 받았을 떄, '이게 가능할까?' 라는 생각이 들었는데, 
조금 생각해보니 [Apk를 다운로드 받아서 앱내에서 실행만 시켜주면 되는 것]이 었습니다.

## flow
- 
사내 파일 서버에 apk 파일 세팅 -> 
앱 내 프로젝트 파일 밑에 apk 다운로드 후 저장 -> 
코드 내에서 프로젝트 버전 체크 후 최신버전이면 apk 실행해서 프로젝트 업데이트 시키기

## key point

- intent._setDataAndType_( {apk file uri} , "application/vnd.android.package-archive") -> 이 코드를 통해 Apk를 실행시킬 수 있었다.

```
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setDataAndType(Uri.fromFile(new File(Environment.getExternalStorageDirectory() + "/download/" + "app.apk")), "application/vnd.android.package-archive");
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```

출처: https://stackoverflow.com/questions/4967669/android-install-apk-programmatically/4969421#4969421


## 회고

### 2020/04/06

- downloadmanager를 사용해서 다운로드 기능을 만들어봤는데 굉장히 편리하고 유저들은 노티를 받아볼 수 있는 기능이 심플하게 제공되기에 
개발자에게도 좋고, 사용자 입장에서도 노티를 받아볼 수 있기에 '사용자 경험' 측면에서도 좋은 것 같다.
- 샘플 코드들을 정확한 분석없이 사용하다 보니, 생각보다 시간을 많이 허비했다.(File provider, file 저장경로)
- intent를 관례적으로 사용만 했지, 그 의미를 자세히 몰랐기 때문에 이슈를 처리하는 데 헤메였다.
- intent관련해 틈틈히 정리할 내용은 메모해서 업데이트 할 것.