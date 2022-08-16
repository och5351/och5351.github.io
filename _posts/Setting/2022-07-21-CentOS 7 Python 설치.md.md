---
title: "CentOS 7 Python 설치"
toc: true
# header:
  # image: /assets/images/nifi/nifi_logo.svg
  # caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Setting
  - Linux
  - Python
toc: true
toc_sticky: true

date: 2022-07-21
last_modified_at: 2022-08-16

---

<br>

1. tgz 파일 다운

```bash
wget https://www.python.org/ftp/python/Python-3.6.8.tgz
```

2. 압축 해제

```bash
tar -xvf Python-3.6.8.tgz
```

3. 파이썬 설치

```bash
cd Python-3.6.8
./configure
make altinstall
```

4. alias 등록

```bash
cd /
find . -name python3.6

vi ~/.bashrc

alias python='/usr/local/bin/python3.6'
alias pip='/usr/local/bin/pip3.6'

source ~/.bashrc
```

<br><br>

# SSL 해제

<br>

1. OpenSSL 설치

```bash
yum -y install openssl openssl-devel
```

2. Setup 경로 확인

```bash
find / -name Setup
```

3. Setup 주석해제

```bash
vi `Setup 경로`

# _socket soketmodule.c


#SSL=/usr/local/ssl
#_ssl _ssl.c \
#       -DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
#       -L$(SSL)/lib -lssl -lcrypto
```

4. 재 설정 & 설치

```bash
./configure
make altinstall
```

5. pip 패키지 설치

```
pip install --trusted-host pypi.python.org --trusted-host files.pythonhosted.org --trusted-host pypi.org 패키지명
```
