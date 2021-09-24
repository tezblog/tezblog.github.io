---
layout: post
title: "구름 IDE 에 Deno 설치"
updated: 2021-03-18
tags: [dev,tool]
---

## Deno

Javascript 언어로 백엔드 개발을 할 수 있도록 해주는 가장 유명한 플랫폼은 [Node.js](https://nodejs.org/ko/) 이나, 요새 [Deno](http://deno.land/) 라는 플랫폼도 새로 생겨 관심을 가지게 되었다.

주로 사용하는 [구름 IDE](https://ide.goorm.io/) 에서는 아직 Deno 개발 위한 템플릿을 지원을 하지 않는다. 그래서 실제로 진행한 설치법을 기록해두려 한다.

## 설치 순서

구름 IDE 에 접속, 새 컨테이너를 생성한다. 소프트웨어 스택에서 'blank' 를 선택했다.

터미널 화면에서 아래 명령어를 입력한다.

```bash
curl -fsSL https://deno.land/x/install/install.sh | sh
```
{:.bash}

설치를 하고 난 터미널 화면을 보면, 수동으로 환경설정을 하라는 아래와 같은 메시지를 확인할 수 있다.

```plaintext
Deno was installed successfully to /root/.deno/bin/deno
Manually add the directory to your $HOME/.bash_profile (or similar)
  export DENO_INSTALL="/root/.deno"
  export PATH="$DENO_INSTALL/bin:$PATH"
Run '/root/.deno/bin/deno --help' to get started
```
{:.text}

IDE 화면의 왼쪽 아래에 있는 설정 (⚙️ 모양) 버튼을 눌러 뜨는 파업에서 "터미널" > "프로필" 메뉴를 선택한다.

프로필 화면의 오른쪽 코딩부분 아래에 아래 두 줄을 추가한다.

```bash
export DENO_INSTALL="/root/.deno"
export PATH="$DENO_INSTALL/bin:$PATH"
```
{:.bash}

이제 터미널 화면으로 돌아가서, 터미널 탭에 있는 새로고침 (🔁 모양) 버튼을 누른다.

새로고침이 된 후 아래 명령어를 터미널에 입력하여 제대로 설치가 되었는지 확인해본다.

```bash
deno --version
```
{:.bash}