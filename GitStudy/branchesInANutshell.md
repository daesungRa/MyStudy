
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190624-lightgray.svg?style=flat-square)

# [GIT-SCM 번역] Git Branching - Branches in a Nutshell

- 거의 모든 VCS 는 branching 기능을 지원하는 몇몇 폼을 가지고 있다.
- **Branching** 이란 development 의 main line 으로부터 **분기되어 나오는 것**을 의미하며,
- 그것(main line)을 어지럽히지 않으면서 작업을 지속하도록 하는 것을 뜻한다.
- 많은 VCS 툴에서 이러한 것(branching)은 값비싼 프로세스인데, 종종 당신의 소스 코드 디렉토리의 새로운 복사본을 생성할 것을 요구하기도 한다.
- 규모가 큰 프로젝트는 이에 소요되는 기간이 길다.
- 몇몇은 Git 의 branching 모델을 가르켜 "killer feature(죽여주는 기능?)" 로 부르는데, 그것은 확실히 Git 을 VCS 커뮤니티에서 구분되도록 만들었다.
- 그것은 왜 그렇게 특별한가?
- Git branches 의 과정은 놀라우리만치 가볍고, branching operations 를 만드는 것은 거의 순식간에 이루어지며, 앞뒤 브랜치 간의 전환이 대체로 빠르다.
- 다른 많은 VCS 들과 다르게, Git 은 **잦은 branch 와 merge 가 하루에도 몇번 씩 이루어지는 workflows** 를 장려한다.
- 이 feature 를 이해하고 마스터하는 것은 당신에게 강력하고 유일한 tool 을 제공하며, 당신의 개발 방법을 통째로 바꿀 수 있다.

## 브랜치 요약설명

- Git branching 과정에 대한 진정한 이해를 위해 우리는 몇 걸음 물러나 어떻게 Git 이 자신의 data 를 저장하는지 시험해 볼 필요가 있다.
- [Getting Started](https://git-scm.com/book/en/v2/ch00/ch01-getting-started) 에서 봤듯이, Git 은 데이터를 changesets 나 differences 의 연속 모음(series)이 아닌 snapshots 의 연속 모음(series)으로써 저장한다.
- 당신이 commit 할 때, Git 은 당신이 staged 한 내용의 snapshot 을 가리키는 포인터를 포함한 commit 객체를 저장한다.
- 이 객체는 또한 작성자의 이름과 이메일 주소, 입력한 메시지, 현재 커밋의 바로 이전 커밋 혹은 커밋들에 대한 포인터를 포함한다. (its parent or parents)
- 최초 커밋은 zero parents, 일반 커밋은 one parent, 둘 또는 그 이상의 브랜치들의 merge 로부터 말미암는 커밋에 대해서는 multiple parents.
- 이것을 나타내기 위해서, 세 개의 파일을 포함하는 디렉토리가 있고, 그것들을 모두 stage 하고 commit 했다고 가정하자.
- Staging the files 는 각각의 file 들에 대한 a checksum 을 계산하고(SHA-1), 해당 파일의 버전을 Git repository 에 저장하고(Git 는 그것들을 blob 형식으로 보낸다), 그 checksum 을 staging area 로 adds 한다.
```bash
$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
```
- 당신이 ```git commit``` 을 실행함으로써 commit 을 생성할 때, Git 은 각 subdirectory 를 checksums 하고(여기서는 그냥 프로젝트 root dir), 그것을 Git repository 의 트리 객체로써 저장한다.
- 이때 Git 은 metadata 와 the root project tree 에 대한 포인터를 가진 commit 객체를 생성한다. 이는 필요에 따라 해당 snapshot 을 재생성하도록 하기 위함이다.
- 이제 당신의 Git repository 는 다섯 개의 객체를 갖는다.
    * three blobs(세 개의 파일들 각각을 대표하는)
    * one tree(디렉토리 내용을 목록화하고 어떤 파일들이 어떤 blobs 로 저장되어 있는지 특정하는)
    * and one commit(root tree 와 모든 커밋의 metadata 에 대한 포인터를 포함하는)

![A commit and its tree](https://github.com/daesungRa/MyStudy/blob/master/imgs/git-scm/gitbranch01.png)

- 




