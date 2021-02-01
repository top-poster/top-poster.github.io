---
layout: post
title: "기계 학습 모델을 종속성이없는 네이티브 코드로 변환하는 방법
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/dandelion-2817950_1920.jpg
tags: undefined
---


대부분의 훈련 된 기계 학습 모델은 피클 파일로 저장됩니다.
 이 파일 유형은 Python에서 객체를 직렬화 및 역 직렬화하는 표준 방법입니다.
 

예측을 수행하려면 저장된 학습 된 모델을로드 한 다음 제공된 입력에서 예측을 수행해야합니다.
 

이 기사에서는 m2cgen Python 라이브러리를 사용하여 학습 된 기계 학습 모델을 종속성이없는 네이티브 코드 (예 : Python, PHP 또는 JavaScript)로 변환하는 방법을 알아 봅니다.
 그런 다음이를 기반으로 예측을합니다.
 

## m2cgen Python 라이브러리 란 무엇입니까?
 

m2cgen (모델 2 코드 생성기)은 훈련 된 기계 학습 모델을 다른 프로그래밍 언어로 변환하는 간단한 Python 라이브러리입니다.
 

예를 들어 Scikit-learn 라이브러리에서 기계 학습 모델을 학습 한 다음 선택한 프로그래밍 언어로 변환 할 수 있습니다.
 

이 라이브러리는 모델 예측을 지원하기 위해 Python 스택을 설치할 수없는 환경에 모델을 배포하려는 경우 매우 유용합니다.
 

### m2cgen 라이브러리에서 지원하는 언어
 

M2cgen은 14 가지 프로그래밍 언어를 지원합니다.
 

- 씨
 verified_user
- 씨#
 verified_user
- 다트
 
- 에프#
 
- 가다
 verified_user
- Haskell
 
- 자바
 
- 자바 스크립트
 
- PHP
 
- PowerShell
 
- 파이썬
 
- 아르 자형
 
- 루비
 
- Visual Basic (VBA 호환)
 

### m2cgen 라이브러리에서 지원하는 모델
 

이 라이브러리는 Scikit-learn의 다양한 회귀 및 분류 모델과 XGBoost 및 LightGBM (Light Gradient Boosting Machine)과 같은 다양한 그라디언트 부스트 프레임 워크를 지원합니다.
 

지원되는 다른 모델에 대해 알아 보려면 https://github.com/BayesWitnesses/m2cgen#supported-models에서 확인하십시오.
 

## m2cgen Python 라이브러리를 설치하는 방법
 

m2cgen을 설치하려면 터미널에서 다음 명령을 실행하십시오.
 

```undefined
pip install m2cgen
```

m2cgen은 Python 버전 3.6 이상에서 지원됩니다.
 

## m2cgen Python 라이브러리 사용 방법
 

다음 예에서는 대출 데이터 세트를 사용하여 LogisticRegression 알고리즘을 사용하는 간단한 기계 학습 모델을 만듭니다.
 알고리즘은 고객이 대출 금액을받을 자격이 있는지 예측할 수 있습니다.
 

그런 다음 m2cgen 라이브러리를 사용하여 훈련 된 모델을 Python, PHP 및 JavaScript로 변환합니다.
 여기에서 데이터 세트를 다운로드 할 수 있습니다.
 

시작하자!
 🚀
 

이 사용 사례를 위해 다음과 같은 중요한 패키지를 가져옵니다.
 

```python
import pandas as pd
import numpy as np                     
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.model_selection import train_test_split 
from sklearn.linear_model import LogisticRegression
import m2cgen as m2c 
import warnings                        # To ignore any warnings
warnings.filterwarnings("ignore")
```

다음 명령으로 Pandas를 사용하여 대출 데이터 세트를로드합니다.
 

```python
data = pd.read_csv("data/loans_data.csv")
```

그런 다음 데이터 세트의 모든 열 목록을 표시합니다.
 

```python
list(data.columns)
```

다음은 우리가 관심을 가질만한 열입니다.
 

Loan_ID
성별
기혼
부양 가족
교육
자영업자
신청자 소득
CoapplicantIncome
대출금
Loan_Amount_Term
신용 기록
Property_Area
Loan_Status
 

12 개의 독립적 인 기능과 하나의 목표 (Loan_Status)가 있습니다.
 여기에서 각 기능에 대한 설명을 읽을 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_H192S1SuTPt0AVdxNwHdQA.png)

다음은 데이터 세트의 처음 5 개 행입니다.
 

```python
#show the first 5 rows of the dataset
data.head() 
```

![image](https://www.freecodecamp.org/news/content/images/2021/01/5-rows.PNG)

보시다시피 데이터 세트에는 숫자 값으로 변환해야하는 일부 누락 된 데이터와 범주 기능이 있습니다.
 다음은 누락 된 데이터 및 기능 엔지니어링을 처리하는 데 도움이되는 간단한 Python 함수입니다.
 그런 다음 처리 된 기능과 대상을 반환합니다.
 

```python
# preprocessing the dataset.

def preprocessing(data):

    # replace with numerical values
    data['Dependents'].replace('3+', 3,inplace=True)
    data['Loan_Status'].replace('N', 0,inplace=True)
    data['Loan_Status'].replace('Y', 1,inplace=True)

    # handle missing data 
    data['Gender'].fillna(data['Gender'].mode()[0], inplace=True)
    data['Married'].fillna(data['Married'].mode()[0], inplace=True)
    data['Dependents'].fillna(data['Dependents'].mode()[0], inplace=True)
    data['Self_Employed'].fillna(data['Self_Employed'].mode()[0], inplace=True)
    data['Credit_History'].fillna(data['Credit_History'].mode()[0], inplace=True)
    data['Loan_Amount_Term'].fillna(data['Loan_Amount_Term'].mode()[0], inplace=True)
    data['LoanAmount'].fillna(data['LoanAmount'].median(), inplace=True)

    # drop ID column
    data = data.drop('Loan_ID',axis=1)
    
    #split features and target 
    X = data.drop('Loan_Status',axis=1)
    y = data.Loan_Status.values

    #scale the  features 
    X  = pd.get_dummies(X,columns=["Gender","Married","Education","Self_Employed","Property_Area"])
    X = StandardScaler().fit_transform(X)
    

    return X,y 
```

대출 데이터 세트를 사전 처리하겠습니다.
 처리 된 기능과 대상을 반환합니다.
 

```python
X,y = preprocessing(data) 
```

그런 다음 Scikit-learn의 `train_test_split`함수를 사용하여 처리 된 데이터를 학습 및 테스트 데이터 세트로 분할합니다.
 

```python
# split into train and test set 
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.1)
```

이제 LogisticRegression 모델을 만들고 훈련 세트로 훈련합니다.
 

```python
# create and train the classifier 

classifier = LogisticRegression()

classifier.fit(X_train,y_train)
```

## 훈련 된 모델을 Python 코드로 변환하는 방법
 

m2cgen 라이브러리는 훈련 된 모델을 위에서 언급 한 지원 언어로 변환하는 방법을 제공합니다.
 이 예제에서는 m2cgen의`export_to_python ()`메서드를 사용하여 훈련 된 모델을 Python으로 변환합니다.
 

```python
# convert model to pure python code  
model_to_python = m2c.export_to_python(classifier)  
```

다음은 Python 코드로 표현 된 학습 된 모델입니다.
 

```python
#pure python code 

def score(input):
    
    return (((((((((((((((((0.7929123964945446) + ((input[0]) * (0.07801862594632314))) + ((input[1]) * (-0.014853900985478468))) + ((input[2]) * (-0.15783041201914427))) + ((input[3]) * (-0.05222073553791883))) + ((input[4]) * (-0.0787403404504791))) + ((input[5]) * (1.3714807410150505))) + ((input[6]) * (0.015077765348160292))) + ((input[7]) * (-0.015077765348160353))) + ((input[8]) * (-0.12161041350915254))) + ((input[9]) * (0.12161041350915253))) + ((input[10]) * (0.09387440269562626))) + ((input[11]) * (-0.09387440269562626))) + ((input[12]) * (-0.0047109053878701835))) + ((input[13]) * (0.004710905387870008))) + ((input[14]) * (-0.14569247529698154))) + ((input[15]) * (0.19858601990225683))) + ((input[16]) * (-0.06417592734444703))
```

생성하는 Python 함수 코드는 입력 데이터를 수신 한 다음 예측을 수행합니다.
 이제 생성 된 Python 코드를 테스트 해 보겠습니다.
 

먼저 실제 학습 된 모델에서 예측을 수행합니다.
 다음은 테스트 세트에서 사용할 샘플 테스트 데이터입니다.
 

```python
test_data = X_test[6]
print(test_data)
```

어레이 ([1.24474546, 1.9817189, -0.55448733, 3.02536229, 0.2732313, 0.41173269, -0.47234264, 0.47234264, -0.72881553, 0.72881553, 0.52836225, -0.52836225, -2.54711697, 2.54711697, 1.55889948, -0.7820157, -0.70020801])
 

이제 실제 훈련 된 모델로 예측합니다.
 

```python
pred = classifier.predict(test_data.reshape(1,-1))  
print("prediction result: {}".format(pred))
```

예측 결과 : [1]
 

모델 예측은 1이며 이는 고객이 대출 금액을받을 자격이 있음을 의미합니다.
 

동일한 테스트 데이터를 사용하여 생성 된 순수 Python 코드에서 예측을 수행하고 동일한 예측을 제공하는지 평가합니다.
 

```python
# test prediction in pure python code 
input = [ 1.24474546,  1.9817189 , -0.55448733,  3.02536229,  0.2732313 ,
        0.41173269, -0.47234264,  0.47234264, -0.72881553,  0.72881553,
        0.52836225, -0.52836225, -2.54711697,  2.54711697,  1.55889948,
       -0.7820157 , -0.70020801]

pred = score(input) 
print("prediction result: {}".format(int(pred)))
```

예측 결과 : 1
 

순수한 Python 코드도 동일한 예측 결과를 제공합니다.
 

## 훈련 된 모델을 PHP 코드로 변환하는 방법
 

m2cgen의`export_to_php ()`메서드를 사용하여 훈련 된 모델을 순수한 PHP 코드로 변환합니다.
 

```python
# convert model to pure PHP code  
model_to_php = m2c.export_to_php(classifier)  
```

다음은 PHP 코드로 표현 된 훈련 된 모델입니다.
 

```php
function score(array $input)
{
    return (((((((((((((((((0.8166973302490392) + (($input[0]) * (0.035269518507829584))) + (($input[1]) * (0.05203333118549156))) + (($input[2]) * (-0.13217178253938103))) + (($input[3]) * (-0.13136526173536608))) + (($input[4]) * (-0.024875019809902837))) + (($input[5]) * (1.2864103414352563))) + (($input[6]) * (-0.005259373701309709))) + (($input[7]) * (0.005259373701309715))) + (($input[8]) * (-0.11512289603368371))) + (($input[9]) * (0.11512289603368378))) + (($input[10]) * (0.06905305123713898))) + (($input[11]) * (-0.06905305123713898))) + (($input[12]) * (0.021080906307735767))) + (($input[13]) * (-0.02108090630773594))) + (($input[14]) * (-0.14491490189610398))) + (($input[15]) * (0.2189862115713242))) + (($input[16]) * (-0.08599736364921017));
}
```

동일한 테스트 데이터를 사용하여 생성 된 순수 PHP 코드에서 예측을 수행하고 동일한 예측을 제공하는지 평가합니다.
 

```php
# test prediction in pure PHP code
$input = [1.24474546, 1.9817189, -0.55448733, 3.02536229, 0.2732313,
    0.41173269, -0.47234264, 0.47234264, -0.72881553, 0.72881553,
    0.52836225, -0.52836225, -2.54711697, 2.54711697, 1.55889948,
    -0.7820157, -0.70020801];

// perform predition with pure php code
$pred = score($input);


echo "Predicton result: ". round($pred);
```

예측 결과 : 1
 

순수한 PHP 코드도 동일한 예측 결과를 제공합니다.
 

## 훈련 된 모델을 JavaScript 코드로 변환하는 방법
 

마지막 예제에서는 m2cgen의`export_to_javascript ()`메서드를 사용하여 훈련 된 모델을 순수한 JavaScript 코드로 변환합니다.
 

```python
# convert model to pure Javascript code  
model_to_javascript = m2c.export_to_javascript(classifier)  
```

다음은 JavaScript 코드로 표현 된 학습 된 모델입니다.
 

```js
function score(input)
{
    return (((((((((((((((((0.8166973302490392) + ((input[0]) * (0.035269518507829584))) + ((input[1]) * (0.05203333118549156))) + ((input[2]) * (-0.13217178253938103))) + ((input[3]) * (-0.13136526173536608))) + ((input[4]) * (-0.024875019809902837))) + ((input[5]) * (1.2864103414352563))) + ((input[6]) * (-0.005259373701309709))) + ((input[7]) * (0.005259373701309715))) + ((input[8]) * (-0.11512289603368371))) + ((input[9]) * (0.11512289603368378))) + ((input[10]) * (0.06905305123713898))) + ((input[11]) * (-0.06905305123713898))) + ((input[12]) * (0.021080906307735767))) + ((input[13]) * (-0.02108090630773594))) + ((input[14]) * (-0.14491490189610398))) + ((input[15]) * (0.2189862115713242))) + ((input[16]) * (-0.08599736364921017));
}
```

동일한 테스트 데이터를 사용하여 생성 된 순수 JavaScript 코드에서 예측을 수행하고 동일한 예측을 제공하는지 평가합니다.
 

```js
// perform predition with pure Javascript code
let input =  [1.24474546, 1.9817189, -0.55448733, 3.02536229, 0.2732313,
    0.41173269, -0.47234264, 0.47234264, -0.72881553, 0.72881553,
    0.52836225, -0.52836225, -2.54711697, 2.54711697, 1.55889948,
    -0.7820157, -0.70020801];

let pred = score(input);

console.log("Prediction results:",Math.round(pred));
```

"예측 결과 :", 1
 

순수한 JavaScript 코드도 동일한 예측 결과를 제공합니다.
 

## 마무리
 verified_user

경우에 따라 m2cgen 라이브러리에서 생성 된 네이티브 코드는 원래 Python 학습 ML 모델과 다른 결과를 제공 할 수 있습니다.
 다음은 라이브러리 개발자의 간단한 설명입니다.
 

> "일부 모델은 기본 Python 라이브러리의 예측 단계에서 입력 데이터를 특정 유형으로 지정합니다. 현재 m2cgen은`float64` (`double`) 데이터 유형에서만 작동합니다. 입력 데이터를 수동으로 다른 유형으로 변환하고 확인할 수 있습니다.
 또한 대상 언어에서 부동 소수점 산술의 특정 구현으로 인해 약간의 차이가 발생할 수 있습니다. "
 (출처 : Github Repository)
 

위에서 언급 한 예에서, 저는 파이썬에`int ()`, PHP에`round ()`, 자바 스크립트에`Math.round ()`를 사용하여 예측 결과를 float 데이터 유형에서 정수 데이터 유형으로 변환합니다.
 

축하합니다.이 기사의 끝까지 완료했습니다!
 

이 기사에 사용 된 데이터 세트, 노트북 및 스크립트 파일은 https://github.com/Davisy/Convert-Trained-ML-Models-To-Native-Code에서 다운로드 할 수 있습니다.
 

새로운 것을 배웠거나이 기사를 읽고 즐거웠다면 다른 사람들이 볼 수 있도록 공유하십시오.
 그때까지 다음 포스트에서 만나요!
 Twitter @Davis_McDavid에서도 연락 할 수 있습니다.
 