---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 5 : Beautiful Soup와 Selenium을 함께 사용하는 방법"
date: 2018-06-13
categories:
  - crawl
description: 페이스북에 마이페이지에있는 정보를 크롤링할겁니다.
image: http://www.fanghuomen.org/data/out/171/726453.jpg
image-sm: http://www.fanghuomen.org/data/out/171/726453.jpg
---
## Selenium으로 진입한 웹 사이트 크롤링하기

#### 1. Facebook Profile로 접속할 준비하기
[파이썬 크롤링 튜토리얼 - 4](/2018/06/12/python-crawling-selenium-2/)에서 페이스북에 로그인 하는 방법을 알아봤었습니다.
튜토리얼 - 4 에서 완성한 코드를 재검토해보고 시작하겠습니다. 
{% highlight python linenos%}
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
usr = "아이디"
pwd = "패스워드"

path = "/Users/hjvsdh/crawl/chromedriver"
driver = webdriver.Chrome(path)
driver.get("http://www.facebook.org")
assert "Facebook" in driver.title
elem = driver.find_element_by_id("email")
elem.send_keys(usr)
elem = driver.find_element_by_id("pass")
elem.send_keys(pwd)
elem.send_keys(Keys.RETURN)
{% endhighlight %}
오늘은 이 코드를 응용해서, 내 타임라인에 있는 글을 몇개 긁어와보려고 합니다. 우리가 사용하는 driver 가 profile에 접속할 수 있도록 profile 링크(href)를 찾아줘야하는데, 이전에 사용했던 선택자말고 `XPath`를 사용해볼겁니다.

프로필태그의 href값을 찾기 위해 아래 사진처럼 개발자모드에 들어가서 이 부분을 선택해줍니다.
![image](https://user-images.githubusercontent.com/39974109/41325353-b99e94b6-6ef4-11e8-93ca-59d9ecca5e7c.png)
이 부분을 우클릭하고 `Copy -> Copy XPath`를 해줍니다.

그리고 4장에서 완성했던 코드 제일 끝에, 아래 코드를 입력합니다.
{% highlight python linenos %}
a = driver.find_elements_by_xpath('이곳에 Copy했던 XPath를 붙여넣습니다.')
driver.get(a[0].get_attribute('href'))
{% endhighlight %}
driver.find_elements_by_xpath XPath로 해당 elements 를 가져오는 겁니다. 기본적으로 a = find_elements_by_xpath 를 하게되면 a 는 list 상태가 되므로, a[0]을 한 뒤, get_attribute('href')를 하는 겁니다. 그러면 elements 안에 있는 href(프로필 주소) 속성값으로 Web Driver 가 접속하게 되는겁니다. 

이제 내 타임라인에 접근하는 것은 성공했습니다.

> 만약 XPath를 찾을 수 없다는 에러가 나오면, time 을 import 하여 Facebook 페이지가 원활히 로딩이 끝날때 까지 time.sleep(5)로 5초정도 기다려주면 해결됩니다. `import time`을 하고, elem.send_keys(Keys.RETURN)아래에 time.sleep(5)를 씁니다. UI가 로딩될 때 까지 기다리는 방법도 있습니다. [참고 문서](http://selenium-python.readthedocs.io/waits.html)

#### 2. Facebook Profile에 있는 데이터 크롤링하기
페이스북 타임라인 포스트 크롤링같은 경우에는 [Facebook Graph API](https://developers.facebook.com/docs/graph-api/?locale=ko_KR)를 사용하는게 훨씬 간결하고 편합니다. 하지만 우리가 크롤링하려는 부분은 바로 이 부분입니다.
![image](https://user-images.githubusercontent.com/39974109/41327261-bc0cfefa-6efd-11e8-9abd-a40bff4114f0.png)
좌측에 위치한 소개 부분을 크롤링 할 겁니다. Web Driver 가 현재 실행중인 웹 사이트의 소스를 가져오려면 아래 소스를 입력해야 합니다.
{% highlight python %}
req = driver.page_source
{% endhighlight %}
이렇게 req에 소스를 저장했으면 이 req가 HTML parser를 사용해야한다고 알려줘야합니다. [참고 : 파이썬 크롤링 튜토리얼 - 1](/2018/06/09/python-crawling-1/). 그 전에 맨 윗쪽에 `from bs4 import BeautifulSoup` 를 해줘야겠죠?
{% highlight python %}
soup=BeautifulSoup(req, 'html.parser')
{% endhighlight %}
그리고 선택자를 찾아내야 합니다. 
![image](https://user-images.githubusercontent.com/39974109/41327433-6a5ebffc-6efe-11e8-93bc-c3c7b554957e.png)
`Copy -> Copy Selector` 을 하게 되면 div의 id가 하나 나옵니다. 그럼 튜토리얼 - 1 에서 처럼 출력이 되는지 확인하기 위해 코드를 입력해봅시다.
{% highlight python %}
information_list = soup.select("#intro_container_id")
for information in information_list:
	print(information.text)
{% endhighlight %}
![image](https://user-images.githubusercontent.com/39974109/41327533-d64ed58a-6efe-11e8-8688-5febd0b61725.png)
크롤링이 된게 확인이 됩니다! 


최종 소스
{% highlight python linenos %}
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup
import time
usr = "아이디"
pwd = "패스워드"

path = "/Users/hjvsdh/crawl/chromedriver"
driver = webdriver.Chrome(path)
driver.get("http://www.facebook.org")
assert "Facebook" in driver.title
elem = driver.find_element_by_id("email")
elem.send_keys(usr)
elem = driver.find_element_by_id("pass")
elem.send_keys(pwd)
elem.send_keys(Keys.RETURN)

a = driver.find_elements_by_xpath('//*[@id="u_0_a"]/div[1]/div[1]/div/a')
driver.get(a[0].get_attribute('href'))

req = driver.page_source
soup=BeautifulSoup(req, 'html.parser')
information_list = soup.select("#intro_container_id")
for information in information_list:
	print(information.text)
{% endhighlight %}
