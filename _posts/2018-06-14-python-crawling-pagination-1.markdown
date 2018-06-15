---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 6 : Pagination 된 게시판 크롤링"
date: 2018-06-14
categories:
  - crawl
description: 웹 사이트에 페이지네이션 된 게시판 글을 크롤링하는 방법
image: https://cdn.dribbble.com/users/141880/screenshots/2513164/dailyui-085.gif
image-sm: https://cdn.dribbble.com/users/141880/screenshots/2513164/dailyui-085.gif
---
## Pagination 된 글 크롤링 하기
> Pagination 이란, 여러 페이지에 일련의 관련 콘텐츠가 있음을 나타내는 페이지 번호 매김을 보여주는 것 입니다.
> 페이지네이션 된 게시판에는 URL에 특정 규칙이 있습니다. page=1, number=1 등 페이지를 넘어갈 때 마다 바뀌는 숫자를 파악해야 합니다.

#### Naver 뉴스 페이지 URL 분석하기
![image](https://user-images.githubusercontent.com/39974109/41453346-5b646e9e-70b0-11e8-82b2-8c1f28041668.png)
[네이버 부동산 뉴스 페이지](http://land.naver.com/news/field.nhn?page=1)를 크롤링 하려고 합니다. 접속하여 URL 을 확인해보면 맨 뒤에 `page=숫자`가 보입니다.
이는, Pagination 으로 현재 몇 번째 페이지를 보여주는지 알려줍니다.
<p align="center"><img src ="https://user-images.githubusercontent.com/39974109/41453443-c0b7b422-70b0-11e8-823c-5450befa4619.png"></p>

첫번째 페이지라서 http://land.naver.com/news/field.nhn?page=1 인겁니다. 우리는 총 세개의 페이지가 있다는 것을 압니다. 하지만 파이썬은 알지 못하므로, 이 사이트가 총 몇번째 페이지까지 Pagination 되어있는지 코드로 알려줘야 합니다. 
![image](https://user-images.githubusercontent.com/39974109/41454046-f9c525fe-70b2-11e8-8afe-a6dbd680a791.png)
첫번째 페이지는 `NP=r:1`, 두번째 페이지는 `NP=r:2` 로 규칙적인 Class 를 가지고 있습니다. 그러면 이를 통해 어떻게 Pagination 의 최대값을 파악하는지 알아보도록 합시다.

#### Python 코드 작성하기
{% highlight python %}
from bs4 import BeautifulSoup
import requests

maximum = 0
page = 1

URL = 'http://land.naver.com/news/field.nhn?page=1'
response = requests.get(URL)
source = response.text
soup = BeautifulSoup(source, 'html.parser')
{% endhighlight %}
우리가 크롤링하려는 페이지가 여러개이므로 동적인 URL 을 입력해줘야 합니다. 그렇기 때문에 만들어준 변수를 두개 만들어줬습니다.
<br>`maximum`은 pagination 의 최대 값입니다. 총 세개가 있다는 것을 우리는 코드를 통해 알려줄 예정입니다.
<br>`page`는 현재 지목하고 있는 page 의 pagination 값입니다. Naver 뉴스 페이지로 설명하자면 http://land...page=`1`, http://land...page=`3` 같은 유동적인 값 입니다.

아래는 페이지네이션이 몇번째 까지 있는지 확인하기 위한 소스 입니다.
{% highlight python linenos%}
while 1:
	page_list = soup.findAll("a", {"class": "NP=r:" + str(page)})
	if not page_list:
		maximum = page - 1
		break
	page = page + 1
print("총 " + str(maximum) + " 개의 페이지가 확인 됬습니다.")
{% endhighlight %}
<sup style="color: #878787;"><br>
1: while 에 진입하기 위해서 `TRUE` 인 `1`을 입력해줬습니다.<br>
2: page_list 에 `NP=r: + str(page)` 클래스를 가진 a를 찾아넣습니다. str(page)는 page가 정수형이기 때문에 String 으로 사용하기 위함입니다.<br>
3: page_list 에 값이 없을 때. 즉, `NP=r:숫자` 가 없는 클래스일때 실행하는 코드입니다. 여기서 `NP=r:숫자` 가 없다는 것은 숫자가 3을 초과했다는 의미가 되겠습니다. 저희가 크롤링하려는 페이지는 총 3개의 페이지네이션을 가지고 있기 때문입니다.<br>
4: maximum 은 페이지네이션의 최대 값 즉 3이 되야 합니다. page가 한번 더해진 상태로 들어왔기 때문에 1을 빼준겁니다.<br>
5: maximum 값을 찾았으므로 while 문에서 탈출합니다.<br>
6: `NP=r:1`클래스가 Naver 뉴스 안에 있음을 확인했으므로 다음 클래스인 `NP=r:2`를 확인하기 위해 작성했습니다. 이 코드가 while 문의 마지막이므로 다시 while의 첫번째 코드로 진행합니다. <br>
7: maximum 가 원하는 값으로 됬는지 확인합니다.
</sup>
![image](https://user-images.githubusercontent.com/39974109/41454046-f9c525fe-70b2-11e8-8afe-a6dbd680a791.png)
네이버 부동산 뉴스 페이지는 총 3개의 페이지를 가지고 있습니다.
![image](https://user-images.githubusercontent.com/39974109/41454996-6e8c8ca8-70b6-11e8-8794-5cde8778e547.png)
maximum 변수의 값도 3 인게 확인이 됬습니다.

그러면 크롤링할 준비가 됬습니다.
{% highlight python linenos %}
whole_source = ""
for page_number in range(1, maximum+1):
	URL = 'http://land.naver.com/news/field.nhn?page=' + str(page_number)
	response = requests.get(URL)
	whole_source = whole_source + response.text
soup = BeautifulSoup(whole_source, 'html.parser')
find_title = soup.select("#content > div.section_headline > ul > li > dl > dt > a")

for title in find_title:
	print(title.text)
{% endhighlight %}
<sup style="color: #878787;"><br>
1: whole_source 는 크롤링할 모든 페이지의 HTML 소스를 전부 저장할 변수입니다. 즉, http://land....page=1 부터 http://land....page=3 의 HTML 소스를 저장합니다.<br>
2: range 함수는 숫자리스트를 만들어줍니다. range(1, maximum+1) 을 함으로써 1부터 maximum 까지의 숫자 리스트가 생성됩니다. 그리고 page_number 은 1부터 더해지면서 maximum 이 될때까지 for 문을 실행하게 됩니다. maximum+1로 해준 이유는 range 는 첫번째 인자 값 1부터, 두번째 인자 값 `미만`까지 실행되기 때문입니다.<br>
5: HTML 소스를 whole_source 에 전부 넣습니다. 그러면 whole_source는 최종적으로 3개의 HTML 소스를 더한게 됩니다.<br>
7: 뉴스의 제목 선택자를 찾아서 soup.select 했습니다. find_title 은 모든 뉴스의 제목을 가진 list가 됬습니다.<br>
9: for 문으로 뉴스 제목들을 출력합니다.
</sup>

![image](https://user-images.githubusercontent.com/39974109/41455768-450d2060-70b9-11e8-8f56-7a06d05a5fc6.png)
첫번째 페이지의 첫번째 뉴스 제목은 '송파구 매매 ... ' 입니다. 

![image](https://user-images.githubusercontent.com/39974109/41455772-480b7a3c-70b9-11e8-8f2d-afae65d94e1b.png)
세번째 페이지의 마지막 뉴스 제목은 '입주 시작했는데 ...' 입니다.

그럼 파이썬 파일을 실행해서 비교해 보겠습니다.
<center>
<script src="https://asciinema.org/a/JfjWlT86TRHXdeATGmAGmeO0h.js" id="asciicast-JfjWlT86TRHXdeATGmAGmeO0h" async></script>
</center>
첫번째 크롤링 된 뉴스 제목과 세번째 페이지에있는 마지막 뉴스 제목까지 전부 크롤링 된게 확인됩니다.

완성했습니다. 다른 게시판들도 비슷한 구조로 크롤링이 가능합니다. 그리고 몇몇 사이트들은 Selenium을 사용해야 됩니다. 

코드에 오류가 있거나 질문이 있다면 댓글로 답변해드리도록 하겠습니다.
