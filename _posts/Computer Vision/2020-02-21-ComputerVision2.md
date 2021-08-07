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

    <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-08-04 at 1.02.10 PM.png" alt="Screen Shot 2021-08-04 at 1.02.10 PM" style="zoom:40%;" />

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

 <img src="/assets/images/计视/myNote/pic02/AlexNet-1.png" alt="AlexNet-1" style="zoom:67%;" />

- input size : 227×227×3

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
  -  Fully Connected Layer의 문제점 :
    1. 연산량 매우 많음
    2. classification에 영향을 끼친 feature가 어느 부분에서 추출되었는지 (위치 정보) 도 알 수 없음
    3. input 의 크기 또한 제한된다

##### Global Average Pooling (GAP)

- Fully Connected Layer의 대안으로 제시된 것

  <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-08-01 at 9.22.52 PM.png" alt="Screen Shot 2021-08-01 at 9.22.52 PM" style="zoom:50%;" />

  - GAP는 각각의 feature map(= Activation map)에서 대표값(average)을 추출해 바로 각 class에 대한 score로 구성된 vector로 나타남

    <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-08-01 at 9.18.51 PM.png" alt="Screen Shot 2021-08-01 at 9.18.51 PM" style="zoom:35%;" />

- GAP의 특징 :
  - 이전 feature map들이 가지고 있는 위치 정보를 유지하면서 class category로 직접 연관시키는 역할
  - 별도의 parameter optimization 을 필요로 하지 않기 때문에 연산량이 적음
    - overfitting 문제도 차단할 수 있음



#### Adversarial examples

- Input 이미지에 대한 최적화를 함으로써 우리는 어떤 Class에 Score라도 maximize할 수 있음

  - 이런 방법을 사용해서 CNN를 속일 수 도 있음 

     <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-08-02 at 4.58.25 PM.png" alt="Screen Shot 2021-08-02 at 4.58.25 PM" style="zoom:30%;" />

- EXPLAINING AND HARNESSING ADVERSARIAL EXAMPLES [Goodfellow, Shlens & Szegedy, 2014]

  - "Neural Network이 Adversarial attack에 취약한 것은 Linear 한 성질 때문이다"

    ***Example.*** Binary linear classifier를 속여보기

     <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-08-02 at 5.04.18 PM.png" alt="Screen Shot 2021-08-02 at 5.04.18 PM" style="zoom:40%;" />

    ​	👉 class 1과 class 0의 확률을 합하면 1.0 이 나옴, 그래서 class 1 확률 = 1.0 - class 0의 확률

     <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-08-02 at 5.06.55 PM.png" alt="Screen Shot 2021-08-02 at 5.06.55 PM" style="zoom:30%;" />

    💁🏻 이제 Adversarial한 x를 만들어보자 (즉 결과가 classifier가 class 1이 나오도록 x를 조정해보자)

    - 대응하는 w가 양수 일 경우 x를 크게 만들고, w가 음수 일 경우 x를 작게 만들자

      - 대신 Adversarial한 x를 원래 x와 유사하게 보여야되기 때문에 약간의 변형만 준다 (한 0.5정도)

       <img src="/assets/images/计视/myNote/pic02/Screen Shot 2021-08-02 at 5.12.46 PM.png" alt="Screen Shot 2021-08-02 at 5.12.46 PM" style="zoom:30%;" />

  

- image *x* 같은 경우 큰 차원(ex. 224x224)을 가지고 있기 때문에 더 약간의 변화만 주더라도 매우 쉽게 Adversarial한 *x*를 만들 수 있음

  - 그래서 Linear regression은 어떤 변화를 줘야하는지 정확히 파악하고 있는 경우라면 아주 작은 변화라도 크게 변경을 시킬 수 있음

- 사실 이런 문제들은 image에서만 국한되는 것이 아니라, speak recognition 등 여러 방면들에서 발생할 수 있음



#### Data Augmentation

- 데이터의 label은 변함없이 Pixel을 고침 그리고 이 변경된 데이터를 학습함 

- 방법들 :

1. 반대로 filp하는 방법

2. 랜덤하게 자르고 스케일도 다양하게 하는 방법

3. 색을 jittering 하기

   - 복잡한 방법 :

     [1] 이미지의 R,G,B에 대해 <a href="https://excelsior-cjh.tistory.com/167">PCA</a>를 적용함 

     [2] 그럼 각 R,G,B에 대해서 Principal componet direction (color가 변화해 나가는 방향성)을 따라서 color의 offset을 sampling 해줌

     [3] 이 offset을 모든 pixel에 더해줌

and more ...

- 작은 데이터셋의 경우 매우 유용



#### All About Convolutions

##### Small Filter

다음 방식들을 활용하면 CNN의 parameter 수가 줄어들어 연산량이 줄고 CNN가 더 non-linearity하게 됨 

1. ***큰 filter 사용을 피하고 작은 filter를 사용하는 방식으로 대체하자*** 

   <img src="/assets/images/计视/myNote/pic04/Screen Shot 2021-08-04 at 4.27.54 PM.png" alt="Screen Shot 2021-08-04 at 4.27.54 PM" style="zoom:30%;" />

   - 3개의 conv. layer가 있고 각 layer에는 3x3 filter (stride=1)를 사용한다하자

   - 직관적으로 봤을 때, 3번째 conv. layer을 통과한 Activation map의 한 뉴런은 input layer의 7x7 영역 (Receptive field) 을 보게되는 셈임

     - 즉, 7x7 filter (stride=1)를 지닌 conv. layer 1개를 통과 시킨 것과 같은 영향을 보이는 셈이다

        <img src="/assets/images/计视/myNote/pic04/Screen Shot 2021-08-04 at 4.37.41 PM.png" alt="Screen Shot 2021-08-04 at 4.37.41 PM" style="zoom:30%;" />

   - 그럼 위에 두 상황에서 parameter 수량과 연산횟수를 계산해보면 작은 filter를 사용하는 방식이 두 방면 모두 더  적다는 걸 알 수 있다. 

      <img src="/assets/images/计视/myNote/pic04/21-57screenshot.png" alt="cs231n 11강 CNNs in practice 21-57 screenshot" style="zoom:25%;" />

   - 그리고 더 층을 쌓을 수로 그만큼 activation function도 쌓아지기 때문에 non-linearity가 더 강화된다

2. ***1x1 "bottleneck" convolution도 매우 효과적임***

    <img src="/assets/images/计视/myNote/pic04/Screen Shot 2021-08-04 at 4.58.21 PM.png" alt="Screen Shot 2021-08-04 at 4.58.21 PM" style="zoom:30%;" />

   - Parameter 수량 계산 (bias 제외)

     |                            | Bottleneck       |                      | Single      |
     | -------------------------- | ---------------- | -------------------- | ----------- |
     | **C/2개 1x1xC filter **    | 1xCx(C/2) 개     | **C개 3x3xC filter** | 9xCxC 개    |
     | **C/2개 3x3x(C/2) filter** | 9x(C/2)x(C/2) 개 |                      |             |
     | **C개 1x1x(C/2) filter **  | 1x(C/2)x(C) 개   |                      |             |
     | **Total (Sum)**            | *3.25 x (C^2)*   | **Total (Sum)**      | *9 x (C^2)* |

     💁🏻 즉 Bottleneck 모델의 Parameter 수량이 더 적고 이로 인해 연산량도 더 적을 것임! (+ 더 깊은 층으로 인한 non-linearity 강화)

3. ***NxN convolution의 경우 1xN 그리고 Nx1의 두 개의 layer로 나누자***

     <img src="/assets/images/计视/myNote/pic04/Screen Shot 2021-08-04 at 5.13.46 PM.png" alt="Screen Shot 2021-08-04 at 5.13.46 PM" style="zoom:30%;" />

   💁🏻 위에서 보이다 싶이 Parameter 수량이 더 적어져 연산이 가벼워짐 (+ 더 깊은 층으로 인한 non-linearity 강화)

     

- 사용된 사례 : GoogLeNet

   <img src="/assets/images/计视/myNote/pic04/Screen Shot 2021-08-04 at 5.16.12 PM.png" alt="Screen Shot 2021-08-04 at 5.16.12 PM" style="zoom:40%;" />



##### Computing Convolutions

###### im2col

- 다차원의 데이터를 행렬로 변환하여 행렬 연산을 하도록 해주는 함수

- 다차원 데이터의 합성곱(convolution) 결과 ==  im2col을 통해 행렬로 변환된 데이터의 내적 결과

  1. 입력 데이터(4x4x3)를 행렬로 변환 

   <img src="/assets/images/计视/myNote/gif/im2col.gif" alt="im2col" style="zoom:40%;" />

  2. 2개 filter(2x2x3)도 행렬로 변환

   <img src="/assets/images/计视/myNote/pic04/im2col2.png" alt="im2col2" style="zoom:43%;" />

  3. 변환한 두 행렬의 곱

   <img src="/assets/images/计视/myNote/gif/im2colMul.gif" alt="im2colMul" style="zoom:60%;" />

  4. 행렬로 변환한 입력 데이터와 필터의 행렬 연산 이후, 우리는 출력된 데이터(행렬)를 다시 원래의 데이터(3차원)로 변환해주는 작업을 해야됨

  <img src="/assets/images/计视/myNote/gif/col2im.gif" alt="col2im" style="zoom: 60%;" />

  

- im2col를 사용하면 연산 속도는 빠르다. 

  - 하지만 연산해야하는 데이터 수가 늘어났기 때문에 연산 메모리가 증가한다는 단점도 존재한다.



###### Fast Fourier Transform (FFT)

 <img src="/assets/images/计视/myNote/pic04/32-42screenshot.png" alt="32-42screenshot" style="zoom:33%;" />

- Filter가 클 경우(7x7) 효과가 상당히 있지만 Filter가 작을 경우(3x3) 효과가 그다지 크지 않음



###### Fast Algorithms

- Strassen's Algoritm을 사용하여 matrix multiplication 복잡도를 줄임
- Details : <a href="https://arxiv.org/abs/1509.09308">Paper</a>



##### Floating point precision

- 일반적으로 프로그래밍에서는 64bit (double precision)가 default

- 성능 향상을 위해 32bit (single precision)를 사용할 수 있음 

- 요즘 16bit (half precision)도 지원하는 library 도 있음 

  - 하지만 Floating point precision 문제가 발생할 수 있음 
  - 16bit (half precision)로 Training과 Testing를 할 경우 error률이 발산함
    - Stochastic rounding 방법 : parameter 와 activation을 모두 16bit로 일단 생성한 다음 곱셉연산이 일어나는 경우에 더 높은 bit로 잠시 올려 줬다가 곱셉연산이 끝나면 다시 낮추는 방식
    - Stochastic rounding 를 적용했을 때 error률이 수렴하는데 성공을 함

- 2016년에는 parameter 와 activation을 모두 1bit로 학습을 시키는 모델을 제안함 (BinaryNet)

  - 모든 parameter 와 activation는 +1 또는 -1 로 설정 

    - 단, gradient는 좀 더 높은 precision을 사용함 

  - Bitwise XNOR 연산을 활용해서 빠른 곱셉을 수행함

    



***

《Reference》

1.  <a href="https://www.youtube.com/watch?v=5t1E3LZ3FDY&list=PL1Kb3QTCLIVtyOuMgyVgT-OeW0PYXl3j5&index=6">Youtube</a>
2. <a href="https://vision4me.tistory.com/5">Blog - GAP</a>
3. <a href="https://cs231n.github.io/">Course Site</a>
4. <a href="https://developer.qualcomm.com/software/qualcomm-neural-processing-sdk/learning-resources/cnn-architectures/deep-learning-convolutional-neural-networks-computer-vision">Qualcomm explanation</a>
5. <a href="https://poloclub.github.io/cnn-explainer/">CNN Workflow Visualization</a>
6. <a href="https://hackmd.io/@bouteille/B1Cmns09I#II-Backward-propagation">Blog - im2col</a>

***

