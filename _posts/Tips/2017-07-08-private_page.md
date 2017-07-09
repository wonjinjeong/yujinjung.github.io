---
layout: post
title: Github private page 
description: 
modified: 2017-07-09
tags: [Tips]
image:
  feature: background/Computer/3.jpg
  credit: unsplash
---

##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

---

# Github page에 private page 만들기 
불가능하다.  
<div markdown="0"><a href="#" class="btn btn-warning">Caution: This repository is private but the published site will be public.</a></div>
Setting page에 적힌 것처럼 저장소가 private여도 site는 public이고 찾아본 결과 private page를 따로 만들 수는 없었다.  

<br/>

---

# 대안
그래서 조금 불편하지만 사용할 수 있는 방법은 두가지 정도가 있다.  
하지만 저장소에서 markdown을 확인하는 방법이라서 private page를 사용하는 것과는 거리가 많이 멀다.  

<br/>

## 저장소 private / published 이용

##### [다음 [LINK] 를 참조하였습니다.](https://stackoverflow.com/questions/14428029/how-to-make-my-post-not-public-for-a-while)

위에서 말한 것처럼 저장소만 private로 돌릴 경우 말그대로 저장소에만 접근이 불가능할 뿐 웹페이지에는 어떠한 제약없이 접근이 가능하기 때문에 published를 이용한 작업을 진행해야한다.  

published 는 해당 markdown 을 발행할 것인지 알려준다.  

```markdown
---
layout: post
title: Github private page 
description: 
published: false
modified: 2017-07-09
tags: [Tips]
image:
  feature: background/Computer/3.jpg
  credit: unsplash
---
```

발행하지 않고자 하는 post는 위에 적힌 것처럼 desciption 밑에 추가된 published를 false로 설정하면 웹페이지에 노출시키지 않는다.  
그리고 확인하고자 할 때에는 아래처럼 저장소에서 markdown file을 확인하면 어떨까싶다. 번거로운가..  
(publish 하는 page를 만들어서 해당 주제에 대한 link만 전달하게 해도 좋을 것 같다.)  

<figure>
  <a href="/images/Tips/private_page/private.png"><img src="/images/Tips/private_page/private.png" alt=""></a>
  <figcaption><a href="https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/Tips/2017-07-08-private_page.md" title="markdown">[Link] markdown</a></figcaption>
</figure>


1. 저장소를 private로 설정
2. private page로 만들고 싶은 page를 published : false 설정
3. 해당 markdown 을 link로 남기는 public page 작성

<br/>

## private 저장소 / public 저장소 분리 사용

##### [다음 [LINK] 를 참조하였습니다.](https://stackoverflow.com/questions/7983204/having-a-private-branch-of-a-public-repo-on-github)

위의 방식을 사용할 경우 저장소 전체가 private 설정되어 숨겨지기 때문에 public 저장소는 노출시키고 싶을 경우에 해당 방법을 사용하면 좋을 것 같다.  

1. 사용하고 있는 저장소(public)를 복제하여 private 저장소로 만든다.  
2. public page에 remote 를 추가하여 public page를 업로드 할 때는 public 저장소와 private 저장소 두 곳에 동시에 업로드한다.  
3. private page를 사용하고자 할 때에는 private 저장소에서 pull하여 동기화하고 markdown을 작성하여 private 저장소에만 업로드하고 public 저장소에는 link를 첨부하는 page를 작성한다.  

### remote 를 추가하는 방법 (public page에서 작성)
```
git remote add <저장소별명> 저장소주소
git remote -v 로 확인

> git remote add private https://github.com/UjinJung/ujinjung.github.io_private.git
> git remote -v
origin  https://github.com/UjinJung/ujinjung.github.io.git (fetch)
origin  https://github.com/UjinJung/ujinjung.github.io.git (push)
private https://github.com/UjinJung/ujinjung.github.io_private.git (fetch)
private https://github.com/UjinJung/ujinjung.github.io_private.git (push)

// 두 곳에 push 할 때 이런식으로 진행한다.
> git push origin master  // public repository
> git push private master // private repository
```


