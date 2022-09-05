# 灰度质心法

## 公式

$x=\frac{\sum_{i=1}^{N}gray(x_i)*x_i}{\sum_{i=1}^{N}gray(x_i)}$

--------



# Steger算法

## 原理

+ 首先通过 二维Hessian 矩阵获得光条的法线方向。
  
  Hessian矩阵特征值绝对值最大的对应的特征向量就是光强变化最快的方向。

+ 然后在其法线方向上对像素灰度应用泰勒多项式展开而得到灰度分布函数。由于光条特征的灰度在其法线方向上近似为高斯分布，因而越靠近光条中心点的像素，其灰度值越大，中心为极大值。因此，对该法线方向上的灰度分布函数的二阶泰勒多项式求取极值，即可得到光条在该法线方向上的中心点坐标。
  
  [Steger算法原理详解_Jieckiee的博客-CSDN博客_steger](https://blog.csdn.net/u011725598/article/details/123744610)    

## 公式

### 1.高斯卷积核

#### 一维高斯分布：

                $G(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{x^2}{2\sigma^2}}$

#### 二维高斯分布：

                $G(x,y)=\frac{1}{2\pi\sigma^2}e^{-\frac{x^2+y^2}{2\sigma^2}}$

+ 一阶偏导

> $\frac{\partial{G}}{\partial{x}}=-\frac{1}{2\pi\sigma^4}xe^{-\frac{x^2+y^2}{2\sigma^2}}$
> $\frac{\partial{G}}{\partial{y}}=-\frac{1}{\sqrt{2\pi}\sigma^4}ye^{-\frac{x^2+y^2}{2\sigma^2}}$

+ 二阶偏导

> $\frac{\partial^2{G}}{\partial{x}^2}=(-\frac{1}{2\pi\sigma^4})(1-\frac{x^2}{\sigma^2})e^{-\frac{x^2+y^2}{2\sigma^2}}$ 
> $\frac{\partial^2{G}}{\partial{x}^2}=(-\frac{1}{2\pi\sigma^4})(1-\frac{y^2}{\sigma^2})e^{-\frac{x^2+y^2}{2\sigma^2}}$ 
> $\frac{\partial^2{G}}{\partial{x}\partial{y}}=(\frac{xy}{2\pi\sigma^6})e^{-\frac{x^2+y^2}{2\sigma^2}}$

### 2.Hessian矩阵

$H(x,y)=\begin{bmatrix}
 r_{xx} & r_{xy} \\
 r_{xy} & r_{yy} 
 \end{bmatrix}$

其中$r_{xx}=\frac{\partial^2{G}}{\partial{x}^2}$

        $r_{yy}=\frac{\partial^2{G}}{\partial{y}^2}$

        $r_{xy}=\frac{\partial^2{G}}{\partial{x}\partial{y}}$

### 3.特征值特征向量

$\lambda=\frac{r_{xx}+r_{yy}-\sqrt{(r_{xx}-r_{yy})^2+4r_{xy}^2}}{2}$

$(n_x,n_y)=(\frac{r_{xy}}{\sqrt{r_{xy}^2+(r_{xx-\lambda})^2}} , \frac{\lambda-r_{xx}}{\sqrt{r_{xy}^2+(r_{xx-\lambda})^2}})$

### 4.验证$(p_x,p_y) \in [-0.5,0.5] \times [-0.5,0.5]$

$(p_x,p_y) =(\frac{r_{xy}[r_{xy}r_x+(\lambda-r_{xx})r_y]} {(r_{xx}-\lambda)[r_{xy}^2+(\lambda-r_{xx})r_{yy}]-\lambda r_{xy}^2} , \frac{(\lambda-r_{xx})[r_{xy}r_x+(\lambda-r_{xx})r_y]} {(r_{xx}-\lambda)[r_{xy}^2+(\lambda-r_{xx})r_{yy}]-\lambda r_{xy}^2})$
