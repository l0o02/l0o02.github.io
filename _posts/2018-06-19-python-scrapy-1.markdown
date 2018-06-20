---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 8 : Scrapy란? 네이버 뉴스 크롤링해서 CSV로 내보내기"
date: 2018-06-19
categories:
  - crawl
description: Scrapy Shell 을 통해 개념을 이해하고, 네이버 뉴스 페이지를 크롤링 하여 CSV파일로 내보내기까지 하겠습니다.
image: https://s3.amazonaws.com/thinkific/courses/course_card_image_000/216/1891512778404.original.jpg?1512778404
image-sm: https://s3.amazonaws.com/thinkific/courses/course_card_image_000/216/1891512778404.original.jpg?1512778404
---
## Scrapy 란?
최근 웹에는 수억개의 웹페이지가 있으며, 대부분의 페이지들은 수많은 정보를 가지고 있습니다. 최근 빅데이터가 대두되면서 이전에 작성되었던 페이지들의 정보를 모아 유의미한 정보를 도출하기 위한 여러가지 방법들이 논의되고 있고, 이를 Scraping(혹은 Crawling)이라고 합니다. `Scrapy`는 Scraping을 도와주기위한 파이썬 기반 라이브러리입니다. Scrapy를 이용하여 필요한 페이지로 접속하여 원하는 형태로 데이터를 가공하여 데이터를 저장할수 있도록 도와줍니다. [(출처)](http://www.incodom.kr/%ED%8C%8C%EC%9D%B4%EC%8D%AC/%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC/Scrapy#h_a103e753e7b14159b61f918a62b1a4c5)

## Scrapy 첫 코드 작성하기
#### 설치하기
터미널에 아래 명령어를 입력해 `Scrapy`를 설치합니다.
{% highlight bash %}
pip install scrapy
{% endhighlight %}
#### Scrapy Shell 사용해보기
Scrapy Shell을 사용함으로써, 프로젝트를 생성하지 않고 간단하게 Scrapy를 체험할 수 있습니다. 아래 명령어를 입력해서 Shell을 실행시킵니다.
{% highlight bash %}
scrapy shell
{% endhighlight %}
[네이버 뉴스 페이지](http://news.naver.com/main/list.nhn?mode=LSD&mid=sec&sid1=001)를 크롤링하려고 합니다. Scrapy 크롤러는 `starting point`를 필요로 합니다. 말 그대로, 크롤링을 시작할 위치를 정하는 겁니다. 
아래 명령어를 통해 Starting Point를 설정합시다.
{% highlight python %}
fetch('http://news.naver.com/main/list.nhn?mode=LSD&mid=sec&sid1=001')
{% endhighlight %}
그럼, [Response Code](https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C)가 출력됩니다.
![image](https://user-images.githubusercontent.com/39974109/41637651-ef34b3a8-748f-11e8-9bcc-e4c4e1fadd78.png)
이제 크롤러가 무엇을 다운로드했는지 확인해봅시다.
{% highlight python %}
view(response)
{% endhighlight %}
이 명령어는 크롤러가 다운로드한 페이지를 기본 브라우저를 통해 실행합니다.
![image](https://user-images.githubusercontent.com/39974109/41637767-863f6f04-7490-11e8-981f-7b06810d10e3.png)
경로를 보면 아시겠지만, 로컬에 저장이 됬습니다. 이제 소스를 확인해볼겁니다.
{% highlight python %}
print(response.text)
{% endhighlight%}
다운로드 된, 웹 사이트의 소스가 출력됩니다.
![image](https://user-images.githubusercontent.com/39974109/41637816-d7a9ecfc-7490-11e8-81a0-929fc60d8fb7.png)
오늘 크롤링 할 것은 세 가지 입니다.
- 제목
- 올린 뉴스 사이트
- 미리보기

#### 제목
그러면 제목을 먼저 크롤링 해보겠습니다. 개발자 도구에 들어가서 제목 부분의 `XPath`를 복사합니다. 두번째 뉴스의 XPath와 맨 아랫줄에있는 뉴스의 XPath도 비교해봅니다.
1. 첫번째 뉴스 : //*[@id="main_content"]/div[2]/ul[1]/li[1]/dl/dt[2]/a
2. 두번째 뉴스 : //*[@id="main_content"]/div[2]/ul[1]/li[2]/dl/dt[2]/a
3. 마지막 뉴스 : //*[@id="main_content"]/div[2]/ul[2]/li[10]/dl/dt[2]/a 

여기서 어떤 정보를 얻을 수 있을까요?<br><br>
첫번째로는, 포스트는 `li[1]~li[10]`로 구성됬다는 것 입니다.<br>
두번째로는, ul로 `두 파트`가 나뉜다는 것 입니다. *아래 사진 참고.*
![image](https://user-images.githubusercontent.com/39974109/41638191-08538ef6-7493-11e8-808e-9f0dc4bc541d.png)

결국 모든 포스트를 list에 저장하기 위해서는 태그[숫자]의 숫자 부분을 삭제해주면 됩니다.
{% highlight python %}
//*[@id="main_content"]/div[2]/ul/li/dl/dt[2]/a/
{% endhighlight %}

XPath를 얻었으니 이제 크롤링을 해봅니다.
{% highlight python%}
response.xpath('//*[@id="main_content"]/div[2]/ul/li/dl/dt[2]/a/text()').extract()
{% endhighlight %}
![image](https://user-images.githubusercontent.com/39974109/41638554-f403c9be-7494-11e8-9911-afc5729e7de0.png)
포스트 제목들이 출력됩니다. \n\t\r 이 지저분해 보이지만, 이 부분은 다음에 Project를 생성하고 CSV를 만들어 내보낼 때 제거하도록 합시다.
#### 올린 뉴스 사이트
올린 뉴스 사이트는 CSS Selector를 통해 크롤링 했습니다.
{% highlight python %}
response.css('.writing::text').extract()
{% endhighlight %}
뉴스 회사들이 출력되는게 보일겁니다.
#### 미리보기
미리보기도 마찬가지로 CSS Selector를 통해 크롤링 했습니다. 
{% highlight python %}
response.css('.lede::text').extract()
{% endhighlight %}
> 계속 CSS Selector인 Class로 크롤링 하는 이유는 .writing이나 .lede같은 의미가 있는 Class들은 대체로 같은 요소에만 쓰입니다. 꼭 그런것만은 아니니 결과를 꼭 확인해 봐야 합니다. 확인해 봤을 때 같은 요소에서만 쓰인다면, 훨씬 간결하고 유연한 코드를 완성할 수 있습니다.

## Spider 작성하기
기본적인 scrapy 프로젝트의 구조는 이렇습니다. 참고하고 프로젝트를 만들어봅시다.
![image](https://user-images.githubusercontent.com/39974109/41639378-76caee9c-7498-11e8-8499-480f2b8c34bc.png)
#### Hello Scrapy World!
아래 명령어를 통해 `scrapy project`를 생성해줍니다.
{% highlight bash %}
scrapy startproject naverscraper
{% endhighlight %}
그리고 프로젝트 안에 있는 `spiders`폴더에 들어갑니다.
<center>
<script src="https://asciinema.org/a/Ix2l2BBFNMq08ajIueRefzaan.js" id="asciicast-Ix2l2BBFNMq08ajIueRefzaan" async></script>
</center>

#### Spider 생성하기
genspider 명령어를 통해서 `newsbot`을 생성합니다.
{% highlight bash %}
scrapy genspider newsbot news.naver.com/main/list.nhn?mode=LSD&mid=sec&sid1=001
{% endhighlight %}
생성된 `newsbot.py` 에 소스를 입력합니다.
{% highlight python linenos %}
import scrapy

class NewsbotSpider(scrapy.Spider):
	name = 'newsbot'
	start_urls = ['http://news.naver.com/main/list.nhn?mode=LSD&mid=sec&sid1=001']

	def parse(self, response):
		titles = response.xpath('//*[@id="main_content"]/div[2]/ul/li/dl/dt[2]/a/text()').extract()
		authors = response.css('.writing::text').extract()
		previews = response.css('.lede::text').extract()

		for item in zip(titles, authors, previews):
			scraped_info = {
				'title' : item[0].strip(),
				'author' : item[1].strip(),
				'preview' : item[2].strip(),
			}
			yield scraped_info
{% endhighlight %}
12번째 줄의 zip 함수는 list들을 하나씩 slice 하는겁니다.
![image](https://user-images.githubusercontent.com/39974109/41640142-a3ddbef2-749b-11e8-9e73-43fc69118fb2.png)

#### Spider 실행하고 결과 확인하기
newsbot을 실행시켜봅니다.
{% highlight bash %}
scrapy crawl newsbot
{% endhighlight %}
![image](https://user-images.githubusercontent.com/39974109/41640256-0ba7dc20-749c-11e8-8b24-8bb077b558f0.png)

## CSV로 내보내기
프로젝트 폴더에있는 `settings.py`에 입력합니다:
{% highlight python %}
FEED_FORMAT = "csv"
FEED_URI = "naver_news.csv"
{% endhighlight %}

그리고 다시 Spider을 실행합니다. 
{% highlight bash %}
scrapy crawl newsbot
{% endhighlight %}
그럼 같은 디렉토리에 뉴스 내용들이 크롤링되어 csv에 저장됩니다.
![image](https://user-images.githubusercontent.com/39974109/41640344-64582b7c-749c-11e8-9510-b57a0356ec58.png)


최종 소스 :<br>
`newsbot.py`
{% highlight python %}
import scrapy

class NewsbotSpider(scrapy.Spider):
	name = 'newsbot'
	start_urls = ['http://news.naver.com/main/list.nhn?mode=LSD&mid=sec&sid1=001']

	def parse(self, response):
		titles = response.xpath('//*[@id="main_content"]/div[2]/ul/li/dl/dt[2]/a/text()').extract()
		authors = response.css('.writing::text').extract()
		previews = response.css('.lede::text').extract()

		for item in zip(titles, authors, previews):
			scraped_info = {
				'title' : item[0].strip(),
				'author' : item[1].strip(),
				'preview' : item[2].strip(),
			}
			yield scraped_info
{% endhighlight %}
수고하셨습니다, 이것으로 scrapy의 기본적인 사용방법에 대해 알아봤습니다.

