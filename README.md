# Deep-Learning-Paper-Review
## Spatial Transformation Networks()

### 1. Introduction
> 기존의 cnn에서는 물체가 공간상에 변화가 있을때 탐지를 못하는 문제가 있었다. 그래서 down sampling을 하고자 하였다. 
> down sampling을 하게되면 spatially invariant할 수 있지만 제약적임으로 동적으로 보완할 수 있는 모듈을 만들고자 한다.

![ex_screenshot](https://jamiekang.github.io/media/2017-05-27-spatial-transformer-networks-ex1.png)
(jamiekang.github, 2018)

### 2. Spatial Transformers

> Spatial Transformes에는 총 3개의 파트가 있다.
> 1. localisation network: transformation parameters 예측
> 2. grid generator: localisation network에서 예측한 값을 parameters로 input map의 point를 sampling할 grid 생성
> 3. sampler: input map과 sampling grid를 input으로 받아 최종적으로 transformed output을 만들어낸다.

![ex_screenshot](https://jamiekang.github.io/media/2017-05-27-spatial-transformer-networks-fig2.png)
(jamiekang.github, 2018)

### 3. Detail Process
#### 3-1. localisation network
> localisation network에서는 input feature map을 가지고 수많은 은닉계층을 통해 예측한 transformation의 parameters(theta)를 output으로 내뱉는다.
> localisation net의 마지막 층에는 regression layer가 있어야 한다. (parameters를 예측하기 위해서)
> 이 때, theta의 사이즈는 transforamtion type에 따라 (affine 혹은 attention) 결정된다.


![ex_screenshot](https://camo.githubusercontent.com/bb81d6267f2123d59979453526d958a58899bb4f/687474703a2f2f692e696d6775722e636f6d2f4578474456756c2e706e67)
(camo.githubusercontent, 2018)

#### Parameterised Sampling Grid: Affine & Attention Transformer
> 위의 이미지에서 왼쪽 이미지에서 I는 identity transformation parameters이고 오른쪽 theta는 Affine transformation parameters이다.
> Affine Transformation은 선의 평행성은 유지가 되면서 이미지를 변환하는 것이다.
> Affine transform은 6개의 parameter로 scale, rotation, translation, skew, cropping을 표현할 수 있다.



> Attention Transformation은 
> floc()은 fully-connected 혹은 convolutional network가 될 수 있다. 그리고 parameters를 예측할 수 있도록 마지막 부분에는 regression layer를 추가해야한다.




> 2. grid generator에서는 1번, localisation network에서 가져온 theta를 parameter로 받아서 sampling grid를 만든다. 
> sampling grid는 points들의 set로 각각의 point는 transformed output을 만들기 위해서 input map에서 뽑을 위치를 나타낸다. 
>
> Differentiable Image Sampling(식설명)



> 3. 마지막으로 feature map과 sampling grid는 sampler의 input으로 들어오고, grid points에서 sampling된 input으로 output map을 만들게 된다.

(figure 3)


