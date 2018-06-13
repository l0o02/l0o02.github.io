---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 4 : Selenium을 이용해 페이스북에 로그인"
date: 2018-06-12
categories:
  - crawl
description: Selenium으로 페이스북에 로그인하는 방법을 알아봅니다.
image: https://i.pinimg.com/originals/64/b7/de/64b7deb1bfa083d44f5e5b6a5b0f1859.jpg
image-sm: https://i.pinimg.com/originals/64/b7/de/64b7deb1bfa083d44f5e5b6a5b0f1859.jpg
---
## Selenium으로 Facebook 로그인하기
<sup style="color: #878787;">
    [Beautiful Soup](/2018/06/09/python-crawling-1/) 은 1장과 2장에서 다룹니다.
</sup>
#### 1. [Facebook](https://www.facebook.com/) 의 HTML 분석하기
[파이썬 크롤링 튜토리얼 - 3](/2018/06/12/python-crawling-selenium-1/)의 Selenium으로 검색하기에서 봤듯이, input에 값을 입력하려면 name이나 id같은 `선택자`가 필요합니다. 개발자 모드에서 찾아보도록 합시다. 
![image](https://user-images.githubusercontent.com/39974109/41305034-aaec211a-6eac-11e8-9f94-3bf30c9e0eb9.png)
아이디를 입력해야 하는 곳은 `email`이라는 id를 사용 중입니다.
![image](https://user-images.githubusercontent.com/39974109/41305056-b84dd8b2-6eac-11e8-9098-c73e756f3e5a.png)
패스워드를 입력해야 하는 곳은 `pass`라는 id를 사용 중입니다.
#### 2. Python 코드 작성하기

{% highlight python %}
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

usr = "아이디"
pwd = "패스워드"

path = "WebDriver의 경로"
driver = webdriver.Chrome(path)
driver.get("http://www.facebook.org")
assert "Facebook" in driver.title
elem = driver.find_element_by_id("email")
elem.send_keys(usr)
elem = driver.find_element_by_id("pass")
elem.send_keys(pwd)
elem.send_keys(Keys.RETURN)
{% endhighlight %}
usr과 pwd에는 본인의 페이스북 아이디와 비밀번호를 각각 입력해주면 됩니다.<br>
driver.get 까지는 [파이썬 크롤링 튜토리얼 - 3](/2018/06/12/python-crawling-selenium-1/)에서 다뤘던 내용이라 생략하겠습니다.<br><br>
`assert "Facebook" in driver.title` : driver.title이 Facebook 이 아니면 예외처리를 하여 Error을 내줘! 라는 의미입니다. Facebook 에 접근한건지 아닌지를 판단하기 위해 입력한 코드입니다. <br>
`elem = driver.find_element_by_id("email")` : 위에서 분석한 아이디 입력란의 id를 찾아서 커서를 두겠다는 의미입니다.<br>
`elem.send_keys(usr)` : usr에 입력한 페이스북 아이디 값을 현재 커서가 위치한 곳에 넣겠다는 뜻입니다.<br>
`elem = driver.find_element_by_id("pass")` : 위에서 분석한 패스워드 입력란의 id를 찾아서 커서를 두겠다는 의미입니다.<br>
`elem.send_keys(pwd)` : pwd에 입력한 페이스북 패스워드 값을 현재 커서가 위치한 곳에 넣겠다는 뜻입니다.<br>
`elem.send_keys(Keys.RETURN)` : Enter키를 누르게 합니다.<br>

그리고 py파일을 실행하게 되면, 페이스북에 알아서 로그인 하는 게 보입니다.

응용하여 Logout 기능까지도 만들어 보면, 이 포스트를 제대로 이해했는지 확인할 수 있을 것 같습니다.