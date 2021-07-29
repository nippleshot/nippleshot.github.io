---
layout: article
title: 01. Deep Learning 요약
author: J_宋
tags: DeepLearning 한글
mathjax: true
key: deepLearning01
---



#### Fundamental

##### Machine Learning

 <img src="/Users/song-giljae/nippleshot.github.io/assets/images/深度学习/myNote/pic01/Screen Shot 2021-07-27 at 11.13.21 AM.png" alt="Screen Shot 2021-07-27 at 11.13.21 AM" style="zoom:40%;" />

- Supervised learning

   <img src="/Users/song-giljae/nippleshot.github.io/assets/images/深度学习/myNote/pic01/Screen Shot 2021-07-27 at 11.13.33 AM.png" alt="Screen Shot 2021-07-27 at 11.13.33 AM" style="zoom:30%;" />

  - 입력 데이터에 대한 정답(label 또는 class)이 함께 주어짐

- Unsupervised learning

   <img src="/Users/song-giljae/nippleshot.github.io/assets/images/深度学习/myNote/pic01/Screen Shot 2021-07-27 at 11.13.43 AM.png" alt="Screen Shot 2021-07-27 at 11.13.43 AM" style="zoom:30%;" />

  - 모델의 입력값으로 오직 입력 데이터만 주어짐
  - 즉 정답이 없는 학습을 모델에게 요구하는 것
  - 데이터 분석을 통해 unknown pattern을 학습할 수 있게 됨



#### Linear regression 

- 선형 회귀는 <span style="color:blue">한 개 이상의 독립 변수 x</span>와 <span style="color:green">종속 변수 y</span>의 선형 관계를 모델링함
  - <span style="color:blue">독립 변수 x</span> : 독립적으로 변할 수 있는 것
  - <span style="color:green">종속 변수 y</span> : 독립 변수 값에 의해서 종속적으로 결정되는 것

- **Simple Linear Regression Analysis**
  $$
  y = Wx+b
  $$

  - W와 b의 값을 적절히 찾아내면 x와 y의 관계를 적절히 모델링한 것이 됨

- **Multiple Linear Regression Analysis**
  $$
  y=W_{1} x_{1}+W_{2} x_{2}+\ldots W_{n} x_{n}+b
  $$

  -  다수의 요소를 가지고 y를 예측

- Loss Function

  - a.k.a Objective function or Cost function

  - x와 y의 관계를 잘 나타내는 W_i와 b의 값을 찾기

    - 실제값과 가설로부터 얻은 예측값의 오차를 계산하는 식을 세우고, 이 식의 값을 최소화하는 최적의 W_i와 b를 찾아냄

  - 회귀 문제의 경우에는 주로 Mean Squared Error, MSE 가 사용됨

    - MSE : 모든 점과 직선 사이의 ↕ 거리(음수,양수 고려)를 제곱하고 모두 더하고 데이터의 개수만큼 나누면 됨

     <img src="/Users/song-giljae/nippleshot.github.io/assets/images/深度学习/myNote/pic01/Screen Shot 2021-07-27 at 12.10.59 PM.png" alt="Screen Shot 2021-07-27 at 12.10.59 PM" style="zoom:30%;" />

    - MSE를 Cost function으로 설정 :

       <img src="/Users/song-giljae/nippleshot.github.io/assets/images/深度学习/myNote/pic01/Screen Shot 2021-07-27 at 12.15.02 PM.png" alt="Screen Shot 2021-07-27 at 12.15.02 PM" style="zoom:40%;" />

    - 모든 점들과의 오차가 大 MSE는 :arrow_heading_up: , 오차가 小 MSE는 :arrow_heading_down:

    - 즉, MSE (Cost function) 를 최소가 되게 만드는 W_i와 b를 구하면 결과적으로 y와 x의 관계를 가장 잘 나타내는 직선을 그릴 수 있다





***

《Reference》

1. Won Joon Yoo, Introduction to Deep Learning for Natural Language Processing, Wikidocs



***

