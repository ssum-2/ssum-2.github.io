---
title: "삼성 빅스비(Bixby)에 커스텀기능을 추가해보자; 빅스비 캡슐 만들기"
classes: wide
categories:
  - Study
tags:
  - AI
  - NLP
  - XXIT
author_profile : true
use_math : true
sidebar:
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-sample
last_modified_at: 2019-10-26
---

## Bixby X XXIT

----

- 빅스비 클라이언트?
  - 사용자의 액션을 받는 곳
  - 사용자의 음성데이터를 캐치해서 빅스비 서버로 올려줌
- ASR; 사람의 음성에서부터 유의미한 정보를 빼내는 역할
  - 빅스비 서버에 내장되어 있으며, 보이스 정보로부터 TEXT를 추출; STT
  - 사용하고 난 뒤에는 음성데이터는 삭제
- NLU; 자연어 이해
  - 빅스비 캡슐을 텍스트정보를 받았을 때 빠르게 처리할 수 있도록 간단한 분석을 해서 보내주는 역할
  - Kick ) NLP 분석시 장소와 기관이 겹치는 경우가 있음



- http://glideapps.com/
  - sample apps, 모바일 앱 개발 시 사용할 수 있는 디자인 샘플을 사용할 수 있음
  - template을 spread sheet로 작성해서 적용해볼 수 있음
- https://stackshare.io/
  - 유명 개발회사의 개발스택을 확인해볼 수 있음
- http://sheety.co/
  - 구글 스프레드시트에 정보를 넣고 File -> publish to the web -> style을 csv로 설정한 뒤 생성된 url을 복사해서 sheets에 적용 -> api 생성 ==> json타입 데이터를 확인할 수 있음
- https://github.com/bixbydevelopers
  - 깃허브에 각종 예제를 확인해 볼 수 있음
  - 외부 api를 호출할 때에는 api url을 입력해서 사용
  - POST url을 사용하고 싶을 때에는 가이드 참조
  - json형태 외 xml을 사용하고 싶을 때에는 format값을 "xmljs"로 설정
  - cacheTime은 캐시를 저장하는 시간, 보통은 60으로 설정함
    - 실시간으로 데이터를 가져와야할 때에는 실시간 서버에서 바로 데이터를 받아와야 하기 때문에 0으로 설정
    - 값을 지정해주지 않는다면 기본적으로는 60(1시간)으로 설정
  - returnHeaders
    - 바닐라 자바스크립트를 지원하지만 헤더 정보까지 주지는 못함 -> 에러를 받으려면 true로 설정해야함
- https://bixbydevelopers.com/dev/docs/reference/JavaScriptAPI
  - 사용할 수 있는 자바스크립트 모듈

# Bixby란?

- 음성을 통한 Intelligent Assistant Service & Platform

- 글로벌 브랜드의 음성 인공지능 서비스 : AWS 알렉스, Google 어시스턴트 외

- 빅스비는 빅스비 플랫폼 서버에서 동작하는 서버어플리케이션이기 때문에 클라이언트에서 별도의 설치가 필요하지않음

- 빅스비는 삼성인공지능 음성인식 서비스로써, 기본적으로 사용자의 ‘발화’를 인식하여 그 발화 안에서 각기 다른 유형의 변수들을 인지함으로써 내부 비즈니스 로직에 의해 결과값을 산출하는 서비스이다.

  - 절차: Client –> ASR(Automatic Speech Recognition) –> NLU(Natural Language Understanding) –> Capsule <--> External Server

  - ex) ‘오늘 날씨 알려줘’ 발화 전달
    –> ASR을 통해 빅스비 플랫폼 서버에서 발화 언어를 텍스트로 변환
    –> NLU에서 기본 분석 완료
    –> Capsule(빅스비 서버 어플리케이션 -> 서버를 따로 다운받지 않아도 됨) <--> External Server

  - ex) '맛집고고에서 주변 맛집 알려줘' 발화 전달

    -> 사용자의 발화가 빅스비 플랫폼 서버로 전달 -> 빅스비 맛집고고 **캡슐**(빅스비 서버 어플리케이션)에 분석한 정보를 전달 "주변/맛집/찾다" ->  적절한 발화 함수를 호출하여 정보 전달 -> 호출된 함수가 발화 정보를 분석 -> **사용자의 의도와 중요한 명령을 찾아냄 (모델링)** -> 실제로 사용자의 의도/명령에 맞는 action을 수행할 수 있는 **비즈니스로직**으로 정보를 전달 -> 정보를 찾은 다음 **UI/UX 레이아웃**을 입힘 -> 클라이언트에 결과를 전송

- training 

  - 모델이 사용자의 의도 분석을 더 정확하게 하기위해서 데이터를 추가하여 모델을 학습시키는 단계



# Bixby 시스템 구조

### Capsule design flow

- 발화 인식 –> 모델링 처리 + 비즈니스 로직 처리 + UI/UX 구현 작업 
- ex) ‘주변 맛집 찾아줘’
  –> 사용자의 발화에서 중요한 의도와 명령을 찾아내는 작업 실시
  –> (모델링) ‘주변’이라는 위치 정보와 ‘맛집’이라는 키워드 정보를 입력받아 특정 객체에 데이터 저장하기 위한 설계 (모델링: 중요한 명령과 의도를 파악하는 작업)
  –> (비즈니스 로직) 위치정보(주변) 및 기본정보(맛집)를 전달받아 찾은 결과(‘샐러드바 식당’)를 모델링에 전달 

### 모델링

- 모델링 작업 -> "빅스비 캡슐을 개발한다" 

- actions(*.model.bxb)
  - 캡슐이 사용자가 원하는 작업을 이해하도록 수행할 동작 정의
  - 빅스비 서버에 올라가있음.
  - 발화 함수
  - 가장 잘 판단할 것 같은 빅스비 캡슐에게 발화 전달 (이 때의 발화함수가 actions이다. 함수이므로 input, output 존재)
- concepts(*.model.bxb)
  - 발화 인식 및 발화 결과를 리턴할 때 필요한 값; 발화 변수
  - Input concepts; 사용자의 발화에서 필요한 정보를 담는 변수
  - output concepts; 비즈니스 로직에서 찾아낸 결과값을 사용자에게 전달하기 위해 담는 변수
  - ex) ‘햄버거 두개 주문해줘’
    –> 햄버거 : FoodName
    –> 2 : NumberOfFood

### 비즈니스 로직

- javascript code(*.js)
  - 모델링 단계에서 정의한 actions, concepts를 바탕으로 실제 서비스 코드 구현
  - 필요한 정보, 실질적으로 수행해야할 task를 통해 사용자가 원하는 결과를 도출하는 단계
  - 이 단계에서 서비스 API 연동 가능
  - 개발자는 메뉴와 주문 정보를 저장하는 서버 구축하여 API를 통해 캡슐과 연동

### UX/UI

- layout(*.layout.bxb)
  - layout-macro-def 함수를 작성하여, view 에서 필요한 부분에 원하는 layout-macro 호출 가능
  - css와 같은 역할, 디자인을 정의/설정, 스타일시트
- view(*.view.bxb)
  - 최종 결과를 사용자에게 보여주는 UX/UI 레이아웃 작업 진행
  - 큰 구조, 뼈대를 잡아주는 것

### 모델링 작성시 권장사항

유사한 Goal은 통합해서 최소화할 것

- 기존) 

  ```
  TurnUpVolume 소리 높여줘
  TurnDownVolume 소리 낮춰줘
  IncreaseBrightness 밝기 높여줘
  DecreaseBrightness 밝기 낮춰줘
  ```

- 개선)

  ```
  IncreaseSettingsValue 높여줘
  DecreaseSettingsValue 낮춰줘
    
  vocab (SttingsValue) {
      SoundVolume
      SoundBrightness
  }
  ```

### vocab 작성시 권장사항

1. 다른 vocab 들과 중복 지양

2. 한 entry에 다른 표현 방법이 있다면, 이들 역시 포함할 것

   ```
   vocab(PlaceName) {
       인천공항,
       인천 공항,
       인천국제공항,
       인천 국제 공항,
       인천 국제공항,
   }
   ```

3. enum vocab에 ‘key’값이 단어 set에서도 필요하다면, 단어 set 에도 한번 더 명시할 것

   ```
   vocab (CuisineStyle) {
       "한식" {
           "한식",
           "한식당",
       }
   }
   ```

4. 태깅 시 조사는 제외

   - 명사 tagging 시, 조사 제외하고 tagging 진행(“행”, “발” 등 제외)

5. 명사/동사 vocab

    

   - 기본적으로 명사를 tagging 하는 것이 권장되나, 명사 외 tagging이 필요하다면, flag나 role로 문장 전체에 tagging 가능
   - 필요하다면 동사 tagging 가능하나, 주의할 점은 vocab 생성 시 다양한 표현을 입력할 것



# Bixby Studio

### New Capsule 생성

- capsule.bxb 파일 생성 : 해당 capsule의 id(‘.’ 기준으로 앞이 팀명, 뒤가 캡슐명) 및 기타 정보 입력
  - Example.*** (test용)
  - playground.***(상용화 불가능 캡슐); private submission -> 휴대폰에서 테스트 가능
    –> 상용화하기 위해서는 캡슐 id 등록 필요
    –> [bixby developers site](https://chohyeonkeun.github.io/2019/08/25/190825-bixby-capsule-develop/bixbydevelopers.com) 접속
    –> 좌측 ‘Teams & Capsules’ 클릭
    –> ‘+ Create Team’ 클릭 및 팀명 입력
    –> ‘+ Register Capsule’ 캡슐명 입력
    –> ‘000(팀명).000(캡슐명)’ 캡슐 생성 완료
  - targets : 어떤 device에서 사용할 것인지 결정(ex. bixby-mobile-ko-KR -> language code 참고)
    –> resources의 training 옵션에 영향을 미침
- code(비즈니스 로직) 폴더, models(모델링) 폴더, resources(UX/UI) 폴더 생성
- 코드와 모델은 빅스비 서버로 전송되며, 코드가 수정되었을 때 수시로 서버에 있는 내용과 싱크를 맞추는 과정이 일어남
  - 네트워크가 좋지 않으면 싱크를 맞추는 과정에 차질이 있어 unable 오류가 발생할 수 있음

1. 워크스페이스 만들기 (캡슐 만들기)

   1. bixbydevelopers.com > teams & capsules 메뉴에서 팀을 만들어서 사용

      - https://bixbydevelopers.com/dev/marketplace/tensor/xxit

   2. File > New capsule > namespace 지정 (위에서 만든 팀네임과 네임스페이스 입력)

   3. 폴더 설명

      - code; 비즈니스 로직
      - models; 모델링
      - Resources; training, UX/UI 
        - Base; 설정값이 들어있는 폴더
          - Endpoints.bxb; 모델과 자바스크립트(비즈니스로직)을 연결하는 설정
        - 언어 분기 폴더; 언어마다 다른 보캡, 설정들을 관리
      - Cf) assets; 기타 다른 데이터 보관용(ex. image)

   4. 오류메시지가 뜰 수 있는데 이때 해당 파일을 바꿔줌

      capsule.bxb

      `capsule {

        id (tensor.xxit)

        version (0.1.0)

        format (3)

        targets {

      ​    *// client의 device 정보*

      ​    *// ISO, 2 char 국가정보를 사용하고 있음*

      ​    target (bixby-mobile-en-US)

      ​    target (bixby-mobile-ko-KR)

        }

        runtime-flags {

      ​    concepts-inherit-super-type-features

      ​    modern-prompt-rejection

      ​    support-halt-effect-in-computed-inputs

      ​    no-filtering-with-validation

      ​    modern-default-view-behavior

      ​    use-input-views-for-selection-list-detail

        }

      }`

      

- json이 나온 이유?
  - html만 쓰던 시절 정해진 태그 외 다른 태그를 쓰고자하는 의도에서 나온 것이 XGML
  - XGML은 너무 느린게 단점이었음 -> XML 등장
  - 더욱 경량화하고 딕셔너리 구조로 만든게 JSON
- Bearware; 라이선스의 한 종류로 소스를 사용하려면 맥주한잔 사라는 데서 비롯된 것



- templated의 종류
  - Stucture; 데이터 종류를 여러개 담을 수 있는 배열 같은 데이터타입
  - Name; 길지 않은 String

### 모델링 폴더 세분화

- models 폴더 하위로 actions, concepts 폴더 생성
  - actions : input과 output으로 이루어진 함수 (특정 js 파일이 실행될 때 input으로 받아야 할 변수명, type, min, max 값 정의 및 output으로 전달해야 할 객체 정의)
  - concepts : primitives + structures로 이루어진 각 변수들의 형 type 정의 (primitives, structures 폴더 생성)
    - primitives : name, integer, enum, text, boolean과 같은 형 type 정의
      - Enum : 가능한 모든 value 나열 가능 (ex. 한국 프로야구 구단)
        –> vocab에 관련 단어들 정의 필수
      - Name : 가능한 모든 value 나열 불가능하나, 이름으로 알 수 있는 형태(ex. 서울대 근처 식당 음식 이름)
        –> 가능하면 vocab 만들어서 늘어나는 training 개수 조절하는 것도 필요
      - Text : 가능한 모든 value 나열 불가능하며, 이름으로도 알 수 없음(ex. 핸드폰 알람 설정 내용)
    - structures : 여러 변수들을 갖고 있는 객체에 대한 property 정의
    - roll-of 와 extends의 차이
      - roll-of : 그 모델의 특성을 그대로 사용
      - extends : 그 모델의 특성에 무언가 추가하여 사용
- 캡슐 폴더 하위로 assets 폴더 > images 폴더 > 로고 이미지 파일 추가
- 모델링 파일은 맨 앞글자를 대문자로 사용(그 이후는 camelcase 형태)
  - 비즈니스 로직 관련된 것은 맨 앞글자를 소문자로 사용
- 모델링 파일의 description –> 해당 모델링에 대한 설명

### 비즈니스 로직 파일 생성

- code > *.js 파일 생성
  - 어떤 input 데이터를 받아와서 어떤 output의 데이터를 전달할지 비즈니스 로직 작성

### UX/UI 폴더 세분화

- resources 폴더 > base 폴더 > endpoints.bxb 파일 생성
  - actions <--> code 연결시켜주는 역할
  - 모델링해서 바로 외부서버와 연동해도 되는 경우, remote-endpoint에 바로 설정
- resources 폴더 > ko-KR 폴더 > training 파일 생성 (훈련 및 학습 역할)
  - training은 Goal당 30개 이상 권장
  - training 발화는 모두 ‘Learned’ 상태여야 함(‘Not Learned’는 대부분 vocab 오류)
- resources 폴더 > ko-KR 폴더 > dialog 폴더 생성 (상황별 화면에 조회될 메시지 전달 목적)
- resources 폴더 > ko-KR 폴더 > layout 폴더 생성 (사용자에게 제공할 UX/UI 레이아웃)
- resources 폴더 > ko-KR 폴더 > views 폴더 생성(사용자에게 제공할 UX/UI 뷰)
- resources 폴더 > ko-KR 폴더 > vocab 폴더 생성 (models > concepts > primitives에서 정의한 enum 변수에 대해 연관있는 모든 단어들을 정의)
- resources 폴더 > ko-KR 폴더 > capsule-info.bxb 파일 생성(캡슐에 대한 기본 정보 입력)
- resources 폴더 > ko-KR 폴더 > 000.hints.bxb 파일 생성(빅스비 캡슐의 원활한 학습을 위해 사용자로부터 예상되는 발화에 대한 문구 입력)
  - hints는 10개 이상 작성 권장



# Bixby 테스트

### 빅스비 캡슐의 학습 및 훈련 실시

- resources > ko-KR > training 클릭
- ‘Adding New Training’ 부분에 ‘햄버거 1개랑 콜라1개 주문해’ 발화 입력
- ‘Goal’부분에 발화가 실행되길 원하는 actions 이름 입력
- ‘NL’에서 input으로 받아올 데이터와 일치하는 부분의 발화 드래그 > concepts에서 해당 데이터 이름 입력
  - ex) ‘햄버거 1개 주문해줘’
    –> 햄버거 : FoodName
    –> 1개 : NumberOfFood
- 우측 상단의 ‘Compile NL Model’ 버튼 클릭
- 등록한 발화들의 상태가 Not Learned -> Learned 로 변환
- 특정 발화의 ‘Open In Simulator’ 클릭하여 예상 시뮬레이션 진행

### Training 시 권장사항

1. 표현이나 띄어쓰기는 최소한 ASR 기준으로 지원
   - 예를 들어, ‘삼성 뮤직’은 ASR에서 ‘Samsung Music’으로 변환되듯이 형태 달라질 수 있음
2. Goal 당 10개 미만의 학습은 지양
3. 다양한 형태의 parameter 학습
   - 단어수와 형태가 다양한 entry(숫자, 한글, 영어)들이 존재한다면 이들에 대한 학습 필수

### 모바일 빅스비에서 실제 발화 테스트

- 모바일 빅스비 > 설정 > 빅스비 보이스 정보 > 버전 부분을 톡톡 터치 > 개발자 옵션 on
- 모바일 빅스비 > 설정 > 개발자 옵션 > 디바이스로 테스트하기 > 사용중 > Revision ID > bixby studio에서 submission history의 ID 입력



# OAuth 연동 방법

현재 빅스비는 구글 OAuth만 연동 가능

1. 구글 사용자 인증정보 만들기 > OAuth 클라이언트 ID 클릭 > 웹 애플리케이션 선택 > 이름 입력 > aibixby.com 입력 > 승인된 리디렉션 URI > https://jonus-jonus.oauth.aibixby.com/auth/external/cb 입력 > 클라이언트 Id, 보안비밀 확인

2. capsule id를 capsule.bxb에 입력

3. 캡슐 폴더 하위로 authorization.bxb 생성

    

   ```
   authorization {
       // Google book api에 대한 OAuth 권한을 얻은 후, OAuth_URI, client_id, client_secret, Token_URI를 알맞은 정보로 변경
       user {
           oauth2-authorization-code (google){
           authorize-endpoint (https://accounts.google.com/o/oauth2/v2/auth)
           client-id (570500524886-a2hdpg3qvgt29avko6lvo4q3is28520i.apps.googleusercontent.com)
           client-secret-key (jonus) 
           //client-secret은 team -> capsule에서 설정 (https://bixbydevelopers.com/dev/docs/reference/ref-topics/capsule-config#config-secrets)
           scope (email profile openid https:/ /www.googleapis.com/auth/books)
           token-endpoint (https:/ /www.googleapis.com/oauth2/v4/token)
           }
       }
   }
   ```

