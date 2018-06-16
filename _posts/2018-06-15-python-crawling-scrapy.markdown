---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 7 : Scrapy 란? Scrapy VS Beautiful Soup"
date: 2018-06-15
categories:
  - crawl
description: Scrapy 와 Beautiful Soup 의 장단점에 대한 이야기
image: https://hakan.io/assets/img/python-scrapy.jpg
image-sm: https://hakan.io/assets/img/python-scrapy.jpg
---
## Scrapy VS Beautiful Soup
> 이전까지 튜토리얼로 배워왔던 Beautiful Soup 와 생소한 Scrapy 의 장단점을 정리해보려고 합니다.

#### Beautiful Soup 란?
`Beautiful Soup`는 웹 상의 가치있는 정보를 빠르게 크롤링 하기위한 도구입니다. 진입 장벽이 매우 낮고 간결해서, 입문 개발자에게 안성맞춤입니다. 그리고
이 라이브러리는, 스스로 웹 사이트를 크롤링 할 수 없습니다. `urlib2` 와 `requests`로 HTML 소스를 가져와야만 합니다.

#### Scrapy 란?
Scrapy 는 파이썬으로 작성되었으며, `spider`를 작성해서 크롤링을 합니다. Scrapy 에서는 직접 `Beautiful Soup` 나 `lxml`을 사용할 수 있습니다. 하지만 Beautiful Soup 에서는 지원하지 않는 `XPath`를 Scrapy에서는 사용할 수 있습니다. XPath 를 사용함으로써 복잡한 HTML소스를 쉽게 크롤링 할 수 있게 해줍니다.

#### 어떤게 더 좋을까?
`Beautiful Soup`는 오로지 HTML을 파싱하고 데이터를 크롤링하는데에만 쓰입니다. 반면에 `Scrapy`는 HTML을 다운로드하고 데이터에 접근하여 저장합니다. 만약 이 두가지중에 선택해야 한다면, 아래 표를 참고하시는게 도움이 될겁니다.

|Framework| Beautiful Soup | Scrapy |
|--|--|--|
| 진입장벽 | 매우 쉽다. | 난해하다, 하지만 [Scrapy 문서](https://doc.scrapy.org/en/1.5/intro/tutorial.html)를 참고해서 따라해보면 생각보다는 괜찮다.
| 자료량 | 많이 부족한 편이다. | 매우 많은 프로젝트와 플러그인이 존재한다. Stack OverFlow 에도 많은 케이스가 존재한다.
| 확장성 | 확장하기가 쉽지는 않다. | 쉽게 middleware 를 커스터마이징 할 수 있다. 유지가 쉬운 편이다.
| 퍼포먼스 | `multiprocessing`을 하면 매우 빠르다.| 괜찮은 편이다. 웹 페이지를 짧은 시간내에 크롤링할 수 있다. 잦은 경우로 download_delay 를 설정해줘야 spider가 정지당하는 경우를 피할 수 있다.

가볍고 빠른 Beautiful Soup 와 자료가 방대하고 유연한 Scrapy 를 상황에 맞게 잘 골라 쓰면 되겠습니다.