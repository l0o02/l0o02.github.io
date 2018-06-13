---
layout: post
comments: true
title: "jekyll 포스트에 Related Post를 추가하는 방법"
date: 2018-06-14
categories:
  - jekyll
description: 같은 카테고리에 있는 글을 리스팅 하는 방법
image: https://images4.alphacoders.com/629/thumb-1920-62986.jpg
image-sm: https://images4.alphacoders.com/629/thumb-1920-62986.jpg
---
## 같은 카테고리의 글 리스팅 하기
> 파이썬 강의를 따라하던 사람이 피드백을 해서 개발하게 됬습니다. 아직 디자인은 안했습니다.

#### _layouts/post.html 수정하기
![image](https://user-images.githubusercontent.com/39974109/41375138-1de8ae52-6f90-11e8-81b2-9e9cd66589b3.png)
post.html 중간 부분쯤을 보면 `content` 가 보입니다. 이 content 부분이 지금 Jekyll에서 포스트를 쓰는 부분입니다. 위에 소스부분을 이 `content` 위에 넣든 아래에 넣든 선택하면 됩니다. 구현하느라 조금 애좀 먹었습니다. 소스에 대한 질문이 있으면 언제든 댓글 남겨주시면 도와드리도록 하겠습니다. 

#### _sass/_base.scss 에서 디자인 정하기
![image](https://user-images.githubusercontent.com/39974109/41375464-2c5d929e-6f91-11e8-9894-a7aa70797812.png)
저는 이런식으로 CSS 코드를 작성했는데, 아직 미완성이라 깔끔하진 않네요. 마음대로 커스터마이징해서 추가하시면 됩니다.
