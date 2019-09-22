
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190922-lightgray.svg?style=flat-square)

# To agility and beyond: The history-and legacy-of agile development

소프트웨어에서 ["Continuous delivery"(지속적 공급)](https://en.wikipedia.org/wiki/Continuous_delivery) 는 more than a buzz phrase.
그렇다. 소프트웨어 continuous delivery 는 개발 방법론, 고객 유지에서 신성한 위치를 차지하며,
이것이야말로 DevOps 가 오늘날 그렇게도 뜨거운 개념이 된 이유이다.
continuous delivery 의 중요성을 이해하기 위해서 agility 에 대한 역사와 애자일이 비롯된 지점에 대해 알 필요가 있다.

애자일 소프트웨어 개발의 역사는 [Agile Manifesto(애자일 선언서)](http://techbeacon.com/50-shades-agile-software-development-manifesto) 로부터
시작된 것이 아니다. 그것의 뿌리는 보다 더 이전으로 돌아가야 알 수 있다.
이 기사는 애자일의 근원과 어떻게 더 많은 최신의 지식들이 우리를 매우 빠른 속도로 deliver cycles 로 이끌고 있는지를 포함하는
30년 이상의 소프트웨어 개발 방법론의 진화에 대해 살펴볼 것이다.

- 바로 지금이 당신의 릴리즈 관리 전략을 다시 생각해볼 때가 아닌가? [왜 적응적 릴리즈 방식이 DevOps 성공의 본질일까? (Gartner)](https://www.microfocus.com/en-us/assets/application-delivery-management/use-adaptive-release-governance-to-remove-constraints-to-devops?utm_source=techbeacon&utm_medium=techbeacon&utm_campaign=00134846)

## First came the crisis

PC computing 이 기업 규모로 증식하기 시작하던 1990 년대 초반, 소프트웨어 개발론은 위기를 맞았다.
그때 이것은 "the application development crisis," 또는 "application delivery lag.(소프트웨어 공급 지연)" 와 같이 널리 언급되었다.
산업 전문가들은 비즈니스 요구사항 검증과 실제 제품으로써의 어플리케이션 출시 간에 약 3년의 시간적 편차가 존재한다고 평가했다.

문제는 비즈니스는 심지어 25년 이전부터 그것보다 더 빠르게 움직이고 있었다는 것이었다.
(출시되기까지) 3년의 공백 안에 요구사항, 시스템, 심지어 전체 비즈니스는 바뀔 가능성이 있었다.
이는 많은 프로젝트의 결과물이 (개발) 도중에 취소되며, 심지어 완성된 결과물 중의 대다수가 프로젝트의 본래 목적과 일치함에도 불구하고
비즈니스의 최신 요구사항에는 부합하지 못했다는 것을 의미했다.

특정 산업에서 지연(the lag)은 3년보다 더 길다. 항공우주방위에서는 복잡한 시스템이 실제 사용 가능해지기까지 20년 이상이 걸리기도 한다.
우주 왕복 프로그램은, 극단적이기는 하지만 결코 특별한 예를 의미하는 것은 아닌데,
1982년에 운용이 시작되기까지 1960년대의 정보처리 기술을 사용했다.
종종 매우 복잡한 하드웨어와 소프트웨어 시스템이 수십년에 걸친 시간적 frame 동안에 디자인, 개발, 그리고 배포된다.

- Get report: [상위 20개의 지속적인 어플리케이션 퍼포먼스 관리 기업들](https://www.microfocus.com/en-us/assets/it-operations-management/research-in-action-continuous-application-performance-management-saas-and-software?utm_source=techbeacon&utm_medium=techbeacon&utm_campaign=00134846)

## Thought leaders were frustrated

1990년대 항공우주 엔지니어인 Jon Kern 는 이러한 긴 진행기간과 향후 바꿀 수 없는 프로젝트 초기에 결정된 사항들에 점점 실망하고 있었다.
소프트웨어를 만드는 더 좋은 방법이 있어야겠다고 느끼는 사람들이 증가하는 가운데 그는 다음과 같이 기록했다.
"우리는 더 시의적절하고 유연한 무언가를 찾고 있었다."
그는 비공식적으로 모임을 시작하여 절차와 과도한 문서와 당시의 다른 유명한 소프트웨어 엔지니어링 기술 없이
소프트웨어 개발을 보다 단순화하는 방법에 대해 토론하는 17개 소프트웨어 그룹(thought) 리더 중의 하나였다.

다른 산업들도 마찬가지로 변화를 겪고 있었다.
자동차 산업은 새로운 자동차를 디자인하는 것에 6년 혹은 그 이상이 걸렸는데, 1990년대에 이르러서는 그 시간이 거의 절반으로 줄었다.
AT&T 가 해체되었고, 소위 Baby Bells 는 과감하게 휴대전화와 서비스의 비용을 절감했다.

전화기 스위치, 자동차, 또는 비행기와 같이 소프트웨어 개발 컴포넌트가 포함된 제품들에서 소프트웨어는 대개 뒷전이 되고는 했는데,
그 이유는 하드웨어 디자인이 결정되기 전까지는 소프트웨어 개발을 시작할 수 없었기 때문이다.
당시 소프트웨어를 만드는 것은 대부분의 제품 개발 팀에게 우선순위가 아니었다.

## And agile was born

겉보기에 비생산적인 소프트웨어 개발 활동을 둘러싼 이러한 좌절은 
