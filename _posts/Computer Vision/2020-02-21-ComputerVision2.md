---
layout: article
title: 02. CS231N 요약 (CNN)
author: J_宋
tags: ComputerVision CS231n 한글
mathjax: true
key: ComputerVision02
---



#### Convolutional Neural Network

##### CNN의 소개

- 간략한 역사 : <a href="https://youtu.be/5t1E3LZ3FDY?list=PL1Kb3QTCLIVtyOuMgyVgT-OeW0PYXl3j5&t=2139">Youtube</a>
- Terminology
  - Localization : 하나의 이미지 내에서 하나의 Object를 찾아내는 것
  - Detection : 하나의 이미지 내에서 여러개의 Object를 찾아내는 것
  - Segmentation : Object의 형태를 따내는 것



##### CNN architecture

<img src="/assets/images/计视/myNote/pic02/cnn_architectures-03.png" alt="cnn_architectures-03" style="zoom:50%;" />

##### Convolution Layer

- 동작 과정 및 설명 (👇 Stride = 2, Padding = 1时)

  <img src="/assets/images/计视/myNote/gif/CNN.gif" alt="CNN" style="zoom:100%;" />

  - Filter

    - 반드시 input과 같은 depth를 가져야됨

    - 간단히 설명하자면 Filter의 역할은 feature 식별용임

       <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-07-30 at 2.01.40 PM.png" alt="Screen Shot 2021-07-30 at 2.01.40 PM" style="zoom:30%;" />

  - Convolution operation

    - filter가 image(= Input volume) 위로 이동하면서, dot product 계산

  - Convolution Layer에서는 *x*개의 filter 가 Convolution Layer의 input위에서 Convolution operation를 진행하면 *x*개의 Activation map들을 생성함

    <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-07-30 at 2.28.57 PM.png" alt="Screen Shot 2021-07-30 at 2.28.57 PM" style="zoom:35%;" />

    - 참고 : Filter가 1 × 1 × input depth 이라도 의미가 있음 

       <img src="/assets/images/计视/myNote/pic02/cs231n18-13screenshot.png" alt="cs231n18-13screenshot" style="zoom:25%;" />

      - But Input이 2차원 ( ex. 6×6×1 )  일 경우 stride 1로 1×1×1 인 Filter 취하는건 아무런 의미 없음

         <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-07-30 at 2.09.16 PM.png" alt="Screen Shot 2021-07-30 at 2.09.16 PM" style="zoom:30%;" />

    - Zero-Padding

      - Zero-Padding를 사용하지 않으면 Activation map이 몇개의 layer 만 거치게 되면 size가 0으로 소멸됨
      - Zero-Padding를 사용하여  Activation map 크기를 보존해주되 그래도 size를 점차 줄여주기 위해 pooling layer가 필요
      - *计算公式1* ： 적합한 Zero-Padding 수 구하기

      $$
      {\text{ Filter Width or Height size } - 1 \over 2 }
      $$

    - 생성된 Activation map들은 다음 Convolution Layer의 input으로 전달됨

      - *计算公式2* ：1개 Activation map의 Width or Height size 구하기

        :warning: total Activation map depth  = filter 개수

      $$
      {\text{ Input Width or Height size } - \text{ Filter Width or Height size } \over \text{ Stride }} + 1
      $$

    - ***Example*** : <img src="/assets/images/计视/myNote/pic02/cs231n15-48screenshot.png" alt="cs231n15-48screenshot" style="zoom:23%;" />

      *Q1 : 그럼 Activation map size는?*

      
      $$
      \text{ Input size (with pad)} = 36 \times 36 \times 3 \\
      \text{ 1 Activation map size } = {36 - 5 \over 1} + 1 = 32 \\
      \text{ Total Activation map size } = 32 \times 32 \times \color{red}{10}
      $$
      

      *Q2 : 이 layer의 parameter수는 ?*

      
      $$
      \text{ Amount of 1 Filter's parameter (with bias)} = (5 \times 5 \times 3 ) + 1 = 76 \\
      \text{ Total amount of 10 Filter's parameter (with bias)} = 76 \times 10 = 760
      $$

##### Pooling Layer

- 각 Activation map을 좀 더 작게, 좀 더 관리할 수 있게 만들어 주는 역할

   <img src="/assets/images/计视/myNote/pic02/cs231n23-52screenshot.png" alt="cs231n23-52screenshot" style="zoom:33%;" />

- Pooling 방법 :

  方法1 ：**Max Pooling** : 

  - Hyperparameters : 
    - Filter size (일반적으론 2 설정)
    - Stride (일반적으론 2 설정)

   <img src="/assets/images/计视/myNote/pic02/cs231n26-3screenshot.png" alt="cs231n26-3screenshot" style="zoom:25%;" />

  - *计算公式3* ：Max Pooling통해 나온 Activation map의 Width or Height size 구하기

    :warning: total Activation map depth = Input depth

    
    $$
    {\text{ Input Width or Height size } - \text{ Filter Width or Height size } \over \text{ Stride }} + 1
    $$

##### Classification Layer

- Flattened Layer
  - used to convert the n-dimensional vector into a mono-dimensional vector

- Fully Connected Layer
  - compute the class scores
  - resulting a vector, where each of the numbers correspond to a class score



#### CNN Case Study

##### LeNet-5 (LeCun at al. 1998)

 <img src="/assets/images/计视/myNote/pic02/cs231n31-48screenshot.png" alt="cs231n31-48screenshot" style="zoom:50%;" />

- Detail : <a href="https://blog.naver.com/laonple/220648539191">Blog</a>



##### AlexNet (Krizhevsky et al. 2012)

 <img src="/assets/images/计视/myNote/pic02/download.png" alt="download" style="zoom:70%;" />

- input size : 227×227×227

- 2개의 GPU로 병렬연산을 수행하기 위해서 병렬적인 구조로 설계됨

- 첫번째 Layer : Convolution Layer

  -  96개의 11x11x3 사이즈 Filter로 input을 Convolution operation 해줌, Stride=4
    - 즉, parameter 개수 = (11x11x3) x 96 = 34,848 
    - 병렬연산을 수행하기 위해  Filter를 48개 48개로 나눔

- 두번째 Layer : Pooling Layer

  - 3x3 사이즈 Filter, Stride=2 적용
  - parameter 개수 = 0, (**parameter는  Convolution Layer때만 있는 것!**)

- 전체 Layer 구조 :

   <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-07-30 at 3.28.05 PM.png" alt="Screen Shot 2021-07-30 at 3.28.05 PM" style="zoom:25%;" />

  -  통용적으로 Classifier 직전에 있는 Fully Connected Layer를 "<span style="color:orange">FC7</span>" 라고도 한다 
  - :warning: <span style="color:green">Normalization Layer</span> : 지금은 효용이 별로 없어서 더이상 사용되지 않는다고 함

- 특징 :

  - 최초로 ReLU를 사용한 모델
  - Data augmentation을 많이 사용함 
  - Fully Connected Layer에서 Dropout = 0.5 
  - Batch size = 128
  - 7가지 CNN 모델을 앙상블해서 error rate를 2.8%정도 줄였음
  - Learning rate = 0.01 (1e-2)



##### ZFNet (Zeiler and Fergus, 2013)

- AlexNet과 유사

   <img src="/assets/images/计视/myNote/pic02/remotesensing-12-01667-g005 copy.png" alt="remotesensing-12-01667-g005 copy" style="zoom:20%;" />

  - 대신 첫번째 Layer의 11x11 Filter, Stride=4 를 7x7 Filter, Stride=2로 줄임
  - 3,4,5번재  Layer의 Filter 개수를 늘림

- Detail : <a href="https://blog.naver.com/laonple/220673615573">Blog</a>



##### VGGNet (Simonyan and Zisserman, 2014)

 <img src="/assets/images/计视/myNote/pic02/clip_image001.png" alt="clip_image001" style="zoom: 50%;" />

- 모든 Convolution Layer에는 오직 3x3 Filter , Stride=1, Padding=1를 사용
- 모든 Max Pooling Layer에는 오직 2x2 Filter , Stride=2 를 사용

- 전체 Layer 

  <img src="/assets/images/计视/myNote/pic02/cs231n 41-0screenshot.png" alt="cs231n 41-0screenshot" style="zoom:50%;" />

  - 위에서 보이듯이 Fully Connected를 사용하는 건 효율적이지 못해 average pooling를 사용할 수 있음

     <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-07-30 at 4.07.16 PM.png" alt="Screen Shot 2021-07-30 at 4.07.16 PM" style="zoom:23%;" />



##### GoogLeNet  (Szegedy et al. 2014)

<img src="/assets/images/计视/myNote/pic02/35002702-d5dccb60-fb2d-11e7-88ac-e29d0319f32b.png" alt="35002702-d5dccb60-fb2d-11e7-88ac-e29d0319f32b" style="zoom:50%;" />

- Inception 모듈이 연속적으로 연결된 형태 

  - Inception module

     <img src="/assets/images/计视/myNote/pic02/cs231n41-59screenshot.png" alt="cs231n41-59screenshot" style="zoom:43%;" />

- Fully Connected Layer를 아예 제거하고 average pooling을 사용함

  - 모델의 Parameter 수를 다 합치면 약 500만개 밖에 안됨!

- AlexNet과 비교했을 때,

  - GoogLeNet 은 AlexNet의 1/12 수준의 parameter를 가짐
  - 연산은 2배이상 빠름
  - Top-5 Error는 약 10% 감소됨 



##### ResNet (He et al. 2015)

- 모델들의 발전

   <img src="/assets/images/计视/myNote/pic02/cs231n45-1screenshot.png" alt="cs231n45-1screenshot" style="zoom:40%;" />

  - Error rate는 점점 :arrow_heading_down: , Layer의 크기는 :arrow_heading_up:



- ResNet의 주장

  <img src="/assets/images/计视/myNote/pic02/cs231n45-58screenshot.png" alt="cs231n45-58screenshot" style="zoom:50%;" />

  - 기존의 Net들은 Layer가 많아짐에 따라 Error rate가 커지는 현상이 나옴으로써 최적화에 실패해 있다고 함
  - ResNet의 경우 Layer가 많아짐에 따라 Error rate는 줄어듬 



- 특징

  - 8개의 GPU machine에서 2-3주 동안 train을 시킴

  - test (runtime) 할 때는 비록 VGGNet 보다 8배 더 많은 Layer를 가지고 있지만 성능은 더 빠름. How?

    - 첫번째 Convolution Layer다음 바로 Pooling을 통해 size를 56x56으로 줄여 이후 150개 Layer들은 이 size 기반으로 진행됨

    - Skip Connection

      <img src="/assets/images/计视/myNote/pic02/cs231n47-43screenshot.png" alt="cs231n47-43screenshot" style="zoom:50%;" />

      - Plue 연산을 추가해서 Backpropagation이 진행될 때, 과정을 건너뛰고 앞쪽 Convolution Layer로 넘어갈 수 있음

         :warning: 参考 ： <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-07-31 at 10.24.45 AM.png" alt="Screen Shot 2021-07-31 at 10.24.45 AM" style="zoom:30%;" />

  - 모든 Convolution Layer 다음 Batch Normalization을 사용함

  - Xavier/2 Weight Initialization을 사용함 (He et al. 이 만든거임)

  - Learning rate = 0.1,  validation set error가 정체 되면 10으로 나눴음

  - dropout을 아예 사용하지 않음 



##### DeepMind's AlphaGo

<img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-07-30 at 4.51.14 PM.png" alt="Screen Shot 2021-07-30 at 4.51.14 PM" style="zoom:50%;" />

- Input size : 19x19x48
- Output size : 19x19 (19x19 인 바둑판의 어디에 두어야하는지에 대한 확률)



#### 최근 Trend

- 더 작은 Filter 사용
- 더 깊은 Layer의 architecure 사용
- Pooling Layer, Fully Connected Layer를 점차 사용하지 않음
  - 그냥 Convolution Layer만 사용 : Stride를 활용하여 spacial reduction하는 방식으로





***

《Reference》

1.  <a href="https://www.youtube.com/watch?v=5t1E3LZ3FDY&list=PL1Kb3QTCLIVtyOuMgyVgT-OeW0PYXl3j5&index=6">Youtube</a>
2. <a href="https://leechamin.tistory.com/98?category=830805">Blog</a>
3. <a href="https://cs231n.github.io/">Course Site</a>
4. <a href="https://developer.qualcomm.com/software/qualcomm-neural-processing-sdk/learning-resources/cnn-architectures/deep-learning-convolutional-neural-networks-computer-vision">Qualcomm explanation</a>

***

