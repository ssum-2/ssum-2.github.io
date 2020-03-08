---
title: "LG CNS AI Tech Talk for NLU Day 발표 정리"
classes: wide
categories:
  - Conference/Seminar
tags:
  - AI
  - NLP/NLU
author_profile : true
use_math : true
sidebar:
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-sample
last_modified_at: 2019-09-05
---


## LG CNS; AI tech talk for NLU DAY

---

한국어 AI 연구

korQuad	1.0 / 2.0



### 1. 소개

   1. B to B
      - 기업에서 AI를 활용한 가치창출을 도움. 업무 혁신
   2. 연구주제
      - 시각지능, 언어지능 등
      - 시각지능; detection, ai 의 신뢰도, ai학습 자동화
      - 언어지능; LM을 가지고 챗봇과 데이터분석에 응용 --> korquad data set
      - 정량적데이터에 대한 ai 연구 개발 진행 중
   3. 딥러닝발전과 오픈데이터셋
      1. 딥러닝의 성장
         - 빅데이터/컴퓨터의 성능 향상
         - 공유하는 정신 -> 오픈소스 프로젝트
           - open access; 논문에 대한 공개
             - 지식에 대한 공유가 쉬워짐
           - open source
           - -> 논문에서 발췌한 알고리즘을 오픈소스로 구현 => 발전에 기여
           - 지속적인 개발/선순환
           - open dataset
             - ImageNet 이 없었다면 AlexNet도 없었을 것
             - 영어 오픈데이터셋은 많지만 한국어에 대한 오픈데이터셋은 없었음!
               - 내부적인 이슈로 korquad 데이터셋을 만들었음
               - 후속적으로 NIA등에서 오픈 데이터셋을 발표하기 시작!

### 2. 자연어처리 관련 학계 최신동향

1. 자연어처리 개요
   - 점점 딥러닝을 활용한 end to end 로 변해가고 있음
   - 자연어의 특정
     - 애매성(ambiguous) --> 이를 해소
     - 단어 하나가 차원이 됨 -> 고차원의 데이터 ---> 차원축소 필요 ---> 워드 임베딩
     - 시퀀스(단어의 열) --> 시퀀스에 대한 분류, 시퀀스 레이블링 태스크
     - sequence to sequence ; 한국어 단어 -> 인코딩 -> 영어 단어열로 변환...
2. 모델의 변화
   - RNN
   - LSTM RNN
   - Encoder-Decoder Model (기계번역)
   - attention mechanism
   - transformer; self attention
     - rnn대신 attention을 가지고 멀리 있는 정보도 직접 가져와 사고한다
     - 레이어를 12개까지 높게 쌓아서 구현할 수 있음
     - 기계번역쪽에서는 대부분 이 모델을 활용하고 있음
   - Transformer-XL
     - encoder를 떼어내서 LM에 적용
     - 시퀀스 입력열의 길이를 제한했던 것을 풀어서 좀더 멀리 볼 수 있게 확장함
     - 멀리있는 context까지 볼 수 있게 확장
   - Adaptive attention span in Transformers
     - 512보다 더 먼 컨텍스트까지 볼 수 있게 해야할 때
     - 각각의 head별로 컨텍스트를 볼 수 있는 거리 제한을 둠
       - ->메모리 양을 최소화
   - Learning Deep transformer models
     - 일반적인 레이어층보다 더 깊게 쌓음(30층까지)
     - normalization을 앞에서 진행
   - Bert
     - transformer에서 encoder만 따와서 자질을 추출할 수 있는 모델
     - GPT; decoder를 떼온 것 또는 LM에서 컨텍스트제한을 해제한 것
     - 대용량의 코퍼스로부터 pre training을 한 bert모델을 다양한 영역에서 파인 튜닝하여 적용한 사례가 많음
   - OpenAI GPT
   - XLNet
     - transformer encoder를 활용
     - LM처럼 앞의 맥락만 보는 게 아니라 순서를 바꿔서 뒤의 문맥까지 볼 수 있게함
     - bert보다 성능이 좋다고 함
   - RoBERTa
     - facebook에서 발표한 것
     - bert를 활용해서 학습을 잘~~ 하면 좋은 성능을 낼 수 있다(masked data를 잘 세팅하는 등)
   - XLM
     - 기계번역
     - Crossed lingual 분류문제에도 적용
   - MASS
     - transformer를 활용
     - 인코더와 디코더를 동시에 학습
     - 생성, 기계번역, 문서요약에 좋은 성능
   - HIBERT
     - 문서요약
     - 센텐스별 인코딩, 도큐먼트별 인코딩 수행
   - byte pair encoding
     - 인코딩 전에 데이터를 압축하는 방법
     - 한국어 어절 + BPE
     - BERT + BPE
     - 한국어 자연어처리에 적용할때에는 형태소로 나눈 다음에 BPE적용
       - unknown문제 해결
3. 딥러닝 기반 한국어 의미역 결정 (현 연구)
   - ETRI 의 Korbert 를 활용하여 NLP 연구 진행 중
   - korBert를 사용했을 때 전 분야에서 성능이 향상되는 걸 알 수 있었음
4. 향후연구
   1. Bert모델의 축소
   2. 한국어 자연어처리도 트랜스포머, 버트 모델을 기반으로 활발히 연구 중



### 3. KorBert

1. Bert 기술 소개
   1. 논문을 보는 방법
      - 논문에서 해결하고자하는 문제는 무엇인가
      - 딥러닝 모델의 경우 곱하기/더하기의 대상이 무엇인가? 차원이 어떻게 바뀌는가?
        - cnn - filter - max pooling
        - rnn - sigmoid로 섞이는 것
        - self attention - softmax - weigthed sum -> n개가 계속 유지
      - 모델의 구성요소가 무엇인가? 필수 미필수 구분
   2. 문제해결
      1. 기존의 워드임베딩; 단어를 실수 벡터로 표현해야하는 것 --> 문맥을 고려하지 못하는 한계
         - n개의 단어에 대해 뉴럴넷을 적용하여 문맥 반영 벡터로 활용
         - contextual representation
         - 대용량 raw데이터 -> self attention -> 단어간의 관계 파악 --> 실수벡터로 표현
      2. Pre training 단계
         - 단어에 masking후 단어를 맞추는 태스크
           - 양방향 정보를 이용한 단어 예측
         - 문장 선후관계 예측
         - 문장에 대한 정답을 자동으로 만들어주는게 버트 성능향상의 핵심
           - 마스킹의 비율을 몇 퍼센트로 했는가
   3. 트랜스포머 모델
      - 문장 -> 워드에 따라 각각의 차원으로 바뀜 -> 곱함 -> softmax -> n*n -> weigthed sum -> result
      - add&norm layer : Add는 역전파 때 오류를 전달, norm은 학습의 안정성
      - 위치정보를 주기 위해 position embedding 진행
   4. marsking 전략
      - 80%는 단어 그대로 마스킹
      - random word 10%

2. korBert 소개
   1. 엑소브레인 프로젝트
      - 변호사와 일반인의 소통을 돕는 것
   2. 최근 연구 결과
      1. ERNIE
         - 개체명 단위로 마스킹하여 학습
      2. BERT update
         - 임의의 단어를 마스킹하지 않고 공백단어 까지 전체 마스킹하여 학습
      3. span BERT
         - masking 할 때 확률 distribution에 따라서 임의의 개수만큼 (최대 10개) 마스킹
         - 앞단어, 뒤단어의 워드벡터와 현재 히든 벡터를 합쳐서 예측을 도움
      4. RoBERTa
         - 매번 다르게 마스킹하여 학습
         - segment 단위 / sentence / full sentence 단위 / doc-sentence
         - batch 8K /  steps 100K --- gpu 1024gb 10개로 하루 소요...
   3. 한국어에 적합한 vocab 구축 및 코버트 학습
      - 끝부분에 _로 구분하여 단어집합을 만든 것이 가장 성능이 좋았음

3. 향후 연구 관점
   1. 실시간 서비스를 위한 속도 개선
      - 256단위로 시퀀스를 줄이면 속도 빨라지고 성능도 유지
      - 모델사이즈 줄이기
      - 버트의 모델적 한계를 극복하는 연구
      - 외부지식 /메모리 활용 관련 연구
      - cross encoding 문제 해결
        - poly, bi encoder로 수행 --> 성능은 유지되면서 속도가 빨라짐
   2. 미해결문제
      - 단락을 보지 않고 질문을 만들기
      - 여러 문맥을 보고 답을 찾을 수 있는 문제
      - 정보 검색까지 포함해서 end to end 로 진행하기
4. 질문
   - google gpu 사용했었음
   - 토큰화 방식 비교?
     - 전부 다 해보고 acc가 가장 괜찮았던 방법으로 채택
   - 파인튜닝의 유의점
     - 파라미터를 어떻게 썼는가

### 4. On device AI

- 모바일에 ai를 적용하는 분야로 확장 중임
- 스냅드래곤 885 플러스
- bayesian deep learning 적용
- tensorflow lite
- Iot device
- 각 디바이스간 통신으로 수집한 데이터를 통해 공동으로 학습할 수 있게 개발 진행 중



### 5. korQuad

1. 1.0

   1. 개요

      - Korean Question Answering Dataset
      - 대규모 기계독해 데이터셋

   2. 데이터셋

      - 한국어 위키백과 문서를 수집 -> 문단단위로 정제 -> 300자 미만, 수식 포함된 경우 제외

      - 크라우드 소싱을 통해 1만쌍의 데이터를 만듦

        - 한문장에 대해 2~3 문장씩 만들 수 있도록 함 

      - <iframe src="//www.slideshare.net/slideshow/embed_code/key/qUKS3TThkbgSim" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/SeungyoungLim/korquad-introduction" title="KorQuAD introduction" target="_blank">KorQuAD introduction</a> </strong> from <strong><a href="https://www.slideshare.net/SeungyoungLim" target="_blank">SeungyoungLim</a></strong> </div>

   3. 리더보드

      - 현재 75건의 제출이 있었으며 클로바가 최다
      - 대부분의 참가자가 bert를 사용

   4. 참가자 발표

      1. 솔트룩스 김성현
         - 정형/비정형 데이터 수집 -> 지식 추출 -> 지식그래프 확장
           - 기계독해 기술을 활용하여 지식 추출
         - 기계독해 프로세스
           - 비정형데이터에서 지식그래프를 그림 (마인드맵 형태)
           - 주요 entity를 추출 -> 이를 중심으로 지식그래프를 형성하여 정답 판별
         - korBERT활용
           - 하루 5000건의 제한이 있음
             - 다른 오픈 api를 활용하여 형태소분석을 진행한 후 데이터를 입력하는게 좋음
             - Mecab, twitter는 형태 유지가 안됨
             - khaii를 코버트와 함께 사용하는 것이 좋다
           - 문장을 복원하는데 유리함
             - json에 key값으로 호출 가능
         - 버트 성능 영향에 미치는 요인
           - 코퍼스 사이즈
             - 단락분리가 정확히 이루어져야 성능 향상
           - 코퍼스 도메인
           - vocab 사이즈
           - 토크나이징 방식
             - word piece, 형태소 단위
           - 형태소 원형분리 + 형태소 분리 + 태깅 + word piece 토큰인 경우 가장 성능이 좋았음
         - entity 정보가 중요
           - 주요 entity 추출, entity tag부착
           - entity embedding layer 추가
         - entity의 속성정보를 추가 --> ner
           - 서로 다르게 임베딩해서 넣기
           - entity linking module 개발
         - 새로운 도메인에 적용하는 버트
           - vocab을 고쳐서 하기는 오래걸리기 때문에 다이내믹 임베딩 레이어를 만들어서 추가된 vocab을 학습할 수 있게 만들고 있음
           - F1 score는 향상되었지만 em은 아직 성능향상이 이뤄지지 않음
         - 특정 도메인에 특화된 버트 모델
           - ERINE 가 현재 가장 성능이 좋았음
         - triple을 자동으로 수집하는 방법?
           - 위키를 대상으로 80퍼센트정도 추출되는 수준이고 마지막에 qa에서 검수
         - 코퍼스에서 문서간 분류 하는 방법
           - 위키; 파싱된 데이터에서 구분되어있는 것
           - 책인 경우 챕터별, 단락구분 되어있는 것을 따름
         - 구글 gpu를 사용하여 1~2개월정도 소요
         - 다이내믹 임베딩 레이어
           - vocab사이즈만큼 워드 임베딩 레이어가 생성
           - 이 사이즈가 고정값인데 새로운 vocab이 등장하면 기존 것과 사이즈를 비교,
           - 임베딩레이어부분만 단순하게 늘려서 학습할 수 있게 만든 것
         - Dynamic Embedding을 사용 하면, 구성 단어들과 배열(word index)이 달라질 것 같은데 이를 각 domain corpus 마다 어떻게 맞추는지
           - 뒤에 연이어 붙이기 때문에 기존단어의 인덱스는 변하지 않음
      2. 네이버 클로바 이동준
         - 참여계기
           - pre trained model을 파인튜닝하는 현재 트렌드에 맞춰 시작
           - BERT의 성능을 검증하기 위한 방법(downstream task) 수집 중 korquad 차용
         - V100을 이용하여 분산처리, FP16등 옵션을 사용하여 학습 진행중
         - BERT는 너무 크기 때문에 하나의 모델을 학습한 뒤 검증하면 너무 오래걸림
           - -> 매 스텝마다 스토리지 서버에 업로드, 트리거를 통해 자동으로 다운스트림 태스크가 돌아감 -> 벤치마킹 서버에 올라감
           - pytorch
         - 효과적이었던 방법론
           - 마스킹전략
             - Basic / entity / phrase
             - space level 로 마스킹(어절)
           - 모델 사이즈
             - 모델사이즈가 크면 클 수록 성능 향상
             - 레이어층 24에서 가장 효과가 좋았음
           - 파인튜닝
             - data augmentation
               - 같은 형태의 데이터를 먼저 학습시키고, 코쿼드 데이터셋으로 학습을 진행 한 후 검증
               - 기존 데이터만 사용한 것보다 0.5정도 향상
             - tokenizer CLaF
               - 최대 문장길이가 정해져있다는 본질적인 한계
                 - 최대 문장길이를 넘는 질문에 대해 doc_stride 단위로 문장을 자름 --> 의미를 상실하는 토큰 발생
                 - sentTokenizer로 자름 ==> 하나의 문장이 온전한 의미를 가지게 됨 => 문장을 기준으로 bert의 input을 구성하여 손실데이터를 줄임
             - NSML; AutoML ; GPU 인프라 서비스
               - 하이퍼파라미터 searching 기능
               - 모델에 따라 하이퍼파라미터 범위만 정해주고 autoML을 돌리면 자동으로 잡아줌
               - https://n-clair.github.io/vision-docs/_build/html/ko_KR/index.html
         - 토크나이저를 무엇을 썼는가
           - Word piece 단위로 구성, character 커버리지 설정 (0.995),
         - 60기가 정도 되는 프리트레이닝 데이터
         - character corverage
           - 최대 어느 정도의 음절을 커버할 것인가를 설정하는 것
           - 처음에 1로 했을 때는 이상한 외계어가 포함되었음...
           - 0.995를 사용
         - RoBERTa에서의 파인튜닝

2. 2.0

   1. 소개
      - 1.0과의 차이점
        - 짧은 문단이 아니라 문서 전체에서 질문에 대한 답을 찾아야함
        - 구조화된 문서(표)에서도 답을 찾아야함
        - 문단과 같이 긴 답변도 가능
   2. 데이터 수집과정
      - 수집
        - 위키피디아 html 크롤링; 페이지 기준 상위 15만건 + 임의로 선정된 5만건 추가
        - 정보가 없는 부분은 제거하고 텍스트가 있는 문단에서만 질문을 생성
      - 질문 답변 생성
        - 크라우드 소싱을 활용하여 8만쌍 제작
        - 소제목 단위로 잘라서 디테일한 부분에서 질문답변을 생성하도록 함
        - 일정부분이 본문과 중복되는 경우는 제출하지 못하도록 제약
      - 기존 데이터를 변환
        - 짧은 문단이었던 것을 전체 문서로 변환
        - 약 2만건
   3. 문서 및 질문 답변 유형 분석
      1. 데이터구성
         - 문서당 2건 정도의 질문이 매핑
         - 도메인에 제약이 없는 광범위한 데이터
         - 답변길이에 따라 short/long
         - 답변이 어디에 존재하느냐에 따라 text <p> . Table<table>, list <ul>
      2. 데이터 분석
         - 문서 길이와 답변 길이가 이전 버전보다 훨씬 늘어남
   4. 베이스라인 성능, 휴먼 퍼포먼스
      - Em: 실제 정답과 정확하게 일치하는 예측치의 비율
      - F1: 어절 단위에서 실제 정답과 예측치의 겹치는 부분을 고려한 점수
      - Latency: 데이터 전처리, 모델 추론을 포함한 질문 하나당 평균 소요 시간
      - 베이스라인은 구글 버트를 그대로 사용한 것
   5. 질의응답
      - Conversational QA 혹은 Knowledge based QA 등의 데이터를 발표할 예정은 있음
      - 

### 6. 논문발표

1. 네이버 클로바; real time open domain QA with dense sparse phrase index

   - phrase representation
     - dense representation; entity를 구분하기 어려움
     - sparse representation; Bow, 단어개수만큼 차원이 생김 --> 1000만이 넘어가는 차원
     - 질문 -> dense, sparse를 모두 인코딩, 학습
   - text encoder 로 bert 사용
     - Coherency vector
   - sparse representation
     - TF-IDF 사용
     - uni gram, bi gram 사용
     - DrQA's vocabulary/TF IDF 사용
   - scale
     - 4kb * 60 billion (phrases)...  240TB
     - 4 GPUs, 64GB ram, 2 TB storage, One week으로 시도...
   - 학습과정
     - closed domain QA dataset
     - paper 내용 참조
   - storage
     - pointer방식으로 저장 -> 12TB로 축소
     - filter -> 4.5TB
     - sclalar quantization ->4pyte 당 1dimension ---> 1.5TB
   - search
     - dense first search -> dense 부터 찾고 ranking
     - Sparse first search
     - dense sparse search
   - DenSPI 데모 실행 가능
     - http://104.154.208.227:5900
   - 연사 깃허브: https://github.com/seominjoon?tab=repositories

2. Textbook Question Answering with Multi-modal Context Graph Understanding and Self-supervised Open-set Comprehension

   - 교과서에 있는 내용을 토대로 QA
     - 정제가 많이 되지 않은 데이터; 텍스트 이미지의 혼합형
     - 데이터셋 다운은 페이퍼에 명시된 출처 확인
   - 본문을 살펴보는 - skimming step
   - 문제를 본격적으로 해결하는 - solving step
   - input이 다양하기 때문에 이에 맞는 다양한 임베딩 방법을 시도함
     - 그림은 다이어그램을 파싱해서 그래프를 만듦
       - -> 왜 이렇게 되었는가에 대해 설명가능해지는 효과
     - 텍스트는 dependency parsing
     - 공통의 질문에 대해 다양한 답변이 주어져있다면 이에 따라 각 문단마다 correct/wrong 판별
   - 마지막은 rnn을 넣음 ; 7지선다를 풀기 위해
   - **self supervised comprehension**
     - Training set에서 학습한 모델을 valid set을 가지고 한번 더 학습을 시켜줌
       - 답을 알려주지는 않음! 새로운 유형과 주제에 대해 학습하는 과정
       - Q-A에 대해 가장 관계가 높은 paragraph를 찾게끔 문제를 바꿈; TF-IDF가 가장 높은 것bi
   - Word embedding, char embedding --> biLSTM을 태움

3. episodic memory reader learning what to remember for question answering from streaming data

   - https://www.aclweb.org/anthology/P19-1434

   - streaming data가 들어올 때 유의미한 데이터를 추려내서 저장하는 방법을 고안

     - Question Answering을 기반으로 해결

     - supporting fact; query를 해결하기 위한 단서 

       --> 이를 external memory에 남기는게 이 과제의 핵심

     - 어떤 데이터가 어떤 시점에 중요한지 모델링해서 메모리에 최소한의 데이터만 저장되게끔 구성

   - Query --> external memory ---> result

     - em은 data stream 개수 m보다 크게 잡지 않음

   - episodic memory reader

     - 강화학습 모델
     - 미래의 알 수 없는 질문에 대해서도 답변 가능한 일반적인 단서만 메모리에 남기는게 목표
     - streaming data가 강화학습의 state가 됨
     - ![image-20190905173836561](/Users/ssum/Library/Application Support/typora-user-images/image-20190905173836561.png)

   - memory encoder

     - EMR independent; 입력 데이터가 각 메모리와 독립적으로 비교되어서 각각의 중요도를 선별
       - 입력데이터는 탈락되지 않음
       - 이전의 입력데이터로부터 메모리의 값들 간의 중요도의 누적치도 저장
       - ![image-20190905174051578](/Users/ssum/Library/Application Support/typora-user-images/image-20190905174051578.png)
     - EMR bi GRU
       - information이 깊어질수록 사라질 수 있음 --> 동등한 비교 불가
     - EMR transformer
       - 모든 메모리가 동등하게 비교될 수 있었음

   - 메모리가 꽉 찬 경우

     - 어떤 것을 지울지 판단 --> EMR이 input과 메모리데이터를 읽어서 policy를 출력
     - policy ; 어떤 메모리 엔트리가 중요하지 않은 지 출력
     - 폴리시에서 제시된 중요도가 낮은 엔트리는 삭제되고 새로운 데이터 인스턴스가 replace됨
     - 주어진 스트림데이터의 시간순이 중요(시계열 데이터)해서 순차적으로 들어가도록 설계(queue)

   - 학습방법

     - 데이터 스트림이 끝날 때마다 학습
     - f1 score로 reward
     - 이후 강화학습을 위해 각 시간마다의 리워드와 action probabilities를 저장
     - 과정이 다 끝나면 QA loss를 계산, 이를 통해 학습 완료

   - RL loss

     - Actor-critic learning loss
     - entropy loss 를 사용

   - baseline

     - FIFO
     - Uniform
     - LIFO
     - EMR independent

   - dataset; 3가지 데이터셋 사용

     - bAbI Dataset; syntetic한 데이터셋, 태스크 2만 사용, noisy task2
       - Men2m
     - triviaQA dataset; 더 긴 문장에 대한 학습을 위해 사용
       - BERT
       - span prediction
     - TVQA; 이미지가 들어가는 데이터에 대해서도 학습
       - Multi-stream
       - video frame + subtitle

