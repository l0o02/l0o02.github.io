---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 3 : Selenium 사용법과 이해"
date: 2018-06-12
categories:
  - crawl
description: Selenium의 사용법과 이해를 합니다.
image: https://i2.wp.com/blog.fossasia.org/wp-content/uploads/2017/11/Cross-browser-testing-in-Selenium-Webdriver.png?fit=525%2C333&ssl=1
image-sm: https://i2.wp.com/blog.fossasia.org/wp-content/uploads/2017/11/Cross-browser-testing-in-Selenium-Webdriver.png?fit=525%2C333&ssl=1
---
## Selenium으로 크롤링 하기
<sup style="color: #878787;">
    [Beautiful Soup](/2018/06/09/python-crawling-1/) 은 1장과 2장에서 다룹니다.
</sup>
#### 1. [Selenium](https://www.seleniumhq.org/) 에 대해서
Selenium은 웹 애플리케이션을 위한 `테스팅 프레임워크`입니다. 자동화 테스트를 위해 여러 가지 기능을 지원합니다. 다양한 언어에서도 사용이 가능합니다. Beautiful Soap는 웹사이트에서 버튼을 클릭해야 얻을 수 있는 데이터라던가, Javascript 에 조건이 충족되어야만 얻을 수 있는 데이터에 접근하는 것에 한계가 있습니다. 그래서, 직접적으로 웹 사이트에 접근할 수 있게 해주는 Selenium을 사용해야 합니다. 새로운 환경에서 웹 브라우저를 대신해 줄 [`Web Driver`](http://chromedriver.chromium.org/downloads)가 필요합니다. Web Driver를 눌러 설치를 합시다. Web Driver는 Selenium이 사용할 웹 브라우저이고, Selenium으로 자동화하여 웹 사이트를 탐험하면 됩니다.

#### 2. Selenium 이해하기
pip 명령어를 사용해 Selenium 을 설치해줍니다.
{% highlight bash %} pip install selenium {% endhighlight %}
Python파일을 하나 만들고 아래 코드를 실행해봅시다.
{% highlight python linenos %}
from selenium import webdriver
 
path = "Webdriver의 경로를 입력합니다."
driver = webdriver.Chrome(path)
{% endhighlight %}
파일을 실행해보면 어떻게 될까요?
![image](https://user-images.githubusercontent.com/39974109/41285921-b8616c44-6e78-11e8-95b4-83150949de2a.png){: width="100%"}
크롬 창이 켜졌습니다! Selenium 으로 제어하기 때문에, 크롬창에 자동화된 테스트 소프트웨어로 제어중이라는겁니다.
{% highlight python %}driver.get('https://www.naver.com'){% endhighlight %}
맨 마지막줄에 입력하고, 다시 실행해봅니다. 네이버로 접속되는게 보일겁니다.

#### 3. Selenium 으로 검색하기
이제 셀레니움으로 크롬을 꺼냈으니 무라도 썰어야합니다. 자, 아래 코드를 입력해봅시다.
{% highlight python linenos%}
from selenium import webdriver

path = "Webdriver 경로를 입력합니다."
driver = webdriver.Chrome(path)
driver.get("http://google.com/")
search_box = driver.find_element_by_name("q")
search_box.send_keys("개발새발 블로그")
search_box.submit()
{% endhighlight %}
실행해보면 구글에 개발새발 블로그가 검색됩니다. 어떤식으로 진행이 되는지 하나하나 알아볼까요?
driver.get()까지는 아까 얘기했으니 그 다음줄부터 알아보겠습니다.

![image](https://user-images.githubusercontent.com/39974109/41304474-38b9ec2c-6eab-11e8-888d-6ccfaf6a3c77.png)
<center>
{% highlight python %}
search_box = driver.find_element_by_name("q")
{% endhighlight %}
위 개발자 도구를 확인해보면 우리가 사용할 input의 name이 q 인것을 확인할 수 있습니다. search_box가 커서를 어디다 둬야할 지 name으로 찾아준겁니다.
</center>

<center>
{% highlight python %}
search_box.send_keys("개발새발 블로그")
search_box.submit()
{% endhighlight %}
아까 찾은 검색 input 에 개발새발 블로그를 입력하고 submit()으로 검색 버튼을 누른겁니다.

<br>
<br>
간단하게 Selenium에 대해 알아봤습니다. 다음 장에서는, Facebook Login을 구현해보도록 하겠습니다.
</center>