---
title: "CentOS7 Loacl Repository 구성"
toc: true
# header:
  # image: /assets/images/nifi/nifi_logo.svg
  # caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Linux
  - Setting
toc: true
toc_sticky: true

date: 2022-07-18
last_modified_at: 2022-07-18

---

<br>

# 필요한 경우

<br>

해당 로컬 Repo는 폐쇄망일 경우 어떤 rpm을 설치해야 할지 모르고 외부망에 붙을 수 없는 경우 처음부터 Repo 들을 가지고 들어와서 구성하여 사용하는 경우에 이용 된다.

<br>

반드시 외부망에 붙을 수 있는 PC 가 있어야 하며, 폐쇄망으로 해당 Repo 파일들을 옮길 수 있어야 한다. 

<br><br>

# Local Repository 다운

<br>

1. Kaist 공개 Repository 에서 외부망에 붙을 수 있는 머신에서 다운 받는다.

```bash
http://ftp.kaist.ac.kr/CentOS/7.9.2009/
```

<br>

2. 기본 Repository는 OS, Extras, Updates, Centosplus 를 받는다.

```
wget --recursive --no-parent http://ftp.kaist.ac.kr/CentOS/7.9.2009/os/x86_64/
wget --recursive --no-parent http://ftp.kaist.ac.kr/CentOS/7.9.2009/extras/x86_64/
wget --recursive --no-parent http://ftp.kaist.ac.kr/CentOS/7.9.2009/updates/x86_64/
wget --recursive --no-parent http://ftp.kaist.ac.kr/CentOS/7.9.2009/centosplus/x86_64/
```

<br>

3. 해당 파일들을 폐쇄망 PC로 옮겨 준다.

<br>

4. 폐쇄망 PC에서 적당한 장소에 x86_64 하위 내용들을 옮겨 담아준다.


```
mkdir /kaist_repo_os
mv ./x86_64/* /kaist_repo_os
```

<br>

5. Repo 리스트를 만들어 준다.

```
vi /etc/yum.repo.d/kaist.repo

[kaist-os]
name=kaist-os
baseurl=file:///kaist_repo_os
enabled=1
gpgcheck=0

[kaist-updates]
name=kaist-updates
baseurl=file:///kaist_repo_updates
enabled=1
gpgcheck=0

[kaist-oextras]
name=kaist-extras
baseurl=file:///kaist_repo_extras
enabled=1
gpgcheck=0

[kaist-plus]
name=kaist-centosplus
baseurl=file:///kaist_repo_centosplus
enabled=1
gpgcheck=0
```

<br>

6. yum repolist를 새로고친다.

```
yum clean all
```

<br>

7. yum repolist를 확인하여 repository 활성화 여부를 확인한다.

```
yum repolist
```

