---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 1 : Beautiful Soup의 개념과 사용법"
date: 2018-06-09
categories:
  - crawl
description: Beautiful Soup로 크롤링하는 방법을 알아봅니다.
image: https://pbs.twimg.com/profile_images/1249885576/logo.jpg
image-sm: https://pbs.twimg.com/profile_images/1249885576/logo.jpg
---
## Beautiful Soup 로 크롤링 하기
<sup style="color: #878787;">
    [Selenium](/2018/06/12/python-crawling-selenium-1/) 은 3장에서 다룹니다.
</sup>
#### 1. Beautiful Soup 라이브러리 설치하기
[Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)는 HTML과 XML 파일로부터 데이터를 가져오기 위한 라이브러리 입니다.
Beautiful Soup를 설치하기 위해 아래 명령어를 입력합니다.
{% highlight sh %}pip install bs4{% endhighlight %}

#### 2. Beautiful Soup 이해하기
파이썬 파일을 하나 만들어줍니다.

{% highlight sh %}touch crawler1.py{% endhighlight %}

그리고 파일 안에 아래 코드를 입력합니다.
{% highlight python %}
from bs4 import BeautifulSoup
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
soup = BeautifulSoup(html_doc, 'html.parser')

print(soup.prettify())
{% endhighlight %}
<sup style="color: #878787;">
 1: bs4 라이브러리에서 Beautiful Soup를 import 시킴<br>
 2: Beautiful Soup가 잘 작동하는지 확인하기 위해 Example Code 를 생성<br>
 19: html_doc를 html로 끌어옴<br>
 21: 끌어온 데이터를 깔끔하게 정리하여 출력
</sup>
{% highlight bash %} python crawler1.py {% endhighlight %}
위 코드를 입력하여, `crawler1.py` 를 실행시켜 봅시다.

print한 결과를 확인해보면 html_doc의 내용이 깔끔하게 정리되어 출력됩니다.
한번 변형을 해서 html_doc를 크롤링 해봅시다.
{% highlight python %} print(soup.title) {% endhighlight%}
17번째 줄에 있는 코드를 이렇게 변형하면 어떻게 나올까요?<br>
``` <title>The Dormouse's story</title> ```부분이 출력됩니다. html_doc 의 title 부분을 크롤링 했습니다.

그 외에도 여러가지 기능이 있습니다. 아래에 간단하게 정리해봤습니다.
위쪽은 코드, 아래쪽은 결과값입니다.
<center>
<hr>
{% highlight python %} print(soup.title.string) {% endhighlight%}
<i class="fa fa-arrow-down fa-1x"></i>
 {% highlight html %} The Dormouse's story {%endhighlight%}
<hr>
{% highlight python %} print(soup.title.parent.name) {% endhighlight%}
<i class="fa fa-arrow-down fa-1x"></i>
 {% highlight html %} head {%endhighlight%}
<hr>
{% highlight python %} print(soup.p) {% endhighlight%}
<i class="fa fa-arrow-down fa-1x"></i>
 {% highlight html %}<p class="title"><b>The Dormouse's story</b></p>{% endhighlight %}
<hr>
{% highlight python %} print(soup.find_all('a')) {%endhighlight%}
<i class="fa fa-arrow-down fa-1x"></i> 
{% highlight html %}[<a class="sister" href="http://example.com/elsie"id="link1">Elsie</a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]{% endhighlight %}
<hr>
{% highlight python %} print(soup.get_text()) {%endhighlight%}
<i class="fa fa-arrow-down fa-1x"></i><br>
The Dormouse's story

The Dormouse's story
Once upon a time there were three little sisters; and their names were
Elsie,
Lacie and
Tillie;
and they lived at the bottom of a well.
...
<hr>
<br>
</center>
 다음 장에서는, [Naver 실시간 검색어 크롤링](/2018/06/10/python-crawling-2/)을 해보겠습니다.
