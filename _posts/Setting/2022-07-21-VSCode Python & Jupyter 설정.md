---
title: "VSCode Python & Jupyter 설정"
toc: true
# header:
  # image: /assets/images/nifi/nifi_logo.svg
  # caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Setting
  - VScode
  - Python
toc: true
toc_sticky: true

date: 2022-07-21
last_modified_at: 2022-07-21

---

<br>

### 1. VSCode 설치

<br>

반드시 Python 은 설치 되어 있어야 한다.

<br>

### 2. Microsoft Market 확장자 설치

<br>

> https://marketplace.visualstudio.com/

* Python
* Python Config
* Python Extension Pack
* Jupyter

설치는 `ctrl + shift + p` 후 `Install` 을 쳤을 때 나오는 Extentions: Install from vsix ... 클릭 후 파일 설치

<br>

### 3. .ipython 만들기

<br>

1. ctrl + shift + p 
2. create
3. jupyter 대화형 창 만들기

<br>

#### 4. vscode python jedi client: couldn't create connection to server 이슈

<br>

참고링크

> https://stackoverflow.com/questions/72210026/vscode-python-jedi-client-couldnt-create-connection-to-server

<br>

<div align='center'>
<img src='https://i.stack.imgur.com/vfdgD.png' width='70%'/>
</div>


<br>

처음 세팅 후 .ipython을 실행하면 해당 에러가 반복적으로 일어난다.

이때는 아래와 같은 순서를 따른다.

1. 파일 > 기본설정 > 설정(ctrl+,)
2. 검색 창에 python.language 
3. Python: Language Server <br> 언어 서버의 유형을 정의합니다. <br> default <br><br> 해당 부분은 Jedi 로 변경하여 준다.



<br>

#### 5. 마켓 어플리케이션 설치 호환 이슈

<br>

해결책을 마련 못 하였으므로 높은 버전의 VSCode를 설치하여 준다.