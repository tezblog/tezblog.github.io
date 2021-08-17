---
layout: post
title: "깃허브 페이지 하이라이터, Rouge 대신 Highlight.js 사용"
updated: 2021-03-02
tags: [dev,blog]
---

## 하이라이터 변경

깃허브 페이지의 기본 정적 웹페이지 제너레이터인 [Jekyll](https://pages.github.com/) 의 기본 코드 하이라이터는 [Rouge](http://rouge.jneen.net/) 이다. 별 불편없이 사용하고 있었으나, Excel 관련 자료를 포스팅하려고 보니 Rouge 는 Excel 문법을 지원하지 않는다는 것을 알았다.

그래서 Excel 하이라이팅을 지원하는 [Highlight.js](https://highlightjs.org/) 로 바꾸기로 마음 먹었다.

## Highlight.js 설치

먼저 Rouge 의 작동이 안되도록 설정해야 한다. 구글링해서 찾은 아래 코드를 "_config.yml" 파일에 추가로 삽입했다.

```yaml
markdown: kramdown
kramdown:
    syntax_highlighter_opts:
        disable: true
```
{:.yaml}

다음은 Highlight.js 를 사용하기 위한 아래 코드를 삽입한다. 본 블로그의 포스팅 템플릿을 담당하는 html 파일에 아래 코드를 삽입했다.

```html
<head>
    <!-- 생략 -->
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.17.1/styles/github.min.css"/>
</head>
<body>
    <!-- 생략 -->
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.17.1/highlight.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.17.1/languages/excel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.17.1/languages/vbnet.min.js"></script>
    <script>
        hljs.initHighlightingOnLoad();
    </script>
</body>
```
{:.html}

참고로, Highlight.js 의 **현재 최신버전은 10 이지만, 이 버전부터는 IE11 을 지원하지 않는다.** 아직은 우리나라 IE11 비중이 다소 높은 편이기에 낮은 버전을 적용하였다.

위 코드를 보면 알겠지만, Excel 과 VBA 하이라이팅을 지원하기 위해 따로 js 랭귀지팩 파일을 추가하였다.

## 사용법

Highlight.js 를 사용한다고 해서 마크다운 작성을 특별히 달리해야하거나 하는 건 없다. 기존에 하던대로 작성하면 되며, 브라우저에 띄울 때, 단지 하이라이팅을 Highlight.js 가 해줄 뿐이다.