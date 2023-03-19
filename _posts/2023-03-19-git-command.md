---
title: "[Git] git 관련 명령어"
categories:
  - Git
tags:
  - Git
  - 깃
  - git command
  - 깃 명령어
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

전세계적으로 많이 활용되는 형상관리 도구 Git을 제대로 활용하기 위해선 관련 명령어에 대해 명확히 알아야한다. 요즘은 Git 클라이언트 툴들이 너무 잘 만들어져 다양한 형태로 시장에 나와있는데, 이러한 툴들도 Git 명령어를 기반으로 동작하는 것이기 때문에 툴을 제대로 사용하기 위해서라도 명령어들을 이해할 필요가 있다. 때문에 이번 포스팅에선 관련 명령어들을 살펴보고자 한다.

# git init
---

```bash
git init [옵션] [<directory>]
```

git init은 새로운 Git 저장소를 만들 때 사용하는 명령어이다. 해당 명령어를 실행하면 .git 디렉토리가 생성되며, .git이 생성된 디렉토리의 경로를 포함한 하위의 모든 디렉토리 및 파일이 Git으로 형상관리가 시작된다. git init의 옵션에 대한 내용은 다음과 같다.

- [-q &#124; --quiet]

```bash
# example

git init -q 
git init -q test

git init --quiet
git init --quiet test
```
>-q 또는 --quiet 옵션은 Git 저장소를 초기화할 때 출력되는 메시지를 제거할 수 있다.

<br>

- [--bare]

```bash
# example

git init --bare
git init --bare test.git
git init --bare test
```

>--bare는 기본적으로 git init 명령어는 일반적인 Git 저장소를 만든다. 그러나 이 옵션을 사용하면 bare 저장소를 만들며, bare 저장소는 작업 디렉토리가 없는 Git 저장소다. 이러한 저장소는 공유 및 백업 목적으로 사용된다.

<br>

- [--template=&#60;template-directory>]

```bash
# example

git init --template=/usr/share/git-core/templates
git init --template=/usr/share/git-core/templates test
```

>이 옵션을 사용하면 사용자 지정 Git 저장소 템플릿을 사용할 수 있다. template-directory는 Git 저장소 템플릿이 포함 된 디렉토리의 경로다.

__알아두기__  
Git 저장소 템플릿이란 Git 저장소에 필요한 파일과 설정값들이 미리 정의된 디렉토리다.

<br>

- [--separate-git-dir &#60;git-dir>]

```bash
# example

git init --separate-git-dir ~/test-git-dir
git init --separate-git-dir ~/test-git-dir test
```

>이 옵션을 사용하면 작업 디렉토리와 Git 저장소를 분리할 수 있다. Git 저장소는 git-directory에 생성되며 작업 디렉토리는 디렉토리 내에서 지정된다. 해당 옵션의 장점은 저장소의 크기를 줄일 수 있으며, 여러 저장소에서 동일 .git 디렉토리를 공유할 수 있다는 것이다.

<br>

- [--object-format=&#60;format>]

```bash
# example

git init --object-format=sha256
git init --object-format=sha256 my_repo
```

>git init 명령어에서 사용할 객체 형식(object format)을 지정하는 옵션이다. Git의 기본 오브젝트 파일 형식은 SHA1이며, 해당 옵션을 통해 다른 알고리즘으로 오브젝트 형식을 지정할 수 있다.  
>
>--object-format옵션은 Git 2.11 이상에서 지원되며, 이전 버전에서는 무시된다. 오브젝트 파일 형식을 업그레이드 한다면 보다 보안성을 높일 수 있다.

<br>

- [-b &#60;branch-name> &#124; --initial-branch=&#60;branch-name>]

```bash
# example

git init -b main
git init -b main test

git init --initial-branch=main
git init --initial-branch=main test
```

>이 옵션을 사용하면 초기 브랜치 이름을 지정할 수 있다. 이 옵션을 사용하지 않으면 기본값으로 'master' 브랜치가 만들어진다.

<br>

- [--shared&#91;=&#60;permissions>&#93;]

```bash
# example

git init --shared
git init --shared test

git init --shared=group
git init --shared=group test 
```

>git init 명령어에서 생성된 Git 저장소에 대한 공유 권한을 지정하는 옵션이다. permissions옵션에는 group과 all이 있으며, --shared를 단독으로 사용한다면 저장소의 권한이 umask로 설정된다.

<br>

---

읽어주셔서 감사합니다. 😊 

__Reference__  
[Git 공식 문서 - Git](https://git-scm.com/docs)  