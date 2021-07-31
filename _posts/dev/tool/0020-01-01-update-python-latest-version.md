---
layout: post
title: "구름 IDE 에 Python 최신 버전 설치하기"
updated: 2021-03-27
tags: [dev,tool]
---

## Python 최신 버전 설치하기

Ubuntu 에는 기본적으로 Python 이 설치되어 있다. 구름 IDE 에서 빈 프로젝트를 생성해도 2021년 3월 현재 Python 3.6 이 설치되어 있다.

하지만 버전이 낮아서 최신 버전인 3.9 를 설치하고자 했고, 이런저런 정보를 구글링해서 찾아낸 방법을 포스팅해두고자 한다.

## 설치 순서

아래와 같이 하면 된다.

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.9
```
{:.bash}

이제 터미널 화면에서 `python3.9` 로 Python 최신버전 실행이 가능하다. 

하지만 아직은 완전하지 않다. pip 이 없어서 NumPy 같은 외부 모듈 설치가 되지 않는다. 이제 pip 을 설치한다.

```bash
sudo apt install python3.9-distutils
curl -sSL https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3.9 get-pip.py
```
{:.bash}

pip 이 설치되었으므로, NumPy 를 설치해본다.

```bash
python3.9 -m pip install numpy
```
{:.bash}

제대로 NumPy 가 설치되었는지 아래 명령어로 확인해본다.

```bash
python3.9 -m pip list
```
{:.bash}