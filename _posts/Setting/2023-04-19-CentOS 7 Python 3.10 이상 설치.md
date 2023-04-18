---
title: "CentOS 7 Python 3.10 이상 설치"
toc: true
layout: posts
categories:
  - Setting
tags:
  - Setting
  - Python
toc_sticky: true
date: 2023-04-19
last_modified_at: 2023-04-19
---

<br><br>

CentOS 7 기준

python 3.10 이상 부터 openssl version 1.1.1 이상을 요구

<br><br>

# 1. Repository 변경

<br>

```bash
yum install epel-release -y
```

<br>

## 아래와 같은 yum error 시

<br>

```
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Loading mirror speeds from cached hostfile
Loading mirror speeds from cached hostfile
Loading mirror speeds from cached hostfile
Loading mirror speeds from cached hostfile


 One of the configured repositories failed (Unknown),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=<repoid> ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>
        or
            subscription-manager repos --disable=<repoid>

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true
```

<br>

## 해결법

<br>

```bash
vi /etc/yum.repos.d/epel.repo
# maetalink 를 주석 처리하고 baseurl 을 살려준다.
```



<br><br>

# 2. OpenSSL 1.1.1 버전 및 의존성 설치

<br>

```bash
yum install openssl11 openssl11-devel -y 
yum -y install yum-utils
yum -y install mariadb-devel
yum -y install zlib zlib-devel libffi-devel bzip2-devel
yum -y install gcc gcc-c++ openssl openssl-devel 
yum -y install zip unzip wget mc git net-tools
```

<br><br>

# 3. OpenSSL 버전 확인

<br>

```bash
openssl11 version 
```

<br><br>

# 4. OpenSSL 전역변수 설정

<br>

```bash
export CFLAGS=$(pkg-config --cflags openssl11)
export LDFLAGS=$(pkg-config --libs openssl11)
```

<br><br>

# 5. Python 설치

<br>

```bash
cd /opt

wget https://www.python.org/ftp/python/3.11.3/Python-3.11.3.tgz  --no-check-certificate

tar -zxvf Python-3.11.3.tgz

cd Python-3.11.3

./configure
make
sudo make install
```

<br><br>

# 6. Python 환경변수 설정 (alias)

```bash
vi /etc/profile

alias python3='/usr/local/bin/python3.11'
alias python='/usr/local/bin/python3.11'
alias pip='/usr/local/bin/pip3.11'

source /etc/profile
```

<br><br>

# 7. 설치 확인

<br>

```bash
python
```