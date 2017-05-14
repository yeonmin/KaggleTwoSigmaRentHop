**Kaggle Competition**에서 배운 내용을 개인적으로 정리하기 위하여 사용하였습니다.<br>
틀린 내용이나 자세한 문의사항은 메일로 주시기바랍니다.<br>

# Two Sigma Connect: Rental Listing Inquiries
https://www.kaggle.com/c/two-sigma-connect-rental-listing-inquiries

## 문제 설명
New York 시의 부동산 선호도 예측 문제<br>
Finding the perfect place to call your new home should be more than browsing through endless listings. RentHop makes apartment search smarter by using data to sort rental listings by quality. But while looking for the perfect apartment is difficult enough, structuring and making sense of all available real estate data programmatically is even harder. 

## Kaggle 진행과정 요약
![Alt text](twosigma_kaggle_process.png?raw=true "kaggle process")

**1. Feature Engineering**
>* Kernel과 Discussion을 많이 읽어보고 괜찮은 Feature들을 지속적으로 추가하고 검증
>* DataSet을 살펴보고 지속적으로 파생변수를 생성하고 검증<br><br>**검증하는 방법**<br>파생변수를 추가하고 학습(LightGBM 추천)하였을 때 Cross Validation Score가 개선되는지 확인 (이 전 결과 대비)<br><br>
>* 처음에는 EDA를 진행하였지만 대회 후반부에는 시간이 부족하여 파생변수 추가하고 바로 검증하는 방식으로 진행


<br>**2. Single Model Tuning**
>* LightGBM과 Xgboost를 사용함
>* https://github.com/Microsoft/LightGBM
>* LightGBM Base Code: https://www.kaggle.com/somnisight/microsoft-lightgbm-starter
>* 둘다 성능은 비슷하게 나왔지만 속도는 LightGBM이 좀 더 빨랐음


<br>**3. StackNet**
>* https://github.com/kaz-Anova/StackNet
>* Single Model의 결과를 Input Data Set에 추가하여 새로운 Data Set을 생성<br><br>**StackNet Input이란?**<br>
Input Data Set이 100개 Feature를 가지고 결과값이 3개의 Multi LogLoss일 경우 결과 값을 Input Data Set에 합쳐 103개의 Feature를 만듬
<br><br>
>* StackNet을 사용하여 학습 진행
>* i7, 8gb ram, gpu없음, ssd에서 1시간정도 소요됨
>* StackNet에서 제공된 Two Sigma용 Prameter를 Tuning해봤지만 크게 좋아지지 않았고 기존에 Base로 제공된 Parameter파일에서 각 알고리즘의 Iteration만 2배로 하였을 경우 조금 향상되었음

<br>**4. Weighted Average Stacking**
>* 여러가지 다른 Feature Set을 가지는 여러 Model들을 합쳐 결과가 좋았던 것에 좀 더 가중치를 주고 Stacking 진행
>* 단순하게 A 결과, B 결과, C 결과가 있었을 때 A 결과가 좀 더 좋았다면 (A+A+B+C) / 4 처럼 하여 결과 제출
>* https://mlwave.com/kaggle-ensembling-guide/
