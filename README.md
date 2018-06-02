# Deep-Learning-Paper-Review

### Introduction
> 기존의 cnn에서는 local max-pooling(e.g. 2x2 pixels)으로 불변 해결하려고 하였지만 small spatial supprot로
> 깊은 계층의 max-pooling과 convolution에만 실현가능했고, cnn의 매개 feature map(convolutional layer activations)은 사실상
> input data의 large transformations에 invariant하지 않는다는 한계점이 있었다

### Spatial Transformers

> Spatial Transformes에는 총 3개의 파트가 있다.
1. localisation network
2. grid generator
3. sampler

> Process
> 1. localisation network에서는 input feature map을 가지고 수많은 은닉계층을 통하여 transformation의 parameters(theta)를 output으로 내뱉는다.
> 이 떄, theta의 사이즈는 transforamtion type에 따라 (affine 혹은 attention) 결정된다.

2. grid generator에서는 
