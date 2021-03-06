---
layout: post
title: "Branching, Stragies"
date: 2017-01-02 20:13:31 +0900
categories: [git]
encoding: UTF-8
---

Git for Teams, 챕터3 내용을 정리한 것


### 1. **Branch**

브랜치란, 다른 브랜치에 영향을 받지 않는 독립적인 작업 영역으로 특정 커밋에 대한 이름이 있는 포인터이다.
각 커밋 객체는 부모에 대한 참조를 갖는다. 

![branch Image](https://raw.githubusercontent.com/sanghak-lee/sanghak-lee.github.io/master/static/img/_posts/Git_for_Teams_Branch.png)



혼자서 진행하거나 규모가 작은 프로젝트의 경우 하나의 브랜치에서 모든 작업이 이루어질 수도 있다. 그러나 대부분의 프로젝트에서는 
프로젝트가 진행되는 브랜치(master branch) 이외에 실험해보고 싶은 것이 있거나 의견이 한가지 사안에 대해 의견이 여러가지일 때 브랜치를 
분기한다. 

분기된 브랜치는 분기된 시점부터 전혀 다른 코드 변경이 이루어질 수 있어 후에 병합하는 과정에서 충돌이 일어날 수 있다.
이런 충돌을 막기 위해 컨벤션을 정하고 언제 분기하고 통합할지 정한다. 

<br/>
<br/>

### 2. **Branch Convention**

컨벤션은 어떤 방식으로 프로젝트를 진행할 것인가에 대한 약속이다. 팀에 적절한 브랜치 전략을 선택하려면
프로젝트의 릴리스 방식을 먼저 정해야한다. 

일반적으로 항상 통합하는 방법과 완료된 작업을 한번에 통합하는 방식 2가지를 혼용하여 사용한다.

<br/>

#### 2.1 **Mainline Branch Development**

메인라인 브랜치 개발은 메인 브랜치에 모든 작업을 커밋하는 방식이다. 참여하는 개발자가 다수일 경우 프로젝트 초기에는 개발자 수만큼 
브랜치가 존재할 수 있으나, 프로젝트가 진행되면서 브랜치 수가 줄어든다. 메인 브랜치에는 배포 가능한 코드만 포함된다는
가정이 전제되어 있다. 이 경우 작업을 일상적으로 통합한 후 가끔 배포하는 방식을 취한다. 

이 방법에서는 저장소가 복잡하지않아 혼란이 생길 여지가 적으며, 웹 사이트와 같이 업데이트가 자동으로 유저에게 적용되는
경우에 적합하다. 

<br/>

#### 2.2 **Branch-Per-Feature Deployment**

메인라인 브랜치 개발의 한계점을 극복하기위해 기능(feature) 브랜치와 통합(integration) 브랜치를 추가한 방법으로
새로운 모든 작업은 기능 브랜치에서 이루어진다. 배포 시점에 기능 브랜치를 병합 여부를 결정하여 통합 브랜치에 병합한 후
합쳐진 기능들이 제대로 실행될 경우 배포한다(마스터 브랜치와 병합). 최근에는 마스터 브랜치에 병합한 이후 배포하는 것이
아니라 통합 브랜치에서 배포한 이후 문제가 없으면 마스터 브랜치에 병합한다.

기능별로 브랜치가 나누어지긴 하지만 브랜치 자체가 기술적으로 다른 브랜치는 아니다.

<br/>

#### 2.3 **State Branching**

상태 브랜칭은 브랜치 네이밍 컨벤션을 통해 브랜치 이름을 어떤 환경에서 어떤 코드가 사용되는지, 어떤 조건에서 사용되어야 하는지
명시한다. 브랜치 이름에 구체적으로 작업이 설명되기 때문에 브랜치를 찾기가 쉬워진다. 상태 브랜칭 전략을 변형하여
Git 프로젝트에서 사용하는 브랜치 네이밍 컨벤션이 존재한다. 이 컨벤션에는 maint, master, next, proposed updates
의 4가지 통합 브랜치를 둔다. 뒤로 갈수록 더 낮은 브랜치이며, 더 많은 양의 커밋을 가지고 있다. 각 브랜치에서 검토 과정이 끝나면
다음 브랜치로 넘어가는 방식이다.


<br/>

#### 2.4 **Scheduled Deployment**

이전까지 나온 브랜칭 전략은 점점 더 복잡해지는 것을 볼 수 있는데, 정기 배포(Scheduled Deployment) 방식에서는
develop이라는 한가지 브랜치가 더 추가된다. 이 develop 브랜치에서 프로젝트에 참여한 개발자들은 기능별 브랜치를 생성하고 기능을 
추가한다. 여기서 쓰이는 기능이라는 용어는 Branch-Per-Feature Deployment에서 나온 기능보다 더 넓은 의미로 사용되는데
일반적으로 쓰이는 기능뿐아니라 리팩토링, 버그 수정 등을 포함한다. 이 방식에서는 더 이상 새로 추가할 기능이 없는 단계,
기능 동결(feature freeze) 상태에 도달했을 때에 새로운 브랜치가 생성되고 이 브랜치에서는 버그 수정만이 커밋된다.
모든 버그를 찾았다고 선언될 경우 master 브랜치에 병합되어 소프트웨어가 출시된다. 크게 보면 개발, 품질보증, 제품 완성 단계를 거쳐 
소프트웨어가 출시된다. 정기적 버전업을 하는 소프트웨어 개발에 적합하다. 

<br/>
<br/>


