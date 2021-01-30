---
layout: post
title: "Python 및 Pandas를 사용하여 주요 폭풍, 비관론 및 하드 데이터를 매핑하는 방법
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/thunderstorm-98541_640.png
tags: PYTHON
---


때로는 모든 것이 옛날보다 훨씬 더 나빠 졌는지 생각해 보는 것이 다소 위안이 될 수 있습니다.
 

"아이들은 존경심이 없습니다."
 

"모든 비용이 너무 많이 든다."
 

"공무원은 신뢰를 불러 일으키지 않습니다."
 

"그리고 날씨는 어떻습니까? 우리는 그렇게 많은 파괴적인 허리케인을 겪은 적이 없었습니다. 그렇죠?"
 

글쎄, 나는 몇 번 블록 주위에있을만큼 충분히 늙었 고 확실하지 않습니다.
 나는 어렸을 때 천사 같지 않았고, 우리가 원했던 것보다 항상 비용이 많이 들었고, 공무원은 지구상에서 가장 사랑받는 생물이 아니 었습니다.
 하지만 큰 폭풍?
 나는 단서가 없습니다.
 

거기에 훌륭한 폭풍 데이터가 많이 있다는 것이 밝혀 졌기 때문에 우리가 적어도 단서를 찾아서는 안 될 이유가 없습니다.
 기존의 전문 도구에 데이터 분석을 추가하려는 시도가 도움이 될 수 있습니다.
 

하지만 먼저 용어를 신중하게 정의하고 배경 세부 정보를 입력해야합니다.
 

## 주요 폭풍이란 무엇입니까?
 

허리케인 (보다 정확하게는 열대성 저기압)은 열대 지역 내의 바다 위에 형성된다는 의미에서 "열대성"입니다.
 "열대"라는 용어는 적도에서 북쪽과 남쪽으로 23도 (또는 그 정도) 이내에있는 지표면의 영역을 나타냅니다.
 

폭풍은 바람의 움직임이 주기적 (남반구에서는 시계 방향, 북반구에서는 시계 반대 방향)이기 때문에 "사이클론"이라고합니다.
 

사이클론은 증발 된 바닷물에 의해 공급되며 특히 거주지 위를 표류 한 후 폭우와 종종 격렬한 뇌우를 남깁니다.
 

넓은 의미에서 약 34 ~ 63 노트 (또는 시속 39 ~ 72 마일)의 지속적인 바람을 생성하는 폭풍은 열대성 폭풍으로 간주됩니다.
 64 노트 (73mph) 이상의 바람을 동반 한 폭풍은 허리케인 (또는 서태평양 또는 북 인도양에서는 태풍)입니다.
 

허리케인은 1에서 5 사이의 카테고리로 측정되며, 카테고리 5 허리케인은 가장 폭력적이고 위험합니다.
 

## 주요 폭풍 데이터의 출처는 어디입니까?
 

적어도 미국에는 지난 세기 반 동안 신뢰할 수 있고 대체로 일관된 과거 폭풍 데이터가 존재합니다.
 그러나 해당 데이터의 맥락을 올바르게 이해하려면 이러한 관찰이 수년 동안 어떻게 이루어 졌는지에 대한 지식이 필요합니다.
 

1940 년대까지 대부분의 관측은 원 양선 승무원에 의해 이루어졌습니다.
 그러나 선원들은 그들이 본 것을 관찰하고보고 만 할 수 있으며, 그들이 보는 것은 어디로 가는지에 따라 결정될 것입니다.
 

1914 년 파나마 운하가 개통되기 전에 유럽과 태평양 사이를 여행하는 선박은 미국 해안 지역을 대부분 놓친 남아메리카의 남단을 따라 항로를 따라갑니다.
 그 결과 기상 현상의 상당 부분이 단순히 놓쳤을 가능성이 있습니다.
 

유사하게, 1940 년대에 항공기 정찰의 출현으로 과학자들은 이전에 놓쳤을 더 많은 사건을 포착 할 수 있었을 것입니다.
 그리고 1960 년대부터 기상 위성을 사용함으로써 우리는 거의 모든 해양 활동을 포착 할 수있었습니다.
 

이러한 변화와 폭풍 데이터에 미치는 영향은 지구 물리학 유체 역학 연구소 (GFDL)에서 수행 한 데이터 분석 연구를 기반으로 미국 정부의 NOAA (National Oceanic and Atmospheric Administration) 사이트에서이 페이지에 깔끔하게 요약되어 있습니다.
 

## 역사적 기록은 무엇을 보여줍니까?
 

그래서 그 모든 배경 후에 데이터는 실제로 무엇을 말합니까?
 심각한 허리케인이 과거보다 더 흔합니까?
 NOAA 웹 사이트에 따르면 대답은 "아니오"입니다.
 그들이 그것을 넣는 방법은 다음과 같습니다.
 

> "2 일 이상 지속되는 대서양 열대성 폭풍은 증가하지 않았습니다. 2 일 미만 지속되는 폭풍은 급격히 증가했지만 이는 더 나은 관측 때문일 것입니다 ... 우리는 증가를 초래할 기후 변화 신호를 인식하지 못합니다.
 이러한 증가는 관찰 관행 개선에서 기대할 수있는 것과 질적으로 일치합니다. "
 

연구 자체를 읽음으로써 그들이 만든 데이터 조작 선택에 대한 멋진 설명을 포함하여 전체 이야기를 얻을 수 있습니다.
 사실,이 연구는 전문가들이 데이터 문제에 어떻게 접근하는지 보여주는 좋은 예이기 때문에 여러분이 그 연구를 읽어 보시기를 권합니다.
 

그러나 여기서부터는 조정되지 않은 원시 데이터 레코드를 시각화하려는 내 아마추어와 단순화 된 시도에 갇히게 될 것입니다.
 

## 미국 허리케인 데이터 : 1851-2019
 

"Continental United States Hurricane Impacts / Landfalls"데이터의 출처는이 NOAA 웹 페이지입니다.
 

데이터를 다운로드하려면 왼쪽 상단 ( "연도"제목 필드)에서 마우스를 클릭하고 오른쪽 하단까지 드래그하여 데이터를 복사했습니다.
 그런 다음 로컬 컴퓨터의 일반 텍스트 편집기에 붙여넣고 `.csv`확장자를 가진 파일에 저장했습니다.
 

### 허리케인 데이터를 정리하는 방법
 

웹 페이지 자체를 빠르게 살펴보면 정리가 필요한 일부 서식이 표시됩니다.
 각 10 년은 `1850s`와 같은 문자열 만 포함하는 단일 행으로 도입됩니다.
 해당 행을 삭제하고 싶을 것입니다.
 이벤트가없는 연도는 두 번째 열에 `none`문자열을 포함합니다.
 그들도 가야 할 것입니다.
 

최대 풍속에 대한 데이터가없는 것으로 보이는 일부 이벤트가 있습니다.
 숫자 (매듭 단위로 측정) 대신 해당 이벤트의 속도 값은 5 개의 대시 (`-----`)로 표시됩니다.
 우리는 그것을 우리가 작업 할 수있는 것으로 변환해야합니다.
 

마지막으로 월은 일반적으로 3 글자 약어로 표시되지만 두 달에 걸쳐 진행되는 몇 가지 이벤트가 있습니다.
 그래서 우리는 그것들을 적절하게 처리 할 수있을 것입니다. 그러므로 저는`Sp-Oc`와`Jl-Au`를 각각`Sep`과`Jul`로 변환 할 것입니다.
 

사실 우리는 실제로 월 열을 사용하지 않을 것이므로 실제로 차이를 만들지 않습니다.
 하지만 알아두면 좋은 도구입니다.
 

Jupyter에서 설정하는 방법은 다음과 같습니다.
 

```undefined
import pandas as pd
import matplotlib as plt
import matplotlib.pyplot as plt 
import numpy as np

df = pd.read_csv('all-us-hurricanes-noaa.csv')

```

각 열의 데이터 유형을 살펴 보겠습니다.
 상태 및 이름 열의 문자열을 무시할 수 있습니다. 어쨌든 관심이 없습니다.
 그러나 우리는 날짜와 Max Wind 열을 가지고 무언가를해야 할 것입니다. 그들은 우리에게 `객체`로서 좋은 것을하지 않을 것입니다.
 

```undefined
df.dtypes

Year                                         object
Month                                        object
States Affected and Category by States       object
Highest\nSaffir-\nSimpson\nU.S. Category    float64
Central Pressure\n(mb)                      float64
Max Wind\n(kt)                               object
Name                                         object
dtype: object

```

따라서 문자`s`에 대해`Year` 열의 모든 행을 필터링하고 간단히 삭제합니다 (`== False`).
 모든 10 년 헤더 (즉,`1850s`와 같은 일부로`s`를 포함하는 행)를 처리합니다.
 

마찬가지로 폭풍이 발생하지 않는 연도를 제거하기 위해`Month` 열에`None` 문자열이 포함 된 행을 삭제합니다.
 

조용한 해는 시각화에 약간의 영향을 미칠 수 있지만 어떤 종류의 null 값을 포함하는 것은 아마도 다른 방식으로 더 왜곡 될 것입니다.
 또한 시각화를 크게 복잡하게 만듭니다.
 

마지막으로이 두 개의 여러 달 행을 바꿉니다.
 

```undefined
df = df[(df.Year.str.contains("s")) == False]
df = df[(df.Month.str.contains("None")) == False]
df = df.replace('Sp-Oc','Sep')
df = df.replace('Jl-Au','Jul')

```

다음으로 편리한 Pandas`to-datetime` 메서드를 사용하여 3 글자 월 약어를 1과 12 사이의 숫자로 변환합니다. 형식 코드`% b`는 Python의 법적 날짜 관련 지정 중 하나이며 Python에 다음과 같이 알려줍니다.
 우리는 세 글자로 된 약어로 작업하고 있습니다.
 전체 목록은이 페이지를 참조하십시오.
 

```undefined
df.Month = pd.to_datetime(df.Month, format='%b').dt.month

```

헤더를 좀 더 조여서 코드에서 읽고 참조하기 더 쉽도록하고 싶습니다.
 `df.columns`는 모든 열 헤더 값을 여기에서 지정한 목록으로 변경합니다.
 

```undefined
df.columns =['Year', 'Month', 'States', 'Category', 
             'Pressure', 'Max Wind', 'Name']  

```

Year 데이터를 문자열 개체에서 정수로 변환해야합니다. 그렇지 않으면 Python이 적절하게 작업하는 방법을 알지 못합니다.
 그것은`astype`을 사용하여 수행됩니다.
 

광고 된대로`Max Wind`의 널 (`-----`) 값을`NaN`으로 변환합니다. NumPy는 "숫자가 아님"으로 읽습니다.
 그런 다음`Max Wind`의 데이터를`object`에서`float`로 변환합니다.
 

```undefined
df = df.astype({'Year': 'int'})
df = df.replace('-----',np.NaN)
df = df.astype({'Max Wind': 'float'})

```

이제 모든 것이 어떻게 보이는지 보겠습니다.
 

```undefined
df.dtypes

Year          int64
Month         int64
States       object
Category    float64
Pressure    float64
Max Wind    float64
Name         object
dtype: object

```

훨씬 낫다.
 verified_user

### 허리케인 데이터를 제시하는 방법
 

이제 데이터를 살펴보면 허리케인 범주, 기압 및 최대 풍속의 세 가지 메트릭을 분류 할 것을 제안합니다.
 

제 생각에는 합병증을 합쳐서 얻는 것이 거의 없으며 더 가벼운 폭풍과 더 심각한 폭풍의 사건 사이의 중요한 차이점을 놓칠 위험이 있습니다.
 

물론 개별 측정 항목을 분리하여 분포가 어떻게 보이는지 확인할 수 있습니다.
 예를 들어`범주`열에`value_counts`를 사용하면 더 가벼운 범주 1과 2의 허리케인이 더 위험한 사건보다 훨씬 더 자주 발생한다는 것을 알 수 있습니다.
 

```undefined
df['Category'].value_counts()

1.0    121
2.0     83
3.0     62
4.0     25
5.0      4
Name: Category, dtype: int64

```

그리고 전체 데이터 세트의 단일 히스토그램을 플로팅하면 히스토리를 통해 이벤트 수 (y 축에 표시됨)에 대한 멋진 개요를 얻을 수 있지만 프로세스에서 일부 세부 정보를 잃을 수 있습니다.
 

이 히스토그램에서 시간이 지남에 따라 폭풍 빈도에 눈에 띄는 변화가 없음이 분명합니다.
 우리가 사용하는 빈 수를 선택하는 것이 의도하지 않게 중요한 추세를 가리지 않도록하려면 25 개가 아닌 다른 값으로 실험 해보십시오.
 

```undefined
df.hist(column='Year', bins=25)

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/h_image_1.png)

하지만 각 측정 항목에 집중할 수 있도록 세 개의 개별 그래프를 그릴 것입니다.
 이를 위해 세 개의 새 데이터 프레임을 만들고 각각 `연도`열과 해당 데이터 열의 내용을 채 웁니다.
 

```undefined
df_category = df[['Year','Category']]
df_wind = df[['Year','Max Wind']]
df_pressure = df[['Year','Pressure']]

```

각 데이터 프레임을 플롯으로 바로 보내면 폭풍의 심각도를 구분하지 못하기 때문에 요점이 누락됩니다.
 따라서 범주 (1-5)별로 데이터를 나누는 방법을 보여 드리겠습니다.
 이 `for`루프는 숫자 1 ~ 6 (1 ~ 5 사이의 숫자를 반환하는 "Python")을 반복하고 각 숫자를 차례로 사용하여 해당 범주의 허리케인을 검색합니다.
 

카테고리가 숫자와 일치하는 행은 `df1`이라는 새 (임시) 데이터 프레임에 기록되며, 이는 차례로 히스토그램을 그리는 데 사용됩니다.
 `plt.title` 행은 범주 번호 (`converted_num`의 현재 값)를 포함하는 인쇄 된 그래프의 제목을 적용합니다.
 

루프는 현재 범주의 이벤트 수를`df1`에 쓸 때마다 프로세스를 5 번 수행합니다.
 5 개의 히스토그램이 차례로 인쇄됩니다.
 

```undefined
for x in range(1, 6):
    cat_num = x
    converted_num = str(cat_num) 
    dfcat = df_category['Category']==(x)
    df1 = df_category[dfcat]
    df1.hist(column='Year', bins=20)
    plt.title("Total Category " + (converted_num) + " Events")

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/h_image_cat1.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/h_image_cat2.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/h_image_cat3.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/h_image_cat4.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/h_image_cat5.png)

보시다시피, 시간이 지남에 따라 폭풍 빈도가 크게 증가한다는 눈에 띄는 증거는 없습니다.
 

항상 그렇듯이 데이터를 스캔 (`value_counts ()`와 같은 도구 사용)하여 플롯이 실제 세계에서 의미가 있는지 확인합니다.
 

## 미국 열대성 폭풍 데이터 : 1851-1965, 1983-2019
 

물론 허리케인 (또는 사이클론)은 이야기의 일부일뿐입니다.
 파괴적인 열대성 폭풍의 빈도가 증가하는 것도 우려의 원인이 될 것입니다.
 

다행히 NOAA는 허리케인 데이터와 거의 동일한 형식으로 관련 데이터를 제공합니다.
 다음은 차트를 찾을 수있는 웹 페이지입니다.
 이전과 동일한 방식으로 데이터를`.csv` 파일에 복사합니다.
 

그러나 1966-1982 년 동안 데이터가 없다는 점에 유의하십시오.
 이유를 묻지 마십시오.
 없습니다.
 재미있는 것, 날씨.
 

허리케인 버전에서 필요한 것이 없으므로 프로젝트의이 부분에 대해 새 Jupyter 노트북을 만들겠습니다.
 따라서 항상 다음과 같이 설정합니다.
 

```undefined
import pandas as pd
import matplotlib as plt
import numpy as np
df = pd.read_csv('all-us-tropical-storms-noaa.csv')

```

### 열대성 폭풍 데이터를 정리합시다
 

이벤트가없는 연도를 나타내는 행은 다시 제거해야합니다.
 

```undefined
df = df[(df.Date.str.contains("None")) == False]

```

이 데이터 세트의 `날짜`열에는 $, *, #, %, & 등 5 개의 각주를 가리키는 문자가 있습니다.
 각주에는 중요한 정보가 포함되어 있지만 해당 문자를 제거하지 않으면 슬픔을 느낄 것입니다.
 

다음 명령어는 `Date`열에있는 모든 문자열을 아무것도없는 것으로 대체하여이를 수행합니다.
 

```undefined
df['Date'] = df.Date.str.replace('\$', '')
df['Date'] = df.Date.str.replace('\*', '')
df['Date'] = df.Date.str.replace('\#', '')
df['Date'] = df.Date.str.replace('\%', '')
df['Date'] = df.Date.str.replace('\&', '')

```

다음으로 열 헤더를 재설정하겠습니다.
 첫째, 멋진 짧은 이름으로 작업하는 것이 더 쉬울 것이기 때문입니다.
 그러나 주로 Linux 시스템 관리자로서 파일 이름이나 제목에 도덕적으로 불쾌감을주는 공백을 발견하기 때문입니다.
 

```undefined
df.columns =['Storm#', 'Date', 'Time', 'Lat', 'Lon', 
             'MaxWinds', 'LandfallState', 'StormName'] 

```

열 데이터 유형에는 몇 가지 작업이 필요합니다.
 

```undefined
df.dtypes

Storm#            object
Date              object
Time              object
Lat               object
Lon               object
MaxWinds         float64
LandfallState     object
StormName         object
dtype: object

```

데이터가 어떻게 생겼는지 봅시다 :
 

```undefined
df.head()

Storm# Date  Time Lat Lon MaxWindsLandfallState StormName
6 10/19/1851 1500Z 41.1N 71.7W 50.0 NY NaN
3 8/19/1856 1100Z 34.8 76.4 50.0 NC NaN
4 9/30/1857 1000Z 25.8 97 50.0 TX NaN
3 9/14/1858 1500Z 27.6 82.7 60.0 FL NaN
3 9/16/1858 0300Z 35.2 75.2 50.0 NC NaN

```

나는 실제로 그`Storm #`값이 무엇인지 확실하지 않지만 누구에게도 해를 끼치지는 않습니다.
 날짜는 허리케인 데이터보다 훨씬 더 형식이 지정됩니다.
 하지만 새로운 형식으로 변환해야합니다.
 제대로하고`datetime`으로 가자.
 

```undefined
df.Date = pd.to_datetime(df.Date)

```

### 열대성 폭풍 데이터를 제시하는 방법
 

우리의 목적을 위해 정말로 중요한 유일한 데이터 열은 폭풍의 강도를 정의하는 MaxWinds입니다.
 이 명령은`Date` 및`MaxWinds` 열로 구성된 새 데이터 프레임을 만듭니다.
 

```undefined
df1 = df[['Date','MaxWinds']]

```

이것을 밀어 낼 이유가 없습니다. 우리는 바로 히스토그램을 실행하는 것이 좋습니다.
 1970 년경에 데이터가없는 격차를 즉시 확인할 수 있습니다.
 다시 말하지만, 상승 추세가 많지 않은 것 같습니다.
 

```undefined
df1['Date'].hist()

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/ts_image_1.png)

하지만 여기서 좀 더 자세히 살펴 봐야합니다.
 결국,이 데이터는 30 노트와 75 노트 폭풍을 혼합합니다.
 우리는 그들이 비슷한 속도로 일어나고 있는지 확실히 알고 싶을 것입니다.
 

우리가 가지고있는 데이터 행 수를 알아 봅시다.
 `shape`는 모두 362 개의 이벤트가 있음을 알려줍니다.
 

```undefined
print(df1.shape)

(362, 2)

```

데이터 프레임을 인쇄하면`MaxWinds` 값이 모두 5의 배수임을 알 수 있습니다. 데이터를 직접 스캔하면 30에서 70 사이의 범위를 알 수 있습니다.
 

```undefined
df1

 Date  MaxWinds
1 1851-10-19 50.0
6 1856-08-19 50.0
7 1857-09-30 50.0
8 1858-09-14 60.0
9 1858-09-16 50.0
... ... ...
391 2017-09-27 45.0
392 2018-05-28 40.0
393 2018-09-03 45.0
394 2018-09-03 45.0
395 2019-09-17 40.0
362 rows × 2 columns

```

따라서 다양한 수준의 폭풍에 대한 합리적인 프록시로 데이터를 4 개의 작은 집합으로 나눕니다.
 저는 4 개의 데이터 프레임을 만들고 더 좁은 범위 (즉, 30 ~ 39 노트, 40 ~ 49, 50 ~ 59, 60 ~ 79)에 속하는 이벤트로 채웠습니다.
 이는 이벤트에 대한 합리적인 기준을 제공해야합니다.
 

```undefined
df_30 = df1[df1['MaxWinds'].between(30, 39)]
df_40 = df1[df1['MaxWinds'].between(40, 49)]
df_50 = df1[df1['MaxWinds'].between(50, 59)]
df_60 = df1[df1['MaxWinds'].between(60, 79)]

```

우리가 선택한 컷오프 지점이 의미가 있는지 확인합시다.
 이 코드는 네 가지 데이터 양식 각각의 인덱스에있는 행 수를 매력적으로 인쇄합니다.
 

```undefined
st1 = len(df_30.index)
print('The number of storms between 30 and 39: ', st1)
st2 = len(df_40.index)
print('The number of storms between 40 and 49: ', st2)
st3 = len(df_50.index)
print('The number of storms between 50 and 59: ', st3)
st4 = len(df_60.index)
print('The number of storms between 60 and 79: ', st4)

The number of storms between 30 and 39:  51
The number of storms between 40 and 49:  113
The number of storms between 50 and 59:  142
The number of storms between 60 and 79:  56

```

이 네 가지 명령을 하나로 결합하는 우아한 방법이있을 것입니다.
 하지만 제 철학은 알아내는 데 한 시간이 걸리는 구문이 5 초의 잘라 내기 및 붙여 넣기의 단순함을 결코 능가하지 않는다는 것입니다.
 이제까지.
 

또한 오랜 친구 인`value_counts ()`를 사용하여 데이터를 조금 더 자세히 살펴볼 수도 있습니다.
 이것은 우리의 시간 범위에 걸쳐 71 개의 40 노트 이벤트와 42 개의 45 노트 이벤트가 있었다는 것을 보여줄 것입니다.
 

```undefined
df_40['MaxWinds'].value_counts()

40.0    71
45.0    42
Name: MaxWinds, dtype: int64

```

4 개의 하위 집합을 모두 함께 표시하기 위해 단일 선 그래프를 그릴 수 있습니다.
 이 플롯은 축 및 플롯 레이블과 범례를 추가하여 데이터를 더 쉽게 이해할 수 있도록합니다.
 `subplot (111)`값은 Figure의 크기를 제어합니다.
 

```undefined
import matplotlib.pyplot as plt
fig = plt.figure()
ax = plt.subplot(111)
df_30['MaxWinds'].plot(ax=ax, label='df_30')
df_40['MaxWinds'].plot(ax=ax, label='df_40')
df_50['MaxWinds'].plot(ax=ax, label='df_50')
df_60['MaxWinds'].plot(ax=ax, label='df_60')
ax.set_ylabel('Wind Speed In Knots')
ax.set_xlabel('Time Between 1851 and 2019')
plt.title('Tropical Storms by Maximum Wind Speeds (knots)')
ax.legend()

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/ts_image_2.png)

이것은 우리가 데이터 자체를 엉망으로 만들고 있지 않음을 확인하는 데 도움이 될 수 있습니다.
 예를 들어 시각적으로 확인하면 실제로 데이터 세트에 30 노트 이벤트가 하나만 있었고 2016 년 기간이 끝날 무렵에 발생했음을 알 수 있습니다.하지만 이벤트의 변화를 보여주는 좋은 방법은 아닙니다.
 회수.
 

이를 위해 각 데이터 프레임에 보관 된 데이터를 살펴 보겠습니다.
 

```undefined
df_30['Date'].hist(bins=20)

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/ts_image_3.png)

```undefined
df_40['Date'].hist(bins=20)

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/ts_image_4.png)

```undefined
df_50['Date'].hist(bins=20)

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/ts_image_5.png)

```undefined
df_60['Date'].hist(bins=20)

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/ts_image_6.png)

이 네 가지 플롯을 빠르게 살펴보면 150 년 정도의 데이터를 통해 상당히 일관된 이벤트 빈도를 알 수 있습니다.
 다시 말하지만, 중요한 트렌드를 놓치고 있지 않은지 확인하기 위해 다른 수의 빈을 사용하여 직접 시도해보십시오.
 

David Clinton의 웹 사이트를 통해 훨씬 더 많은 기술 콘텐츠를 찾을 수 있습니다.
 특히 그의 새 책, Keep Up : Backgrounders to all the big technology trends you ca n`t over ignore를 즐길 수 있습니다.
 