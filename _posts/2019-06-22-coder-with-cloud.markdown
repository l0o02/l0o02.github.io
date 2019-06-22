---
layout: post
comments: true
title: "싸지방에서 개발 환경 구축하기"
date: 2019-06-01
categories:
  - life
description: VSCODE 를 다운받지 않고, 구글 클라우드와 CODER를 사용하여 환경을 구축하는 방법.
image: https://cdn0.tnwcdn.com/wp-content/blogs.dir/1/files/2017/01/oqtafyt5ktw-ilya-pavlov-796x532.jpg
image-sm: https://cdn0.tnwcdn.com/wp-content/blogs.dir/1/files/2017/01/oqtafyt5ktw-ilya-pavlov-796x532.jpg
---
## 사이버 지식 정보방에서 환경 구축하기
개발을 하고싶어하는 군인들을 위해서, 내가 사용하는 환경과 그 구축 방법을 알려주려고 한다. 아래 사진은 완성된 작업 환경이다.
![image](https://user-images.githubusercontent.com/39974109/59957305-e904ea80-94d1-11e9-8346-8f331920f6a2.png)

VSCODE를 설치하지도 않았고, 100% 웹 사이트에서 실행 가능하다. VSCODE 에서 사용하는 여러가지 Package들을 사용할 수 있다. 심지어 Theme도 설치가 가능하다. 사회에서와 같은 환경으로 개발이 가능하다는 점이 개발을 좋아하는 군인들에게있어 꽤 매력적일 것이라 생각한다. 

## Google Cloud Platform
![image](https://user-images.githubusercontent.com/39974109/59957364-9841c180-94d2-11e9-9d27-79eecd5dacf7.png)
우선적으로 구글 클라우드 플랫폼을 가입해야한다. [이동하기](https://console.cloud.google.com/), 구글 클라우드는 약 1년정도 무료로 사용 가능하며, 300$ 정도의 크레딧을 제공한다. 현역 병사들에게 1년정도의 무료 사용 기간은 어느정도 충분하다고 생각하지만 부족하다면은 결제를 고려해보는 것도 좋다. 그리고 다른 서버를 가지고 있다면, 이 과정은 생략해도 좋다.
#### VM 인스턴스 생성하기
