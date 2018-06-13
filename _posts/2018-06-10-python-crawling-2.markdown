---
layout: post
comments: true
title: "파이썬 크롤링 튜토리얼 - 2 : Beautiful Soup로 네이버 실시간 검색어 크롤링"
date: 2018-06-10
categories:
  - crawl
description: Beautiful Soup로 실시간 검색어 크롤링 강좌 입니다.
image: https://cdn.instructables.com/FIQ/JABL/GFMBYASM/FIQJABLGFMBYASM.LARGE.jpg
image-sm: https://cdn.instructables.com/FIQ/JABL/GFMBYASM/FIQJABLGFMBYASM.LARGE.jpg
---
## Beautiful Soup 로 네이버 실시간 검색어 크롤링 하기
<br>
#### 1. Requests 설치하기
우리가 만들고자 하는 것은 웹 상에있는 HTML 혹은 여러 소스파일들을 분석하고 가공하여 쓸모있는 데이터로 만드는 것 입니다.
그러기 위해서 우선 인터넷 상에 있는 HTML 파일을 읽어와야 하는데 [`requests`](http://docs.python-requests.org/en/master/) 라이브러리가 도와줄겁니다.
{% highlight sh %}pip install requests{% endhighlight %}
requests 라이브러리를 설치했으면, requests가 어떤 녀석인지부터 차근차근 이해해보도록 합시다.
python 파일을 하나 만들어서 아래 코드를 입력해봅니다.
{% highlight python %}
import requests
URL = 'http://naver.com/'
response = requests.get(URL)
print(response.text)
{% endhighlight %}
결과를 확인하셨나요?, 그러면 이 라이브러리가 어떤 라이브러리인지 바로 이해가 되실겁니다. https://www.naver.com/ 의 HTML소스를 String으로 변환시켜서 변수에 저장시켜주는 역할을 합니다. 그 외에도 쿠키와 헤더를 추가하거나 parameter, data 를 전달할 수도 있습니다.

#### 2. Naver 웹사이트 분석하기
[네이버](https://www.naver.com/) 실시간 검색어 크롤링을 하기 위해선 해당 부분의 HTML을 이해해야 합니다. 개발자모드에 진입해서 크롤링해야할 부분을 잡아줘야합니다.
개발자모드 왼쪽 상단에 위치하는 Select 도구를 클릭하여, 크롤링하길 원하는 부분을 페이지상에서 잡아줘야합니다.
![image](https://user-images.githubusercontent.com/39974109/41200451-a90d4078-6cdf-11e8-8335-b064430a6357.png)
네이버 실시간 검색어 부분을 잘 선택했다면 다음과 비슷한 코드가 선택되어있을겁니다.
![image](https://user-images.githubusercontent.com/39974109/41200514-ba44e70a-6ce0-11e8-9c2c-722837dd35b6.png)
그 코드를 우클릭하여 `Copy -> Copy Selector`를 눌러 [선택자](https://developer.mozilla.org/ko/docs/Web/CSS/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/%EC%84%A4%EB%A0%89%ED%84%B0)를 복사해줍니다.
선택자란 말 그대로 선택해주는 요소입니다. CSS 코드에서 특정 요소를 선택하여 스타일을 적용할 때 사용합니다. 우리는 이 선택자를 이용해 필요한 부분을 `선택`하여 크롤링해야 합니다.
<pre><code>
#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_roll.PM_CL_realtimeKeyword_rolling_base > div > ul > <code class="highlighter-rouge">li:nth-child(19)</code> > a > span.ah_k
</code></pre>
메모장에 붙여 넣어 보면 이런 선택자가 보일 겁니다. 눈치가 빠르신 분들은 `li:nth-child(19)`에서 숫자 19가 순위를 의미한 다는 것을 이미 눈치채셨을 겁니다. 다른 순위들을 클릭해보면 1등은 li:nth-child(1), 2등은 li:nth-child(2), 같이 규칙적으로 변합니다. 만약 이 `li:nth-child(19)`를 그대로 사용한다면, 19위만 출력이 될 겁니다. 19등만 선택한 선택자이기 때문이죠! 그래서 좀 더 포괄적으로 필요한 데이터를 가져오기 위해 `:nth-child()` 부분을 지워줘야합니다.
<pre><code>
#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_roll.PM_CL_realtimeKeyword_rolling_base > div > ul > <code class="highlighter-rouge">li</code> > a > span.ah_k
</code></pre>
자, 이제 선택자부분을 잘 골라냈습니다. 그러면 크롤링을 시작해야겠죠?

#### 3. 네이버 실시간 검색어 순위 크롤링

파이썬 코드를 입력합니다.
{% highlight python linenos %}
import requests
from bs4 import BeautifulSoup

req = requests.get('https://www.naver.com/')
source = req.text
soup = BeautifulSoup(source, 'html.parser')
{% endhighlight %}
<sup style="color: #878787;">
4: req 변수에 https://www.naver.com/ 에 접속하면 받게되는 소스 즉 HTML소스를 저장합니다.<br>
5: req 변수에 저장된 HTML 값을 source로 가져옵니다. text는 소스를 가져오는 것이며 req.status_code를 입력하면 [응답 상태](https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C)를 가져오게 됩니다.<br>
6: BeautifulSoup 에 source 변수가 가지고 있는 값은 HTML라서, HTML.parser을 사용해야 한다고 알려줍니다.
</sup>

자 그럼, 조금전에 추출해낸 선택자를 들고 Beautiful Soup 의 크롤링 기능을 사용해볼 차례입니다!
아래 코드를 입력합니다.
{% highlight python %}
print(soup.select("#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_roll.PM_CL_realtimeKeyword_rolling_base > div > ul > li > a > span.ah_k"))
{% endhighlight %}
실행해보면, 아래처럼 출력이 됩니다.
{% highlight html %}
[<span class="ah_k">영국연방국가</span>, <span class="ah_k">내로라</span>, <span class="ah_k">싱가포르</span>, <span class="ah_k">그린북</span>, <span class="ah_k">추자현</span>, <span class="ah_k">아이스 버킷 챌린지</span>, <span class="ah_k">오재원</span>, <span class="ah_k">싱가포르 대통령</span>, <span class="ah_k">박지성</span>, <span class="ah_k">시나몬</span>, <span class="ah_k">장도연</span>, <span class="ah_k">박지성 딸</span>, <span class="ah_k">루게릭병</span>, <span class="ah_k">아일랜드</span>, <span class="ah_k">김정은</span>, <span class="ah_k">흥부 박씨</span>, <span class="ah_k">싱가포르 샹그릴라 호텔</span>, <span class="ah_k">정글북</span>, <span class="ah_k">김민지</span>, <span class="ah_k">메트로놈</span>] {% endhighlight %}

성공적으로 네이버 실시간 키워드가 출력됩니다.

그런데 사용하기에는 뭔가 지저분하죠? 아무래도 가공을 더 해야할 것 같습니다.

방금 입력했던 print 부분을 지우고, 아래 코드를 입력해봅시다.
{% highlight python %}
top_list = soup.select("#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base >div.ah_roll.PM_CL_realtimeKeyword_rolling_base > div > ul > li > a > span.ah_k")

print(top_list[0].text)
{% endhighlight %}
글자만 깔끔하게 출력되는게 보이실겁니다. top_list[0]에는 `<span class="ah_k">영국연방국가</span>` 가 저장되있습니다.
그 문자열에 `.text`를 사용함으로써 html 코드를 제외한 우리가 원하는 키워드 글자만 가져오는 겁니다.

For 문을 사용하여 전체를 출력하면 될 것 같습니다.

마지막으로, 다시 print 부분을 지우고 아래 For문을 입력합니다.

{% highlight python %}
for top in top_list:
	print(top.text)
{% endhighlight %}

순서대로 출력이 되는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/39974109/41201265-843451dc-6cef-11e8-866f-2a141babdc2b.png){: width="250px"}

완성입니다!

