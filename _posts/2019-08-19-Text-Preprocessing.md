---
title: "Text Preprocessing"
classes: wide
categories:
  - Study
tags:
  - AI
  - NLP
author_profile : true
use_math : true
sidebar:
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-sample
last_modified_at: 2019-08-19
---

## 텍스트 전처리(Text preprocessing)
> 용도에 맞게 텍스트를 사전에 처리하는 작업	

---

1. 토큰화(Tokenization)

   - 코퍼스 데이터를 사용 용도에 맞게 토큰(token) 단위로 나누는 작업
   - 보통 의미있는 단위로 토큰을 정의

2. 단어 토큰화(word tokenization)

   - 토큰의 기준을 단어(word)를 기준으로 하는 경우 또는 단어구, 의미를 갖는 문자열로 나누는 경우

     > Ex. 구두점(punctuation: . , ? ㅣ ! 등)을 제외시키는 단어 토큰화 작업
     >
     > Input: Time is an illusion. Lunchtime double so!
     >
     > Output: "Time", "is", "an", "illusion", "Lunchtime", "double", "so"
     >
     > > 구두점을 지운 뒤 띄어쓰기(whitespace)를 기준으로 잘라냄

   - ! 구두점이나 특문을 모두 제거하면 토큰의 의미를 잃어버리는 경우가 발생

   - ! 한국어는 띄어쓰기로 단어 토큰을 구분하기 어려움

3. 토큰화 중 생기는 선택의 순간

   - 용도에 따라 토큰화의 기준을 선택해야하는 경우가 발생함
   - 아포스트로피(')가 있는 단어를 어떻게 토큰화해야할지 고민하는 문제

4. 토큰화에서 고려해야할 사항

   - 구두점, 특문을 단순 제외해서는 안됨

     - 온점의 경우 문장의 경계를 알 수 있는데 도움이 되므로 제외하면 안됨
     - 의미단위의 단어 자체에 구두점을 포함하는 경우도 존재(날짜, 달러 등)
     - 숫자 사이에 구두점(금액)

   - 줄임말과 단어 내에 띄어쓰기가 있는 경우

     - 영어에서의 아포스트로피('), 접어
     - 의미단위로 하나의 단어지만 띄어쓰기가 있는 경우 존재 (지역명사 등)

   - 표준 토큰화 예제

     - Penn Treebank Tokenization 규칙

       > > 하이픈(-)으로 구성된 단어는 하나로 유지
       > > doesn't과 같이 접어가 함께하는 단어는 분리

5. 문장 토큰화(Sentence Tokenization)

   > 갖고있는 코퍼스 내에서 문장 단위로 구분하는 작업; 문장 분류(sentence segmentation)

   

https://public.oed.com/how-to-use-the-oed/abbreviations/

