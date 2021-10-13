---
title : "Kinematics 와 Dynamics 차이점"
category :
    - Research
tag :
    - Robotics
toc : true
toc_sticky: true
comments: true
---

Kinematics와 Dynamics 그리고 Kinetic에 대해서 알아보자!  

로봇공학을 배우다보면 매번 헷갈리는 단어들이 있어 정리를 하고자 한다.  

바로 **Kinematics, Dynamics 그리고 Kinetic**이다.  

## 역학(Mechanics)와 관련된 단어들 간의 관계

역학(Mechanics)을  배울 때 다음 그림과 같이 분류를 한다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/137078488-2e958a28-d2a6-4621-a357-1540618bb56e.png" width = "500" ></p>  

> **역학(Mechanics)은 정역학(Statics)과 동역학(Dynamics)로 나뉘며,  
    동역학(Dynamics)은 운동학(Kinematics)과 운동역학(Kinetics)로 구분할 수 있다.**  

Kinematics와 Dynamics가 매번 헷갈렸는데, Dynamics가 조금 더 포괄적인 개념이였다...!  

그럼 단어 하나 하나의 의미를 조금 더 살펴보자.  

## 단어들의 구체적인 의미  

- **역학(Mechanics)** 은 <u>외부 양의 영향을 받는 물체의 연구와 관련된 물리학의 한 분야</u>라고 정의한다. 이 외부량은 힘, 토크(비틀림력) 또는 운동량일 수 있다.    
<br>

- **정역학(Statics)** 은 평형 상태에 있는 물체를 다루는 역학의 한 분야이며, 평형이라고 할 때 <u>물체가 정지해 있거나 일정한 속도로 움직이는 등속 운동을 의미</u>한다.  
<br>

- **동역학(Dynamics)** 은 <u>힘의 영향으로 인해 가속된 운동을 하는 물체</u>를 다룬다. 정역학과 동역학의 주요 차이점은 정역학에는 가속이 없다는 것이다. 물체의 역학은 그것의 움직임과 움직임 이면의 근본적인 이유를 고려한다.  
<br>

- **운동학(Kinematics)** 은 <u>운동의 원인, 즉 힘을 염두에 두지 않고 점, 물체 또는 물체 시스템의 운동을 연구하는 것과 관련이 있다. </u>(운동학은 물체의 질량을 고려하지 않는다. 왜냐하면 그것은 간접적으로 힘을 고려한다는 것을 의미하기 때문이다.) 따라서 우리는 물체의 운동학을 다룰 때 순수 운동에만 관심이 있다고 말할 수 있습니다. Kinematics는 "어떻게 변화하는가?"에 대해 궁금해하지 않고 "얼마나 변화합니까?"에 대서 연구를 하는 학문이다.  
<br>  

- **운동역학(Kinetics)** 은 <u>질량이 있는 물체의 운동에 대한 힘과 토크의 영향에 관한 것에 대해 연구를 하는 학문</u>이다. 운동학(Kinematics)은 질량을 고려하지 않고 물체의 위치, 속도, 가속도를 구하는 반면, 운동역학(Kinetics)은 운동방정식 $F=ma$를 고려한다.  

## Kinematics와 Dynamics 차이점  

예시를 들어 두 차이점이 무엇인지 이해해보자.  

아래 그림과 같은 2-DoF(Degree of freedom)의 로봇 팔을 만든다고 생각해봅시다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/137092521-1dff3222-237a-4584-a7c9-180b68516fb9.png" width = "400" ></p>  

우리가 로봇의 끝점(End-effector)이 원하는 위치에 보내기 위해서는 모터가 계산된 각도로 움직여야 합니다.  

먼저 로봇을 간단하게 모델링하여 아래 그림과 같이 나타냅시다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/137093091-bfaba108-7978-4963-95c7-6c4b2dff12fa.png" width = "400" ></p>  

이 때 $m_1, m_2$에는 관심이 없고 원하는 위치에 가기 위한 $\theta_{1}, \theta_{2}$를 구하는 것에 집중하는 것이 **운동학(Kinematics)** 입니다.  

하지만 $m_1, m_2$를 고려하고 원하는 각도에 가기 위해 얼마만큼의 모터의 토크가 필요한지 즉, 힘을 고려하여 시스템에 대해 구하는 것이 **동역학(Dynamics)** 입니다.  

두 차이점을 이해하는데 조금이나마 도움이 되었으면 좋겠습니다. 🎉  

## References  

- [The Design and Simulation of Mechanisms](https://www.theseus.fi/bitstream/handle/10024/147813/Thesis_Inna%20Sharifgalieva-last%20edited.pdf;jsessionid=B0143CA51D1AA8F8954FF123C90DD364?sequence=1)  

- [Differentiate Kinetics and Kinematics](https://studiouslyyours.com/kinetics-and-kinematics)  

- [Kinematics and Dynamics - 네이버 블로그](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=chg0118&logNo=220152482328)  

- [What is the difference between "kinematics" and "dynamics"?](https://physics.stackexchange.com/questions/1135/what-is-the-difference-between-kinematics-and-dynamics)  
