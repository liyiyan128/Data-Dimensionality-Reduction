# Data Dimensionality Reduction: *PCA and NMF to the Monkey BMI Tensor Dataset ^[1]^*

This project is originated from my year 1 Individual Research Project at Imperial College London, in which I built my own Python code to implement PCA (principal component analysis) and NMF (non-negative matrix factorisation) and performed dimensionality reduction to the monkey BMI tensor dataset.

## Contents
- [Data Dimensionality Reduction: *PCA and NMF to the Monkey BMI Tensor Dataset ^[1]^*](#data-dimensionality-reduction-pca-and-nmf-to-the-monkey-bmi-tensor-dataset-1)
  - [Contents](#contents)
  - [Data Dimensionality Reduction](#data-dimensionality-reduction)
    - [Problem Setting](#problem-setting)
  - [Principal Component Analysis (PCA)](#principal-component-analysis-pca)
  - [Non-negative Matrix Factorisation (NMF)](#non-negative-matrix-factorisation-nmf)
  - [The Monkey BMI Tensor Dataset](#the-monkey-bmi-tensor-dataset)
    - [PCA and NMF Results](#pca-and-nmf-results)
  - [References](#references)


## Data Dimensionality Reduction
In many fields, scientists deal with high-dimensional data, such as DNA microarrays, molecules etc. With high-dimensional data, the number of features often exceeds the number of observations, which makes calculations and further data analysis extremely difficult.

To develop more effective models that allow data analysis become computationally feasible and interpretable, we need to perform dimensionality reduction. ***Data Dimensionality Reduction** is about obtaining low-dimensional representations of the original data*.

There are several techniques to perform dimensionality reduction. The following text will illustrate **PCA** and **NMF** on the monkey BMI tensor dataset.

### Problem Setting
- Data matrix ${\bf X}_{N \times p}$
$$
{\bf X}_{N \times p} = \left(\begin{array}{ccccc}
    x_1^{(1)} & \cdots & x_i^{(1)} & \cdots & x_p^{(1)} \\
    \vdots & \ddots & \vdots & \ddots & \vdots \\
    x_1^{(n)} & \cdots & x_i^{(n)} & \cdots & x_p^{(n)} \\
    \vdots & \ddots & \vdots & \ddots & \vdots \\
    x_1^{(N)} & \cdots & x_i^{(N)} & \cdots & x_p^{(N)}
\end{array}\right) ,
$$ where rows $\{{\bf x}^{(n)}\}_{n=1}^{N}$ are data points

- Covariance matrix ${\bf C}_{\bf X}$ is defined by $${\bf C}_{\bf X} = \frac{1}{N} \sum ({\bf x}^{(n)} - \mu)({\bf x}^{(n)} - \mu)^{T} ,$$ which describes information in the data

- $k$: the number of principal components (PCs), the parameter that controls how well the data are approximated


## Principal Component Analysis (PCA)
*Principal Component Analysis (PCA)* is based on a projection onto a subspace of lower dimensions designed to retain maximal information (i.e. variance) of the original data.

PCA seeks directions of projection $\{\phi_{j}\}_{j=1}^{m}$ that capture the most variance.

The projection of ${\bf x}^{(n)}$ onto $j$^th^ PC: $$a_j^{(n)} = \phi_j^{T}{\bf x}^{(n)}$$

The approximation by the first $k$ PCs: $$\widetilde{\bf x}^{(n)} = \sum_{j=1}^m a_j^{(n)}\phi_j$$


## Non-negative Matrix Factorisation (NMF)
*Non-negative Matrix Factorisation (NMF)* approximates ${\bf X}_{N \times p}$ by the product of two non-negative matices ${\bf W}_{N \times k}$ (non-negative coefficients) and ${\bf H}_{k \times p}$ (non-negative equivalent of PCs).

The best ${\bf W}$ and ${\bf H}$ can be found by *Lee and Seung's Multiplicative Update Rule ^[2]^*:
$$
\begin{aligned}
{\bf W}_{ij} \leftarrow {\bf W}_{ij} \frac{{\sum}_{k=1}^{p} H_{jk} X_{ik} / ({\bf W}{\bf H})_{ik}}{{\sum}_{k=1}^{K}{\bf H}_{jk}}\\
{\bf H}_{jk} \leftarrow {\bf H}_{jk} \frac{{\sum}_{i=1}^{N} W_{ij} X_{ik} / ({\bf W}{\bf H})_{ik}}{{\sum}_{i=1}^{N}{\bf W}_{ij}}
\end{aligned}
$$


## The Monkey BMI Tensor Dataset
The *Monkey Brain Machine Interface (BMI) tensor dataset* has been curated from a series of experiments, where a monkey moves a cursor to one of four targets at 0, 90, 180 and -90 degrees.

The tensor is formatted as $\bf 43 \ neurons \times 200 \ timesteps \times 88 \ trials$. To study neuron assemblies with correlated patterns of activity, we analyse the data as the follwing:

1. Fix a timestep (e.g. $t = 50$) to get a matrix ${\bf X}_{88\times43}$,
2. Average over the same-angle trials to get a matrix ${\bf X}_{200\times43}$

Then we implement PCA and NMF in Python to reduce data dimensionality. In task 1, we visualise the data using projection onto 1^st^ principal component (PC1) and 2^nd^ principal component (PC2). In task 2, we use the coefficient of PC1 ($a_1$) to represent the activity of the major neuron at different timesteps.


### PCA and NMF Results
Visualised data in Python plots:

![1.1](C:/Users/liyiy/Data-Dimensionality-Reduction/2D-Representation-using-PCA-(t=50).png) ![1.2](C:/Users/liyiy/Data-Dimensionality-Reduction/2D-Representation-using-NMF-(t=50).png)

As shown if Fig 1.1, the data in PCA result cluster in four groups by angles as expected. However, there is no obvious clustering in NMF result. This will be explained afterwards.

![2.1](C:/Users/liyiy/Data-Dimensionality-Reduction/Fraction-of-Residual-Variance.png) ![2.2](C:/Users/liyiy/Data-Dimensionality-Reduction/MSE-of-NMF-(t=50).png)

![3.1](C:/Users/liyiy/Data-Dimensionality-Reduction/Coefficient-of-PC1-using-PCA.png)
![3.2](C:/Users/liyiy/Data-Dimensionality-Reduction/Magnitudes-of-Components-of-H.png)


***
To be updated.


## References

[1] T. G. Kolda. Monkey BMI Tensor Dataset. https://gitlab.com/tensors/tensor_data_monkey_bmi, 2021.
[2] Lee DD, Seung HS. Learning the parts of objects by non-negative matrix factorization. *Nature* 1999; 401: 788-791.
[3] Deisenroth MP, Faisal AA, Cheng SO. Dimensionality Reduction with Principal Component Analysis. In: *Mathematics for Machine Learning*. Cambridge University Press; 2020. p.317-347.