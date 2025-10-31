# Dynamic Mode Decomposition for Financial Trading Strategies

A data-driven analysis applying Dynamic Mode Decomposition (DMD) to model complex dynamical systems for financial price prediction

---
### 🧠 Introduction & Motivation

Dynamic Mode Decomposition (DMD) is a powerful, data-driven "equation-free" algorithm that addresses this challenge. It analyzes a time-series of "snapshots" from a system and decomposes its complex spatio-temporal behavior into a set of low-rank, coherent structures called DMD modes. Each mode is associated with a specific oscillation frequency and a growth or decay rate (an eigenvalue). This allows us to build a simple, linear model that captures the dominant evolutionary patterns of the system.

📈 Financial Price Prediction: By treating the stock market as a complex dynamical system, we use DMD to identify dominant "growing modes" to predict price movements and inform trading strategies.

---

### 📊 2. Dataset Description

- Source: National Stock Exchange (NSE500 Index)

- Scope: Minute-wise stock prices of selected companies across sectors

- Sampling Frequency: 1 minute

- Training / Testing Split: 80/20


| Parameter  | Description              | Example |
| ---------- | ------------------------ | ------- |
| N          | Number of companies      | 10      |
| M          | Number of time snapshots | 2000    |
| Δt         | Sampling interval        | 1 min   |
| Observable | Stock price at time *t*  | 245.5 ₹ |

---
## ⚙️ 3. Methodology

We treat the stock market as a discrete-time dynamical system evolving as:

$$
x_{k+1} = F(x_k)
$$

where \( x_k \in \mathbb{R}^n \) is the system state (vector of stock prices at time \( k \)) and \( F \) is an unknown nonlinear mapping.  
DMD approximates this system by finding a best-fit linear operator \( A \) such that:

$$
x_{k+1} \approx A x_k
$$

The goal is to find \( A \) that minimizes the least-squares error between actual and predicted states.

  <p align="center">
  <img src="image (35).png" alt="" width="400"/>
</p>  

---

### 🧩 3.1 Matrix Formulation

Snapshots of the system are organized as:

$$
X = [x_1, x_2, \dots, x_{m-1}]
$$

$$
X' = [x_2, x_3, \dots, x_m]
$$

We seek \( A \) satisfying:

$$
X' = A X
$$

The optimal approximation is given by:

$$
A = X' X^{+}
$$

where \( X^{+} \) is the **Moore–Penrose pseudoinverse** of \( X \).

---

### 🧮 3.2 Dimensionality Reduction via SVD

To mitigate noise and reduce dimensionality, we compute the truncated **Singular Value Decomposition (SVD)**:

$$
X = U \Sigma V^{T}
$$

where:

$$
\begin{aligned}
U &\in \mathbb{C}^{n \times r} &&\text{— POD modes} \\
\Sigma &\in \mathbb{C}^{r \times r} &&\text{— singular values} \\
V &\in \mathbb{C}^{m \times r} &&\text{— temporal coefficients}
\end{aligned}
$$

The reduced linear operator becomes:

$$
\tilde{A} = U^{T} X' V \Sigma^{-1}
$$

---

### 🧠 3.3 Eigen Decomposition

We compute eigenvalues and eigenvectors of \( \tilde{A} \):

$$
\tilde{A} W = W \Lambda
$$

Then, the **exact DMD modes** are:

$$
\Phi = X' V \Sigma^{-1} W
$$

Each mode \( \phi_k\) evolves in time as:

$$
x(t) \approx \sum_{k=1}^{r} b_k \phi_k e^{\omega_k t}
$$

where:

$$
\omega_k = \frac{\ln(\lambda_k)}{\Delta t}
$$

and \( \lambda_k \) are the **DMD eigenvalues**.

---

### 💹 3.4 Interpretation for Financial Prediction

If:

$$
\Re(\lambda_k) > 1
$$

→ **Growing mode** → increasing stock value  

If:

$$
\Re(\lambda_k) < 1
$$

→ **Decaying mode** → decreasing stock value  

---

## 🧰 3.5 Implementation Overview

> **Implemented using:** <span style="color:#4CAF50">`PyDMD`</span> — Python Dynamic Mode Decomposition library  
> **Dataset:** <span style="color:#00BCD4">NSE500 stock price time series</span>

### 🖥️ Graphical User Interface (GUI)

Built using <span style="color:#FF9800">Streamlit</span> with the following features:

- 📈 **Stock Price Prediction** — Predicts future stock movement using DMD reconstruction.  
- 💹 **Stock Recommendation** — Ranks stocks by *growing eigenvalues* (positive real part).  
- 🎨 **Visualization Tools** — Interactive plots for:
  - DMD Modes  
  - Eigenvalue Spectra  
  - Temporal Dynamics  

<p align="left">
  <img src="image (38).png" alt="" width="700"/>
</p>

---

## 📊 4. Results & Analysis

| 🧮 **Metric** | 📏 **Value** | 📖 **Interpretation** |
|---------------|--------------|-----------------------|
| **MAE** | `15.5299` | Average absolute prediction error |
| **MAPE** | `1.94%` | <span style="color:#4CAF50">Excellent short-term forecasting accuracy (less than 2% relative error)</span> |


---

### 🧩 Key Observations

- 🔹 **Eigenvalue Analysis:** Revealed clear *growing* and *decaying* modes in the stock time series.  
- 🔹 **Accuracy:** DMD effectively captured *short-term market dynamics* with minimal computational cost.  
- 🔹 **Visualization Insight:** Stem plots of DMD modes showed distinct *temporal oscillations* and *trend separations*.

  <p align="left">
  <img src="image (36).png" alt="" width="800"/>
</p>  

---

> ⚡ **Insight:**  
> The model demonstrates that *Dynamic Mode Decomposition* is not only effective for physical systems but also a powerful analytical tool in **financial forecasting** — offering interpretability, speed, and accuracy.


The **growth rate** (real part of eigenvalue) represents trend strength, while the **complex part** encodes cyclic behavior.  

This allows ranking stocks by their **dominant growing modes**, forming the basis for **automated trading recommendations**.
