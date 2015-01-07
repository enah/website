---
title: A Step-By-Step Guide to the Chernoff Bound
author: Ena Hariyoshi
---
I TA-ed for the Discrete Mathematics and Probability course (CS70) in the EECS department at UC Berkeley in Fall 2014. Every year students have difficulty with Chernoff bounds; I thought I'd leave this here that future CS70 students - and others - can use this as a reference.

Some overview about the Chernoff Bound: The Chernoff Bound is derived by applying the Markov Bound to $M(s) =  \mathbb{E}[e^{sX}]$, which is the time-reversed [bilateral Laplace transform](http://en.wikipedia.org/wiki/Two-sided_Laplace_transform) of the random variable $X$ and is commonly called the [moment-generating function](http://en.wikipedia.org/wiki/Moment-generating_function).

A brief note about moments: the mean of a random variable is analogous to the first moment; the variance of a random variable is analogous to the second moment; third moment is skew; and so on. $M(s) =  \mathbb{E}[e^{sX}]$ is a representation of all the moments, by Taylor expansion. Many probability distributions can be expressed as this representation of all the moments, but some cannot. If you're curious you can read the Wikipedia page.  
It is a transform in the same sense that the Fourier transform, or the Fourier series, takes a function and represents it as a series of coefficients of complex exponentials (sines and cosines).

This is a list you can work through step by step to derive the Chernoff Bound.

1. First, note that $\Pr(X \geq \alpha) = \Pr(e^{sX} \geq e^{s\alpha})\ \forall s \geq 0$  
(Because $y = e^{sx}$ is strictly increasing).
2. $$\Pr(e^{sX} \geq e^{s\alpha}) \leq \frac{\mathbb{E}[e^{sX}]}{e^{s\alpha}}$$ (Is $e^{sX}$ always positive?)
3. Minimize $$\frac{\mathbb{E}[e^{sX}]}{e^{s\alpha}}$$ over all $s \geq 0$ to find the tightest bound.
4. This is the same as minimizing $$\ln \frac{\mathbb{E}[e^{sX}]}{e^{s\alpha}} = \ln \mathbb{E}[e^{sX}] - s\alpha$$ (Why is minimizing this the same? $y = \ln\ x$ is strictly increasing. Why do we do this? Which one is easier to minimize?)
5. When $X = \sum X_i$ and $X_i$ are independent, $\mathbb{E}[e^{sX}] = \prod \mathbb{E}[e^{sX_i}]$
6. When $X_i$ are identical, $= \mathbb{E}[e^{sX_i}]^n$
7. Then, $\ln \mathbb{E}[e^{sX}] - s\alpha = n\ln \mathbb{E}[e^{sX_i}] - s\alpha$
8. Let $s'$ be the $s$ that minimizes $n\ln \mathbb{E}[e^{sX_i}] - s\alpha$. Then, $$n \frac{\partial \ln \mathbb{E}[e^{s'X_i}]}{\partial s'} = \alpha$$
9. At this point, we are stuck unless we know the distribution of $X_i$. Let $X_i \sim Bernoulli(p)$.  
Then, $\mathbb{E}[e^{sX_i}] = p \cdot e^{s \cdot 1} + (1-p) \cdot e^{s \cdot 0} = pe^s + 1 - p$
10. Solve for $$n \frac{\partial \ln (pe^{s'} + 1 - p)}{\partial s'} = \alpha \\ \frac{npe^{s'}}{pe^{s'} + 1 - p} = \alpha \\ e^{s'} = \frac{\alpha}{n - \alpha}\frac{1-p}{p}$$
11. $$e^{s'} = \frac{\alpha}{n - \alpha}\frac{1-p}{p}$$ minimizes $$\frac{\mathbb{E}[e^{sX}]}{e^{s\alpha}} = \frac{\mathbb{E}[e^{sX_i}]^n}{e^{s\alpha}} = \frac{(pe^s + 1 - p)^n}{e^{s\alpha}}$$
12. Plug in, and massage into nicer forms. Let $an = \alpha$.
$$\Pr(X \geq \alpha) \leq \frac{\left(\frac{\alpha(1-p)}{n-\alpha} + 1-p\right)^n}{\left(\frac{\alpha}{n-\alpha}\frac{1-p}{p}\right)^\alpha} = \frac{\left(\frac{a(1-p)}{1-a} + 1-p\right)^n}{\left(\frac{a}{1-a}\frac{1-p}{p}\right)^{\alpha}} \\ = \frac{\left(\frac{1-p}{n-a}\right)^n}{\left(\frac{1-p}{1-a}\frac{a}{p}\right)^{\alpha}} = \left(\frac{1-p}{1-a}\right)^{n-\alpha}\left(\frac{p}{a}\right)^{\alpha}$$
As $n$ gets large, for what values of $p$ and $a$ does $\Pr(X \geq \alpha)$ get small?
14. Extra note: the bound is also said to be $e^{-n\Phi_{X_i}(a)}$, where $$\Phi_{X_i}(a) = a \ln \frac{a}{p} + (1 - a) \ln \frac{1-a}{1-p}$$
$\Phi_{X_i}(a)$ is known as the [Kullback-Leibler divergence](http://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence).

