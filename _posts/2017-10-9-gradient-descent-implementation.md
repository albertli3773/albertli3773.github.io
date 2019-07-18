---
layout: post
title: Gradient Descent Algorithm Matlab Implementation
description: >
  This is a implementation of gradient descent written in Matlab
tags: [implementation, matlab]
---

Here's a very basic Matlab implementation of the gradient descent algorithm for
a regression problem.

~~~matlab

for i = 1:iters
    J(i) = 0;
    %update yhat
    yhat = X * beta;
    %update gradient
    errors = (yhat - y);
    grad = (alpha * (1/n) * errors' * X);
    %update beta
    beta= beta - grad';
    beta_hist(i,:)=beta;
    %betas(i)=sum((beta-bhat).^2);
    %calculate cost function
    sse(i) = sum((yhat-y).^2);
    J(i) = 1/(2 * n) * sse(i);   
    cost(i) = J(i);
end

~~~

The following plot shows the cost for every iteration in the algorithm. It
should be decreasing function as in each iteration we try to move towards the
global minimum direction.

<img src="{% asset 'cost.png' @path %}" alt="cost" />
