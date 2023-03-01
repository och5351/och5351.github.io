---
title: "Ubuntu Oh My zsh 설정"
toc: true
# header:
# image: /assets/images/nifi/nifi_logo.svg
# caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Setting
  - Zookeeper
toc_sticky: true
date: 2023-03-01
last_modified_at: 2023-03-01
---

<br><br>

# 1. zsh 설치

<br>

```bash
# zsh 설치
sudo apt-get install zsh

# 터미널 기본 셸 변경
sudo chsh -s $(which zsh)

# 세션 종료
exit

# 재로그인
```

<br><br>

# 2. .zshrc 파일 만들기

아래와 같은 문구가 뜨면 0을 선택하여 `.zshrc` 파일을 만들어 준다.

```bash
This is the Z Shell configuration function for new users,
zsh-newuser-install.
You are seeing this message because you have no zsh startup files
(the files .zshenv, .zprofile, .zshrc, .zlogin in the directory
~).  This function can help you with a few settings that should
make your use of the shell easier.

You can:

(q)  Quit and do nothing.  The function will be run again next time.

(0)  Exit, creating the file ~/.zshrc containing just a comment.
     That will prevent this function being run again.

(1)  Continue to the main menu.

(2)  Populate your ~/.zshrc with the configuration recommended
     by the system administrator and exit (you will need to edit
     the file by hand, if so desired).

--- Type one of the keys in parentheses ---
```

# 3. oh-my-zsh 설치

<br>

```bash
# oh-my-zsh 설치
# wget 사용
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# curl 사용
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

<br><br>

# 4. powerlevel10k 적용

<br><br>

```bash
# 저장소에서 테마 복사
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

<br>

# 5. zsh 설정파일 수정

<br>

```bash
# zsh 설정 파일 수정
sudo vi ~/.zshrc

# 파일 내 ZSH_THEME 값 변경
# 기본값이 ZSH_THEME="robbyrussell"로 되어 있다
ZSH_THEME="powerlevel10k/powerlevel10k"

# 터미널에서 256 컬러로 나타나게 설정
export TERM=xterm-256color

source .zshrc
```

<br><br>

# 6. 설정

<br>

1. 다이아몬드가 제대로 보인다면 `y`

```bash
   This is Powerlevel10k configuration wizard. You are seeing it because you haven't
  defined any Powerlevel10k configuration options. It will ask you a few questions and
                                 configure your prompt.

                    Does this look like a diamond (rotated square)?
                      reference: https://graphemica.com/%E2%97%86

                                     --->    <---

(y)  Yes.

(n)  No.

(q)  Quit and do nothing.

Choice [ynq]:
```

2. 자물쇠 표시가 제대로 보인다면 `y`

```bash
                              Does this look like a lock?
                     reference: https://fontawesome.com/icons/lock

                                     --->    <---

(y)  Yes.

(n)  No.

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [ynrq]:
```

3. 데비안 로고가 제대로 보인다면 `y`

```bash
                   Does this look like a Debian logo (swirl/spiral)?
                  reference: https://debian.org/logos/openlogo-nd.svg

                                     --->    <---

(y)  Yes.

(n)  No.

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [ynrq]:
```

4. `X` 사이에 아이콘들이 잘 보인다면 `y`

```
                      Do all these icons fit between the crosses?

                             --->  XXXXXXXXX  <---

(y)  Yes. Icons are very close to the crosses but there is no overlap.

(n)  No. Some icons overlap neighbouring crosses.

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [ynrq]:
```

5. 프롬프트 스타일 선택 (3)

```bash
                                      Prompt Style

(1)  Lean.

  ~/src master                                                                      5s
  ❯

(2)  Classic.

  ╭─ ~/src  master                                                            5s ─╮
  ╰─                                                                                ─╯

(3)  Rainbow.

  ╭─ ~/src  master                                                            5s ─╮
  ╰─                                                                                ─╯

(4)  Pure.

  ~/src master 5s
  ❯

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [1234rq]:
```

6. Charactor Set을 선택 (1)

```
                                     Character Set

(1)  Unicode.

  ╭─ ~/src  master                                                            5s ─╮
  ╰─                                                                                ─╯

(2)  ASCII.

   ~/src  master                                                                   5s
  >

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [12rq]:
```

7. 현재시간을 어떻게 표현 할 것인지? (2)

```bash
                                   Show current time?

(1)  No.

  ╭─ ~/src  master                                                            5s ─╮
  ╰─                                                                                ─╯

(2)  24-hour format.

  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(3)  12-hour format.

  ╭─ ~/src  master                                              5s  04:23:42 PM ─╮
  ╰─                                                                                ─╯

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [123rq]:
```

8. 프롬프트의 구분을 어떻게 모양으로 할 것인지 선택 (1)

```bash
                                   Prompt Separators
              separator
(1)  Angled. /
            /
  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(2)  Vertical.

  ╭─ ~/src  master                                                   5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(3)  Slanted.

  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(4)  Round.

  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [1234rq]:
```

9. 프롬프트의 머리 부분을 어떻게 표현할 것인지 선택 (1)

```bash
                                      Prompt Heads
                   head
(1)  Sharp.         |
                    v
  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(2)  Blurred.

  ╭─ ~/src  master ▓▒░                                           ░▒▓ 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(3)  Slanted.

  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(4)  Round.

  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [1234rq]:
```

10. 프롬프트 끝 부분을 어떻게 표현할 것인지 선택 (1)

```bash
                                      Prompt Tails
                                                                                 tail
(1)  Flat.                                                                         |
                                                                                   v
  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(2)  Blurred.

  ╭─░▒▓ ~/src  master                                           5s  16:23:42 ▓▒░─╮
  ╰─                                                                                ─╯

(3)  Sharp.

  ╭─ ~/src  master                                               5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(4)  Slanted.

  ╭─ ~/src  master                                               5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(5)  Round.

  ╭─ ~/src  master                                               5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [12345rq]:
```

11. Prompt를 어느 위치에 둘 것인지? (1)

```bash
                                     Prompt Height

(1)  One line.

   ~/src  master                                                     5s  16:23:42

(2)  Two lines.

  ╭─ ~/src  master                                                 5s  16:23:42 ─╮
  ╰─                                                                                ─╯

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [12rq]:
```

12. 프롬프트 작성 후 띄우기를 어떻게 할 거 것인지? (1)

```bash
                                     Prompt Spacing

(1)  Compact.

   ~/src  master                                                     5s  16:23:42
   ~/src  master                                                     5s  16:23:42

(2)  Sparse.

   ~/src  master                                                     5s  16:23:42

   ~/src  master                                                     5s  16:23:42

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [12rq]:
```

13. ICON을 표시할 것인지?(2)

```bash
                                         Icons

(1)  Few icons.

   ~/src  master                                                     5s  16:23:42

(2)  Many icons.

      ~/src    master                                       5s   16:23:42 

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [12rq]:
```

14. from, took, on 등의 문자를 표시할 것인지?(1)

```bash
                                      Prompt Flow

(1)  Concise.

      ~/src    master                                       5s   16:23:42 

(2)  Fluent.

      ~/src  on   master                            took 5s   at 16:23:42 

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [12rq]:
```

15. 연속된 명령어는 어떻게 표현할 것인지? (n)

```bash
                                Enable Transient Prompt?

(y)  Yes.

  ❯ git pull
  ❯ git branch x

      ~/src    master  git checkout x

(n)  No.

      ~/src    master  git pull

      ~/src    master  git branch x

      ~/src    master  git checkout x

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [ynrq]:
```

16. Verbose (1)

```bash
                                  Instant Prompt Mode

     https://github.com/romkatv/powerlevel10k/blob/master/README.md#instant-prompt

(1)  Verbose (recommended).

(2)  Quiet. Choose this if you've read and understood instant prompt documentation.

(3)  Off. Choose this if you've tried instant prompt and found it incompatible with your
     zsh configuration files.

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [123rq]:
```

17. Yes (y)

```bash
                               Apply changes to ~/.zshrc?

(y)  Yes (recommended).

(n)  No. I know which changes to apply and will do it myself.

(r)  Restart from the beginning.
(q)  Quit and do nothing.

Choice [ynrq]:
```

<br><br>
