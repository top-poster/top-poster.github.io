---
layout: post
title: "Chart.js를 사용하여 미적으로 만족스러운 시각화를 만드는 방법"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Laptop displaying charts](https://miro.medium.com/max/3840/1*t6wp6xn1GJfKE8uIuNbm7Q.png)

프런트엔드 개발자들은 프런트엔드 차트에서 작업을 하기가 어렵다는 것을 알고 있으며, 이는 그들이 일반적으로 캔버스나 사용자 정의 svg 요소를 사용하지 않기 때문이다. 모든 차트 라이브러리가 이 두 가지 방법을 사용하기 때문에, 이것이 왜 쉽지 않은지 이해할 수 있습니다.

Chart.js는 무료 오픈 소스 라이브러리로, 하이차트와 같은 라이브러리를 위해 비싼 라이센스를 구입해야 하는 필요성을 대체할 수 있다. 이러한 유료 라이브러리는 종종 매우 강력하며 Chart.js에서는 많은 기능을 찾을 수 없지만, 특히 간단한 프로젝트를 위해 여전히 많은 기능을 제공합니다.

Chart.js 차트의 기본 모양은 기껏해야 평범하다고 생각하는데, 라이브러리를 사용하여 아름다운 차트를 만드는 방법을 설명하겠습니다.

기본 라인 차트는 이렇지만 실제 프로덕션 환경에서는 이러한 차트를 사용하지 않습니다. 대시보드의 전체 모양이 손상되기 때문입니다.

![Chart](https://miro.medium.com/max/2836/1*l-wUmJX2fwhXhepVfKF7tQ.png)

단순함을 위해 제가 좋아하는 차트를 선택하고 차트에 대해 정확히 동일한 모양을 만드는 방법을 설명하겠습니다. Bare metrics의 차트가 마음에 들어서 다른 색깔로 따라 할 거예요.

![Line chart](https://miro.medium.com/max/1986/1*s55DqhTtEZ-d8duqqcUrsg.png)

이제 Chart.js의 기본 선 차트부터 살펴보겠습니다.

참고: 코드를 따라 연습할 수 있습니다. 전체 코드는 기사의 끝부분에도 나와 있습니다.

이 차트는 맨뼈 설정만으로 구성된 간단한 선 차트이며 다음과 같습니다.

![Simple line chart](https://miro.medium.com/max/1446/1*Kr0t35rCYJAZ6MIM3hkJqg.png)

# 라인 자체 스타일링

선을 어떻게 스타일링해서 우리의 예와 더 비슷하게 만들 수 있는지 봅시다. 차트를 노골적으로 복사하지 않도록 나머지 예에서는 파란색(#3ABEFF)을 사용합니다.

여기서 우리가 하고 있는 일의 기본을 설명합시다. 원래 라인이 너무 얇아서 색상이 맞지 않았어요. 또한 차트의 모든 점에 대한 점을 표시하고 선과 X축 사이에 채우기도 합니다. 마지막으로, 우리의 원래 차트는 너무 곡선인 반면, 우리가 사용하고 있는 예는 곡선이 아닙니다.

이 차트는 다음과 같습니다.

![New line chart](https://miro.medium.com/max/1464/1*p_uieRaAv3RUapIkznCJsw.png)

# 그리드 개선

아직 개선의 여지가 있으니 차트가 더 깨끗해 보이도록 격자선을 숨기자.

옵션 객체를 만들어 차트 작성자에게 전달해야 합니다. 사용 기본 사항을 보려면 Chart.js 설명서의 시작하기 섹션을 참조하십시오.

디스플레이와 드로보더(draw border)를 거짓으로 만들면 X축과 Y축의 테두리를 포함한 모든 격자선이 숨겨진다. 알다시피 x축과 y축은 배열을 받아들인다. 이는 데이터 세트가 여러 개 있는 경우에 대한 것입니다. 여기 하나밖에 없어서 더 이상 할 게 없어요.

# 눈금 사용자 정의

눈금에 대해 잘 모를 경우 차트 하단의 작은 레이블과 차트 왼쪽에 있는 값이 표시됩니다. 우리는 그들이 원하는 대로 보이도록 그들을 맞춤 제작하기를 원합니다.

여기서는 y축만 보여주는데, x축에 폰트컬러와 폰트사이즈를 전달하면 색상과 폰트사이즈를 바꿀 수 있다.

콜백(callback) 방법을 사용하면 원하는 방식으로 문자열을 사용자 정의할 수 있습니다. 이 경우, 우리는 통화 부호를 앞에 붙이지만 많은 수의 쉼표를 포함하기 위해 정규식 규칙도 사용한다(예: 1000은 1,000으로 표시됨).

격자무늬와 눈금을 개선한 후에 어떤 것이 있는지 봅시다. 우리가 거기에 도착할 것 같지 않니?

![Cleaner line chart](https://miro.medium.com/max/1424/1*0O81vss-MIXSUsFvm-xzJg.png)

# 사용자 지정 HTML 도구 설명 만들기

이제 가장 어렵지만 가장 중요한 것이 필요한 때입니다. Chart.js로 제공되는 일반 도구를 대체할 수 있는 아름다운 맞춤형 HTML 툴팁을 만들어야 합니다.

사용자 지정 HTML 툴팁을 생성하면 이를 완전히 사용자 정의하고 미적으로 만족스러운 결과를 얻을 수 있습니다.

Chart.js로 어떻게 할 수 있는지 봅시다. `옵션` 내에서 `툴팁` 구성으로 재생해야 합니다.

이러한 구성을 하나씩 살펴보면서 모든 것이 어떻게 작동하는지 알아보겠습니다.

이제 툴팁에 대한 HTML 섹션을 만들어 보겠습니다.

툴팁을 배치할 때 차트 컨테이너에 상대적이 되도록 차트를 포장해야 합니다. 두 개의 빈 섹션이 있습니다. 하나는 레이블용이고 다른 하나는 필요할 때 동적으로 업데이트할 값입니다.

이제 `프로세스`를 만들어 보겠습니다.툴팁을 업데이트하는 툴팁 모델 기능:

여기서 무슨 일이 일어나고 있는지 설명하자. 툴팁 모델에 본체가 없다면 보여줄 것이 없다는 뜻이고 그래서 우리는 `반환`하는 것이다. 그렇지 않으면 툴팁 모델의 값을 사용하여 툴팁을 동적으로 업데이트한다.

참고: Vue, React 또는 Angular를 사용하는 경우 도구 설명 모델 또는 도구 설명의 속성을 전달하여 도구 설명 구성 요소를 동적으로 배치할 수 있습니다.

`케어 X` 와 `케어Y는 툴팁의 X-와 Y-위치를 보여주며 이를 사용해 왼쪽과 위쪽 CSS 속성을 설정한다. 툴팁 요소를 표시하기 위해 디스플레이도 `블록`으로 설정합니다. 마지막으로, 우리는 값과 레이블 섹션을 실제 값으로 바꿉니다.

참고: 툴팁의 높이와 추가 비트를 기준으로 상단 위치에서 빼서 툴팁과 라인 사이에 약간의 여백을 만듭니다.

이 예제를 완료하려면 컨테이너에 대한 CSS와 모든 작업을 수행할 수 있는 툴팁을 참조하십시오.

그리고 이것이 결과입니다! 이 예에서는 툴팁이 매우 간단하지만 CSS를 사용하여 원하는 만큼 사용자 지정하여 원하는 결과를 만들 수 있습니다.

![GIF of final chart](https://miro.medium.com/max/1088/1*jutE_9egGcI0gpVjWNfMbg.gif)

## 추가 참고 사항

# 결론

Chart.js는 모든 프런트 엔드 애플리케이션을 위한 강력한 차트 라이브러리이다. 위에서 설명한 기본 사항을 배우면 미적으로 만족스러운 차트를 만들 수 있습니다.

이제 선, 그리드, 눈금 등을 사용자 정의하고 사용자 정의 HTML 도구 팁을 만들 수 있습니다. 이 지식을 사용하여 응용 프로그램에서 더 많은 시각화를 제공하는 유용한 차트를 만들 수 있습니다.

GitHub에서 완전히 작동하는 코드를 가진 요지를 찾을 수 있습니다.