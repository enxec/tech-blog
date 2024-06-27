---
title: "[Git] git 관련 명령어"
description: 
author: Enxec
date: 2023-03-19
categories: [Git]
tags: [git, 깃, git command, 깃 명령어]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/git-logo.png
  lqip: 
  alt: 
---

전세계적으로 많이 활용되는 형상관리 도구 Git을 제대로 활용하기 위해선 관련 명령어에 대해 명확히 알아야한다. 요즘은 Git 클라이언트 툴들이 너무 잘 만들어져 다양한 형태로 시장에 나와있는데, 이러한 툴들도 Git 명령어를 기반으로 동작하는 것이기 때문에 툴을 제대로 사용하기 위해서라도 명령어들을 이해할 필요가 있다. 때문에 이번 포스팅에선 관련 명령어들을 살펴보고자 한다.

## git init
---

```bash
git init [옵션] [<directory>]
```

git init은 새로운 Git 저장소를 만들 때 사용하는 명령어다. 해당 명령어를 실행하면 .git 디렉토리가 생성되며, .git이 생성된 디렉토리의 경로를 포함한 하위의 모든 디렉토리 및 파일이 Git으로 형상관리가 시작된다. git init의 옵션에 대한 내용은 다음과 같다.

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

## git clone
---

```bash
git clone [옵션] <repository> [<directory>]
```

git clone은 Git 저장소(repository)를 로컬 컴퓨터로 복사하는 명령어다. 보통 다른 개발자가 작성한 코드를 가져와서 수정하거나, 또는 협업 프로젝트에 참여하기 위해서 다른 저장소의 코드를 내 컴퓨터로 복제할 때 사용한다. git clone의 옵션에 대한 내용은 다음과 같다.

- [--template=&#60;template-directory>]

```bash
# example

git clone --template=/usr/share/git-core/templates https://github.com/user/my-project.git 
git clone --template=/usr/share/git-core/templates https://github.com/user/my-project.git test
```
>이 옵션을 사용하면 사용자 지정 Git 저장소 템플릿을 사용할 수 있다. template-directory는 Git 저장소 템플릿이 포함 된 디렉토리의 경로다.

<br>

- [-l]

```bash
# example

git clone -l /path/to/local/repo
git clone -l /path/to/local/repo test
```
>git clone 명령어를 -l 옵션 없이 사용하면 원격 저장소를 복제하지만, -l 옵션을 사용하여 clone 명령어를 실행한다면 로컬 저장소를 복제합니다.

<br>

- [-s]

```bash
# example

git clone -s https://github.com/user/my-project.git
git clone -s https://github.com/user/my-project.git test
```
>-s 옵션은 서브모듈(submodule)을 포함하여 저장소를 복제할 때 사용하는 옵션이다. Git에서 서브모듈은 다른 Git 저장소를 프로젝트의 하위 디렉토리로 추가하는 기능이다. 서브모듈을 사용하면 여러 개의 Git 저장소를 하나의 프로젝트로 관리할 수 있다.

<br>

- [--no-hardlinks]

```bash
# example

git clone --no-hardlinks https://github.com/user/my-project.git
git clone --no-hardlinks https://github.com/user/my-project.git test
```
>해당 옵션은 하드링크를 사용하지 않고 파일을 복제할 때 사용하는 옵션이다.

<br>

- [-q]

```bash
# example

git clone -q https://github.com/user/my-project.git
git clone -q https://github.com/user/my-project.git test
```
>해당 옵션은 실행 중 출력되는 메시지를 최소화하는 옵션이다.

<br>

- [-n]

```bash
# example

git clone -n https://github.com/user/my-project.git
git clone -n https://github.com/user/my-project.git test
```
>'-n' 옵션은 '--no-checkout'과 함께 사용하여 복제된 저장소를 초기화하지 않고 작업 트리를 생성하지 않는 옵션이다. git clone 명령어는 저장소를 복제할 때, 원격 저장소의 모든 파일과 커밋을 가져와서 로컬 저장소를 초기화하고 작업 트리를 생성한다. 하지만 -n 옵션을 사용하면 초기화하지 않고 작업 트리를 생성하지 않아, 저장소를 복제하고 나중에 필요할 때 초기화할 수 있다.

<br>

- [--bare]

```bash
# example

git clone --bare https://github.com/user/my-project.git
git clone --bare https://github.com/user/my-project.git test
```
>'--bare' 옵션은 Git 저장소를 작업 트리 없이 복제할 때 사용하는 옵션이다. Git 저장소는 일반적으로 작업 트리와 함께 사용되는데, 작업 트리는 Git 저장소에 포함된 파일과 폴더를 포함하며, 사용자가 파일을 수정하고 커밋하도록 허용한다. 그러나 작업 트리를 포함하지 않는 Git 저장소도 존재하며, 이러한 저장소를 bare 저장소 또는 naked 저장소라고 부르고 일반적으로 Git 서버에서 사용된다.

<br>

- [--mirror]

```bash
# example

git clone --mirror https://github.com/user/my-project.git
git clone --mirror https://github.com/user/my-project.git test
```
>'--mirror' 옵션은 원본 저장소의 모든 브랜치, 태그, 설정, 레퍼런스 등을 복제하는 데 사용되는 옵션이다. 이 옵션은 원본 저장소의 모든 이력을 복제하고, 저장소를 업데이트할 때 원본 저장소와 정확히 동일하게 유지한다.

<br>

- [--mirror]

```bash
# example

git clone --mirror https://github.com/user/my-project.git
git clone --mirror https://github.com/user/my-project.git test
```
>'--mirror' 옵션은 원본 저장소의 모든 브랜치, 태그, 설정, 레퍼런스 등을 복제하는 데 사용되는 옵션이다. 이 옵션은 원본 저장소의 모든 이력을 복제하고, 저장소를 업데이트할 때 원본 저장소와 정확히 동일하게 유지한다.

<br>

- [-o &#60;name>]

```bash
# example

git clone -o myremote https://github.com/user/my-project.git
git clone -o myremote https://github.com/user/my-project.git test
```
>'--mirror' 옵션은 원본 저장소의 모든 브랜치, 태그, 설정, 레퍼런스 등을 복제하는 데 사용되는 옵션이다. 이 옵션은 원본 저장소의 모든 이력을 복제하고, 저장소를 업데이트할 때 원본 저장소와 정확히 동일하게 유지한다.

<br>

- [-o &#60;name>]

```bash
# example

git clone -o myremote https://github.com/user/my-project.git
git clone -o myremote https://github.com/user/my-project.git test
```
>'-o <name>' 옵션은 로컬 저장소에 추가할 원격 저장소의 이름을 지정하는 데 사용된다. 원격 저장소를 복제할 때, Git은 기본적으로 origin이라는 이름의 원격 저장소를 생성한다. 그러나 -o 옵션을 사용하여 다른 이름의 원격 저장소를 생성할 수 있다.

<br>

- [-b &#60;name>]

```bash
# example

git clone -b mybranch https://github.com/user/my-project.git
git clone -b mybranch https://github.com/user/my-project.git test
```
>'-b <name>' 옵션은 복제할 원격 저장소의 브랜치를 선택하는 데 사용된다. Git은 기본적으로 원격 저장소의 master 브랜치를 복제한다. 그러나 -b 옵션을 사용하여 다른 브랜치를 선택할 수 있다.

<br>

- [-u &#60;upload-pack>]

```bash
# example

git clone -u /path/to/git-upload-pack https://github.com/user/my-project.git
git clone -u /path/to/git-upload-pack https://github.com/user/my-project.git test
```
>'-u <upload-pack>' 옵션은 git-upload-pack 프로그램의 경로를 지정하는 옵션이다. git-upload-pack 프로그램은 원격 저장소에서 객체를 가져오는 데 사용된다. Git은 기본적으로 원격 저장소에서 git-upload-pack 프로그램을 찾기 위해 PATH 환경 변수를 검색하는데 -u 옵션을 사용하여 명시적으로 git-upload-pack 프로그램의 경로를 지정할 수 있다.

<br>

- [--reference &#60;repository>]

```bash
# example

git clone --reference /path/to/another/repo https://github.com/user/my-project.git
git clone --reference /path/to/another/repo https://github.com/user/my-project.git test
```
>'--reference <repository>' 옵션은 이미 존재하는 다른 로컬 저장소를 참조하여 복제하는 옵션이다. 이 옵션을 사용하면 복제 작업이 더 빠르게 수행될 수 있다.

<br>

---

읽어주셔서 감사합니다. 😊 

__Reference__  
[Git 공식 문서 - Git](https://git-scm.com/docs)  