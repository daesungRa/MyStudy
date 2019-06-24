
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190624-lightgray.svg?style=flat-square)

# [GIT-SCM 번역] Git Branching - Branches in a Nutshell

&nbsp;거의 모든 VCS 는 branching 기능을 지원하는 몇몇 폼을 가지고 있다.
**Branching** 이란 development 의 main line 으로부터 **분기되어 나오는 것**을 의미하며,
그것(main line)을 어지럽히지 않으면서 작업을 지속하도록 하는 것을 뜻한다.
많은 VCS 툴에서 이러한 것(branching)은 값비싼 프로세스인데, 종종 당신의 소스 코드 디렉토리의 새로운 복사본을 생성할 것을 요구하기도 한다. 규모가 큰 프로젝트는 이에 소요되는 기간이 길다.
몇몇은 Git 의 branching 모델을 가르켜 "killer feature(ㅇㅇㅇㅇ죽여주는 기능?)" 로 부르는데, 그것은 확실히 Git 을 VCS 커뮤니티에서 구분되도록 만들었다.
그것은 왜 그렇게 특별한가?
Git branches 의 과정은 놀라우리만치 가볍고, branching operations 를 만드는 것은 거의 순식간에 이루어지며, 앞뒤 브랜치 간의 전환이 대체로 빠르다.
다른 많은 VCS 들과 다르게, Git 은 **잦은 branch 와 merge 가 하루에도 몇번 씩 이루어지는 workflows** 를 장려한다.
이 feature 를 이해하고 마스터하는 것은 당신에게 강력하고 유일한 tool 을 제공하며, 당신의 개발 방법을 통째로 바꿀 수 있다.

