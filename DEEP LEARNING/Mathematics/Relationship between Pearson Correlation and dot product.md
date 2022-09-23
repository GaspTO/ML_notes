[Blog where this is explained](https://matthew-brett.github.io/teaching/correlation_projection.html#correlation-and-projection "Permalink to this headline")

Here we phrase the Pearson product-moment correlation coefficient in terms of vectors.
Say we have two vectors of $n$ values:
$$
\begin{gathered}
\vec{x}=\left[x_{1}, x_{2}, \ldots, x_{n}\right] \\
\vec{y}=\left[y_{1}, y_{2}, \ldots, y_{n}\right]
\end{gathered}
$$
Write the mean of the values in $\vec{x}$ as $\bar{x}$ :
$$
\bar{x}=\frac{1}{n} \sum_{i=1}^{n} x_{i}
$$
Define two new vectors, $\overrightarrow{x^{c}}, \overrightarrow{y^{c}}$ that contain the values in $\vec{x}, \vec{y}$ with their respective means subtracted:
$$
\begin{aligned}
&\overrightarrow{x^{c}}=\left[x_{1}-\bar{x}, x_{2}-\bar{x}, \ldots, x_{n}-\bar{x}\right] \\
&\overrightarrow{y^{c}}=\left[y_{1}-\bar{y}, y_{2}-\bar{y}, \ldots, y_{n}-\bar{y}\right]
\end{aligned}
$$
Define the sample variance of $\vec{x}$ as the mean of the squared deviations from the mean:
$$
v_{x}=\frac{1}{n} \sum_{i=1}^{n}\left(x_{i}-\bar{x}\right)^{2}
$$
The sample standard deviation of $\vec{x}$ :
$$
s_{x}=\sqrt{v_{x}}
$$

![[Pasted image 20220816133504.png]]