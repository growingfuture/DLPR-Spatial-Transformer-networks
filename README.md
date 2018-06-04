# Deep-Learning-Paper-Review

### 1. Introduction
> 기존의 cnn에서는 물체가 공간상에 변화가 있을때 탐지를 못하는 
> down sampling을 하게되면 spatilly invariant할 수 있지만 제약적임으로 동적으로 보완할 수 있는 모듈을 만들고자 함

### 2.Spatial Transformers

> Spatial Transformes에는 총 3개의 파트가 있다.
1. localisation network: transformation parameters 예측
2. grid generator: localisation network에서 예측한 값을 parameters로 input map의 point를 sampling할 grid 생성
3. sampler: input map과 sampling grid를 input으로 받아 최종적으로 transformed output을 만들어냄


##### Detail Process
> 1. localisation network에서는 input feature map을 가지고 수많은 은닉계층을 통해 예측한 transformation의 parameters(theta)를 output으로 내뱉는다.
> localisation net의 마지막 층에는 regression layer가 있어야 한다. (parameters를 예측하기 위해서)

figure(3)

> 이 떄, theta의 사이즈는 transforamtion type에 따라 (affine 혹은 attention) 결정된다.

(affine, attention 이미지)

> Affine Transformation은 선의 평행성은 유지가 되면서 이미지를 변환하는 것을 일컫음

> affine 식
> attention 식

> floc()은 fully-connected 혹은 convolutional network가 될 수 있다. 하지만 final regression layer는 포함시키면 안된다고 한다.




> 2. grid generator에서는 1번, localisation network에서 가져온 theta를 parameter로 받아서 sampling grid를 만든다. 
> sampling grid는 points들의 set로 각각의 point는 transformed output을 만들기 위해서 input map에서 뽑을 위치를 나타낸다. 
>
> Differentiable Image Sampling(식설명)



> 3. 마지막으로 feature map과 sampling grid는 sampler의 input으로 들어오고, grid points에서 sampling된 input으로 output map을 만들게 된다.

(figure 3)


