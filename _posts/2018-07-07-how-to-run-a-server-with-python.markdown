---
layout: post
comments: true
title: "파이썬만으로 간단하게 HTTP 서버 열기"
date: 2018-07-07
categories:
  - python
description: 파이썬을 이용해 간단하게 웹 서버를 구축하는 방법
image: https://i.pinimg.com/originals/d4/e8/0f/d4e80f42466a860cd1b0810d2323e3ed.jpg
image-sm: https://i.pinimg.com/originals/d4/e8/0f/d4e80f42466a860cd1b0810d2323e3ed.jpg
---
## Python으로 간단한 웹 서버 만들기

Apache를 설치하기 힘든 환경일 때나, 웹 디자인을 공부할 때 용이하다. 사용 방법은 `index.html`가 있는 루트 디렉토리로 가서 아래 명령어를 실행하면 된다.
뒤에 숫자는 포트이다.
<center>
`python 2.X` 버전
{% highlight bash %}
python -m SimpleHTTPServer 8000
{% endhighlight %}
`python 3.X` 버전
{% highlight bash %}
python3 -m http.server 8000
{% endhighlight %}
</center>
![image](https://user-images.githubusercontent.com/39974109/42408605-5ebf682c-8209-11e8-9e37-8b6ff2c63124.png)
