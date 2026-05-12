# Bayesian Optimization

- 기존의 parameter 튜닝 방식

1) GridSearchCV()

: 사용자가 지정한 hyperparameter 값들의 모든 조합 탐색
(탐색할 매개변수와 값의 리스트를 dict로)

  
  adv. 최적의 해를 구할 수 있다
  
  dis. type이나 range가 클수록 매우 많은 시간 소요

=> best_estimator_에 모델 저장

=> best_params 를 통해 parameter 알수 있음

=> cv_result_ ['mean_test_score'] 를 통해 각각의 파라미터별 교차검증 스코어 평균을 알 수 있음

2) Random search

: 내가 준 범위 내에서 n_iter만큼 랜덤하게 샘플링해서 하이퍼파라미터 튜닝

--> 속도가 빠르고 input을 확률분포로 받음

--> gridsearchcv()와 동일하게 모델 저장, 파라미터 등등 메소드 동일

- Bayesian Optimization

: 미지의 함수 (black-box function)가 반환하는 값의 Maximum(or minimum) 값을 짧은 반복을 통해 찾아내는 최적화 방식

- hyperparameter 조합과 그에 따른 모델의 성능의 관계 = black-box function (모르기에 black-box)

--> 주로 DL보단 ML에서 사용되는 방법론   

-용어

objective function: 우리가 최대화(최소화)하려는 값을 돌려주는 함수 (최적화 대상)

Black-box function: 내부 구조를 모르고 입력을 넣어봐야만 출력을 알 수 있는 함수

Gaussian Process: 함수를 하나의 확정된 곡선이 아닌 '가능한 곡선들의 확률분포'로 표현하는 모델

Surrogate model

: 지금까지 추론한 결과들을 바탕으로 미지의 함수 f(x)의 대략적인 값을 추정 

<img width="1192" height="542" alt="image" src="https://github.com/user-attachments/assets/7c407b44-191c-4fa4-9625-1025aa1429cb" />

검은 색 실선: 관측 데이터들을 바탕으로 예측한 추정 함수의 expectation  

회색 영역: 미지 함수 f(x)가 존재할 신뢰 구간

--> 대략적인 값을 추정할 때 Gaussian process 사용

Acquisition funciton

: surrogate model의 확률분포를 이용해서 나온 확률분포를 이용해 지금까지 나온 값들보다 더 큰 값이 나올 가능성이 제일 높은 점을 알려주는 함수

Exploration(탐색) vs Exploitation(활용)
(trade-off 관계)

+Exploration: 불확실성이 가장 높은 곳에서 지금까지 나온 값들보다 더 좋은 것이 있을 거라는 전략

+Exploitation: 지금까지 나온 것들 중에서 높은 값들 근처에서 더 좋은 것이 있을 거라는 전략

--> 적절하게 균형을 잡도록 만든 acquisition funciton이 Expected Improvement 함수 

--> 말고도 UpperConfidenceBound 함수 존재 

-Bayesian Optimization의 전체 프로세스
1. 초기값 몇 개로 실제 '미지의 함수'를 실행하여 관측 데이터를 확보
1. 이 관측 데이터를 바탕으로 Surrogate Model을 만듦
3. Surrogate Model을 기반으로 Acquisition Function을 계산
4. Acquisition Function 값이 최대가 되는 지점(다음 탐색 지점)을 찾습니다.
5. 이 지점의 하이퍼파라미터로 '미지의 함수'를 실제로 실행하여 새로운 관측 데이터를 얻
습니다.
6. 이 새 데이터를 추가하여 Surrogate Model을 업데이트
(본문 그림처럼 해당 지점의 회색 영역이 줄어들고 검은색 실선이 실제 함수에 가까워
짐)
7. 정해진 반복 횟수(n_iter)만큼 2~6번 과정을 반복

