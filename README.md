# Deep-Learning-Paper-Review
## Spatial Transformation Networks()

### 1. Introduction
> * 기존의 cnn에서는 물체가 공간상에 변화가 있을때 탐지를 못하는 문제가 있었다. (rotation등)
> * spatial pooling(=down sampling, =sub sampling)을 하게되면 중요한 정보만 추출함으로 spatially invariant할 수 있지만 제약적임으로 동적으로 보완할 수 있는 모듈을 만들고자 한다.

![ex_screenshot](https://jamiekang.github.io/media/2017-05-27-spatial-transformer-networks-ex1.png)
(jamiekang.github, 2018)

### 2. Spatial Transformers Schema

> Spatial Transformes에는 총 3개의 파트가 있다.
> 1. localisation network: transformation parameters 예측
> 2. grid generator: localisation network에서 예측한 값을 parameters로 input map의 point를 sampling할 grid 생성
> 3. sampler: input map과 sampling grid를 input으로 받아 최종적으로 transformed output을 만들어낸다.

![ex_screenshot](https://jamiekang.github.io/media/2017-05-27-spatial-transformer-networks-fig2.png)
(jamiekang.github, 2018)



### 3. Detail Process

#### 3-1. localisation network
> * localisation network에서는 input feature map을 가지고 수많은 은닉계층을 통해 예측한 transformation의 parameters(theta)를 output으로 내뱉는다.
> * localisation net의 마지막 층에는 regression layer가 있어야 한다. (parameters를 예측하기 위해서)
> * 이 때, theta의 사이즈는 transforamtion type에 따라 (affine 혹은 attention) 결정된다.
>
> 3-2. grid generator에서는 1번, localisation network에서 가져온 theta를 parameter로 받아서 sampling grid를 만든다. 
> * sampling grid는 points들의 set로 각각의 point는 transformed output을 만들기 위해서 input map에서 뽑을 위치를 나타낸다. 
>
#### 3-1a. Parameterised Sampling Grid: Affine & Attention Transformer
> * 아래의 이미지에서 왼쪽 이미지에서 I는 identity transformation parameters이고 오른쪽 theta는 Affine transformation parameters이다.
> * Affine Transformation은 선의 평행성은 유지가 되면서 이미지를 변환하는 것이다.
> * Affine transform은 6개의 parameter로 scale, rotation, translation, skew, cropping을 표현할 수 있다.
![ex_screenshot](https://camo.githubusercontent.com/bb81d6267f2123d59979453526d958a58899bb4f/687474703a2f2f692e696d6775722e636f6d2f4578474456756c2e706e67)
(camo.githubusercontent, 2018)
>
>
> * 3개의 parameter로 isotropic scale (= 가로와 세로 비율이 같은 확대/축소), translation, cropping을 표현하는 attention model이 있다.
> * floc()은 fully-connected 혹은 convolutional network가 될 수 있다. 그리고 parameters를 예측할 수 있도록 마지막 부분에는 regression layer를 추가해야한다.
>
![ex_screenshot](http://northstar-www.dartmouth.edu/doc/idl/html_6.2/images/Interpolation_Methods-13.jpg)
>
> 3. 마지막으로 feature map과 sampling grid는 sampler의 input으로 들어오고, grid points에서 sampling된 input으로 output map을 만들게 된다.
>

>
> 3-1. Differentiable Image Sampling
> 위의 이미지는 input feature map의 spatrial transformation을 하기 위해서 sampler는 
>
![ex_screenshot](http://northstar-www.dartmouth.edu/doc/idl/html_6.2/images/Interpolation_Methods-13.jpg)
> 위의 그림에서보는것처럼 그 위치가 정확히 정수 좌표 값을 가지지 않을 가능성이 더 높기 때문에 interpolation(선형보간법)을 통해 계산을 해야합니다.
>
>
> 3-1a interpolation(선형보간법)과 sampling kernel
>![ex_screenshot](https://t1.daumcdn.net/cfile/tistory/2378C54C52D3842030)
> 선형보간법은 x1, x2에서 데이터 값을 알고 있을 때 x1<=xi<=x2에서 값을 추정하는 것을 말합니다.
>
> 본 논문에서는 interpolation으로 integer sampling kernel과 bilinear sampling kernel을 소개하고 있습니다.(interpolation == smapling kernel)
>


>
>
> 이 식에서는 xi와 m, yi와 n사이의 값을 추정한다고 보면 됩니다.
> 
![ex_screenshot](https://t1.daumcdn.net/cfile/tistory/2222AF3552D3BFDB2A)
> 이 경우 보간 방법은 원래 사각형의 네점 ABCD를 A'B'C'D'으로 변환시키는 선형변환(linear transformation) T를 구한 후, 구한 T를 이용하여 P를 변환시킨 P'를 구하고 단위 정사각형에서 bilinear interpolation을 수행하면 된다
>
![ex_screenshot](http://acm.ee.ccu.edu.tw:2017/Image%20of%20Paper/Figure%202.jpg
>
>

