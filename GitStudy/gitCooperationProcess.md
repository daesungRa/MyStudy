
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190623-lightgray.svg?style=flat-square)

# Cooperation Process using Git, Github

> Git, Github 은 그 직관성과 편리성에 의해 광범위하게 사용되는 VCS 이다.

> Git, Github 을 위한 협업의 방식은 'Feature Branch Workflow', 'Forking Workflow 의 두 가지로 요약 가능하다.

## Feature Branch Workflow

1. 팀원들이 공동으로 사용하는 **Organization 계정 (그룹 계정)** 을 생성한다.
2. **Main remote repository** 를 만들고, 각 팀원은 그것을 **자신의 로컬에 직접 clone** 하여 작업한다.
3. 자신의 업무에 해당하는 **feature branch** 를 생성하고, 그곳에 변경 사항을 add 후 commit 한다.
4. 해당 branch 를 **main repository 에 직접 push** 함으로써 자신의 변경 사항을 공유한다.
5. Organization 은 각 변경 사항을 확인하고, **main 의 master branch 에 merge** 한다.

## Forking Workflow

- 각 팀원은 main repository 를 **자신의 계정에 Fork** 한다.
- 해당 fork 를 로컬에 clone 하고, branch 를 생성하여 변경 사항을 작업한다.
- commit 후 **fork 한 remote repository 에 push** 한다.
- **pull request** 를 main repository 에 보내면, 메인 관리자가 확인하고 comment 와 함께 merge 혹은 reject 한다.



