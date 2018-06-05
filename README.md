# Deep-Learning-Paper-Review
## Spatial Transformation Networks()

### 1. Introduction
> * 기존의 cnn에서는 물체가 공간상에 변화가 있을때 탐지를 못하는 문제가 있었다. (rotation(회전),translation(평행이동)등)
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

#### 3-1. Localisation network
> * localisation network에서는 input feature map을 가지고 수많은 은닉계층을 통해 예측한 transformation의 parameters(theta)를 output으로 내뱉는다.
> * localisation net의 마지막 층에는 regression layer가 있어야 한다. (parameters를 예측하기 위해서)
> * 이 때, theta의 사이즈는 transforamtion type에 따라 (affine 혹은 attention) 결정된다.
> * floc()은 fully-connected 혹은 convolutional network가 될 수 있다. 그리고 parameters를 예측할 수 있도록 마지막 부분에는 regression layer를 추가해야한다.
![ex_screenshot](http://acm.ee.ccu.edu.tw:2017/Image%20of%20Paper/Figure%202.jpg)


#### 3-2. Grid generator
![ex_screenshot](https://camo.githubusercontent.com/bb81d6267f2123d59979453526d958a58899bb4f/687474703a2f2f692e696d6775722e636f6d2f4578474456756c2e706e67)
(camo.githubusercontent, 2018)
> * grid generator에서는 localisation network에서 가져온 theta를 parameter로 받아서 sampling grid를 만든다. 

> * sampling grid의 각각의 point는 input feature map에서 뽑을 위치를 나타낸다. 


#### 3-2a. Parameterised Sampling Grid: Affine & Attention Transformer
![ex_screenshot](https://tang.su/images/paper-notes/Spatial-Transformer-Networks/pic1.png)
> * xs,ys : T(G)의 픽셀(위치)이며, inuput feature에서 sampling kernel을 적용한 값을 가져올 spatial location을 나타낸다.
> * xt,yt : output featuremap의 target픽셀(위치)이다.
> * Affine Transformation은 선의 평행성은 유지가 되면서 이미지를 변환하는 것이다.
> * Affine transform은 6개의 parameter로 scale, rotation, translation, skew, cropping을 표현할 수 있다.
> * 3개의 parameter로 isotropic scale (= 가로와 세로 비율이 같은 확대/축소), translation, cropping을 표현하는 attention transform도 있다.

#### 3-3. Sampler 
> * 마지막으로 feature map과 sampling grid는 sampler의 input으로 들어오고, grid points에서 sampling된 input으로 output map을 만들게 된다.

#### 3-3a. Differentiable Image Sampling
![ex_screenshot](http://northstar-www.dartmouth.edu/doc/idl/html_6.2/images/Interpolation_Methods-13.jpg)
> Image sampling은 미분가능한 kernel로 선형보간법을 사용한다.

##### 3-3b. interpolation(선형보간법)
>![ex_screenshot](https://t1.daumcdn.net/cfile/tistory/2378C54C52D3842030)
> 선형보간법은 x1, x2에서 데이터 값을 알고 있을 때 x1<=xi<=x2에서 값을 추정하는 것을 말한다.

![ex_screenshot](https://t1.daumcdn.net/cfile/tistory/2222AF3552D3BFDB2A)

> 이 경우 보간 방법은 원래 사각형의 네점 ABCD를 A'B'C'D'으로 변환시키는 선형변환(linear transformation) T를 구한 후, 구한 T를 이용하여 P를 변환시킨 P'를 구하고 단위 정사각형에서 bilinear interpolation을 수행하면 된다

#### 3-3c. 논문에서의 선형보간법(integer & bilinear sampling kernel)
> 본 논문에서는 interpolation으로 integer sampling kernel과 bilinear sampling kernel을 소개하고 있다.(interpolation == smapling kernel)


> * Unm : feature map의 (n,m)위치의 value값 / v : output의 위치 (xt,yt)
> * xs,ys : T(G)의 픽셀(위치)이며, inuput feature에서 sampling kernel을 적용한 값을 가져올 spatial location을 나타낸다.
> * xt,yt : output featuremap의 target픽셀(위치)이다.
> * c : channel
> * ingter sampling kernel의 경우, x+0.5를 반올림하여 x에서 가장 가까운 integer를 찾는 방법으로 결국 pixel(x,y)에서 가장 가까운 점을 찾는다고 보면 된다.
> * bilinear sampling kernel의 경우, max함수를 써서 nearest neighbor interpolation에 비하여 좀 더 스무딩하게 출력하는는 보간법이다.
>![ex_screenshot] https://jamiekang.github.io/media/2017-05-27-spatial-transformer-networks-interpolation.jpg
> linear, bilinear 선형보간법 참고: https://m.blog.naver.com/PostView.nhn?blogId=aorigin&logNo=220947541918&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F


### Spatial Transformer Networks
> * Localisation network, grid generator와 sampler로 구성한 spatial transformer module을 CNN 구조에 끼워 넣은 것을 Spatial Transformer Network라고 한다.
> * Spatial transformer module은 CNN의 어느 지점에나, 몇 개라도 이론상 집어넣을 수 있다. 
> * 해당 모듈은 계산속도가 굉장히 빠르고 상황에 따라 속도를 증가시킬 수도 있다. (down sampling의 역할)
>
>
>
### Experiments
>
> 
.
>

