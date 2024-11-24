---
title: "Space Flight Trajectory prediction via Kalman-Filter"
excerpt: "Project presented for the Modeling Simulation and Optimization FAU SummerSemester 2024 course<br/><img src='/images/animation_ht_kf_frame3.png'>"
collection: portfolio
---


<img src="images/sim_KF_precomp_velocities_dt14.gif" alt="KF simulation"  width="250" />

This project was presented for the Modeling Simulation and Optimization FAU Summer Semester 2024 master's course. 

The group modeled a space flight simulation between Saturn and Jupiter using the Hohmman Transfer theory and 
tools such as the Kalman-Filter model, to estimate trajectory states.


## Simulation: Trajectory Prediction, Kalman-Filter
<!-- \author{Martin Betz, \underline{Pedro Blöss}, Temucin Cil, Jannik Hausladen, Pietro Italiano} -->

### Motivation
- Why use the Kalman-Filter?
- Are the dynamics of our system (e.g. velocities) being considered?
- How do we account for noise and perturbations to our trajectory?
- How does NASA use the Kalman-Filter?
- Given a theoretical trajectory, does our spacecraft actually follow the trajectory?
- Are our assumptions viable?

### How does NASA use the Kalman-Filter?
<!-- \parencite{NASA_Kalman_Filter} \parencite{how_NASA_used_the_KF_Apollo} -->
<!-- \parencite{helmick2004path} -->

Applications:
-  Navigation and guidance systems: real-time updates, combining data from inertial navigation systems and other sensors.
-  Attitude estimation: used on the International Space Station (ISS), to fuse data from star trackers, gyroscopes, and other sensors, for the station orientation.

Examples:
- Apollo program. 
<!-- %\parencite{how_NASA_used_the_KF_Apollo} -->
- Mars Rover.
<!-- %\parencite{helmick2004path} -->

### Kalman-Filter: Predicting the Next Position
<!-- \parencite{KALMAN} \parencite{how_NASA_used_the_KF_Apollo} -->

Given observed positions, and taking into account the dynamics $(x, \dot{x}, \ddot{x})$, \textbf{we want a method that can accurately estimate the next position}, considering perturbations in our system.

### Kalman-Filter: Embedding Uncertainty
<!-- \parencite{KALMAN} \parencite{sheldon_applied_prob} -->
- $Q$: Process noise covariance, uncertainty about the system \textbf{dynamics}.
- $R$: \textbf{Measurement} noise covariance.
- $P$: \textbf{Initial estimate} covariance, uncertainty of initial estimate.



### Kalman-Filter: Notation
<!-- \parencite{KALMAN} -->

\begin{align}
    \underbrace{\hat{x}_{k-1|k-1}}_{\substack{\text{measurement} \\ \text{at } k-1}}
    \underbrace{\to}_{\substack{
    \text{state update} \\ \text{(velocities, position, etc)}
    }}
    \hat{x}_{k|k-1} 
    \underbrace{\to}_{
        \substack{
        \text{incorporate noise}
        \\
        \text{updated covariance}
        }
    } 
    \underbrace{\hat{x}_{k|k}}_{
    \substack{
    \text{measurement} \\ \text{at } k
    }}
\end{align}

### Kalman-Filter: Estimation from Observed Measurements
To simulate observed measurements, we take the Hohmann transfer $x_{h}, y_{h}$ vectors of positions and add a Gaussian noise with arbitrary variance $\sigma^2$:

\begin{align}
    x_{\text{obs}} = x_{\text{Hohmann}} + n, \quad n \in \mathcal{N}(0, \sigma^2) \\
    y_{\text{obs}} = y_{\text{Hohmann}} + n, \quad n \in \mathcal{N}(0, \sigma^2) 
\end{align}

### How does the Kalman-Filter takes into account the state?
Example with nonconstant velocity, and constant acceleration:

\begin{align}
\begin{cases}
    \hat{x}_{k+1,k} = \hat{x}_{k,k} + \hat{\dot{x}}_{k,k} \Delta t + \frac{1}{2} \hat{\ddot{x}}_{k,k} \Delta t^2 
    \\
    \hat{\dot{x}}_{k+1, k} = \hat{\dot{x}}_{k,k} + \hat{\ddot{x}}_{k,k} \Delta t \\
    \hat{\ddot{x}}_{k+1, k} = \hat{\ddot{x}}_{k, k} \\
\end{cases}.
\end{align}

Then, the state update is:

\begin{align}
\begin{bmatrix}
    \hat{x}_{k+1, k} \\
    \hat{\dot{x}}_{k+1, k} \\
    \hat{\ddot{x}}_{k+1, k} \\
    \hat{y}_{k+1, k} \\
    \hat{\dot{y}}_{k+1, k} \\
    \hat{\ddot{y}}_{k+1, k} 
\end{bmatrix}
= 
\begin{bmatrix}
1 & \Delta t & 0.5 \Delta t^2 & 0 & 0 & 0 \\
0 & 1 & \Delta t & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 1 & \Delta t & 0.5 \Delta t^2\\
0 & 0 & 0 & 0 & 1 & \Delta t \\
0 & 0 & 0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
    \hat{x}_{k, k} \\
    \hat{\dot{x}}_{k, k} \\
    \hat{\ddot{x}}_{k, k} \\
    \hat{y}_{k, k} \\
    \hat{\dot{y}}_{k, k} \\
    \hat{\ddot{y}}_{k, k} \\
\end{bmatrix}.
\end{align}

### Kalman-Filter: Including Hohmann Transfer Positions and Velocities

- Initially, the Kalman-Filter considers constant acceleration. Its non-constant acceleration version is the extended Kalman-Filter.
-  In our case, we already have the **pre-computed velocities for the Hohmann transfer**, which we want to input into the Kalman-Filter state update
-  There are a couple of alternatives:
  - Design an appropriate state transition matrix $A$.
  - Input our **pre-computed velocities* in each update.
  - Estimate accelerations, and include them in the state update.


### Kalman-Filter: State Update
<!-- \parencite{NASA_Kalman_Filter}  \parencite{WelchB95} -->

The state update step updates the dynamics from $k-1|k-1$ to $k|k-1$:

\begin{align}
\hat{x}_{k|k-1} &= \underbrace{A}_{\substack{
\text{state update} \\
\text{update }x, \dot{x}, \ddot{x}
}} \hat{x}_{k-1|k-1},
\\
\underbrace{P_{k|k-1}}_{\substack{
\text{update covariance}\\
\text{uncertainty of estimate}
}} &= A P_{k-1|k-1}A^T + \underbrace{Q}_{\substack{
\text{add process noise} \\
\text{uncertainty of dynamics}
}}.
\end{align}

### Kalman-Filter: Measurement Update
<!--     \parencite{NASA_Kalman_Filter}
    \parencite{WelchB95} -->

The measurement update step updates the measures from $k|k-1$ to $k|k$:

\begin{align}
    \underbrace{K_k}_{\substack{
        \text{Kalman gain}
        }} 
    &= P_{k|k-1} H^T (H P_{k|k-1} H^T 
    + \underbrace{R}_{\substack{
        \text{measurement} \\
        \text{noise}
        }}
    )^{-1},
    \\
    \hat{x}_{k|k} &= \hat{x}_{k|k-1}  + K_k (\underbrace{z_k}_{\substack{
        \text{measurement}\\
        \text{vector}
        }} 
    - H \hat{x}_{k|k-1}),
    \\
    P_{k|k} &= (\mathbb{I} - K_k H) P_{k|k-1}.
\end{align}

where $z_k = (x_k^{\text{(obs)}}, y_k^{\text{(obs)}})$.

### Kalman-Filter: Using Pre-Computed Values

 Using pre-computed velocities $\{ v_k \}_k$:
 
\begin{align}
\begin{cases}
    \hat{x}_{k+1,k} = \hat{x}_{k,k} + \color{red}{v_k}  \color{black} \Delta t 
    \\
    \hat{\dot{x}}_{k+1, k} = \color{red}{v_{k+1}}  \color{black} \\
    \text{ same for } (y, \dot{y})
\end{cases}.
\end{align}

Then, the state update is:

\begin{align}
\begin{bmatrix}
    \hat{x}_{k+1, k} \\
    \hat{y}_{k+1, k} \\
    \hat{\dot{x}}_{k+1, k} \\
    \hat{\dot{y}}_{k+1, k} \\
\end{bmatrix}
= 
\begin{bmatrix}
1 & 0 & \Delta t & 0 \\
0 & 1 & 0 & \Delta t \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
    \hat{x}_{k, k} \\
    \hat{y}_{k, k} \\
    \color{red}{v^{(x)}_{k+1, k}}  \color{black} \\
    \color{red}{v^{(y)}_{k+1, k}}  \color{black} \\
\end{bmatrix}.
\end{align}

### Estimating the Actual Kalman-Filter Velocity

<!-- \begin{figure}
    \centering
    \includegraphics[scale=0.7]{images/kf_vel_approx_photo.png}
    \caption{Illustration of KF velocity estimate.}
    % \label{fig:enter-label}
\end{figure} -->
<img src="images/kf_vel_approx_photo.png" width="250" />

-  Given the KF estimated positions, we can \textbf{approximate the velocities}, with the simple derivative.
-  It is assumed that the acceleration is incorporated in each velocity step.

### Estimating the Actual Kalman-Filter Velocity
<!-- \begin{figure}
    \centering
    \includegraphics[scale=0.9]{images/kf_ht_velocities.png}
    \caption{Kalman-Filter estimated velocities and Hohmann Transfer velocities}
    % \label{fig:kf_ht_vel}
\end{figure} -->
<img src="images/kf_ht_velocities.png" width="250" />


The presence of noise is noticeable in the velocity estimates.

### Estimating the actual Kalman-Filter velocity
<!-- \begin{figure}
    \centering
    \includegraphics[scale=0.9]{images/kf_vel_reg.png}
    \caption{Kalman-Filter estimated velocities and Hohmann Transfer velocities, with KF linear regression}
    % \label{fig:kf_ht_vel_and_reg}
\end{figure} -->
<img src="images/kf_vel_reg.png" width="250" />

### Estimated KF velocities error
<!-- }{\parencite{celebi2011euclidean} -->
<!-- \begin{figure}
    \centering
    \includegraphics[scale=0.95]{images/kf_pos_approx_photo.png}
    \includegraphics[scale=0.95]{images/vel_error_dist.png}
    \caption{Distribution of error estimates (Euclidean norm).}
    % \label{fig:kf_ht_vel_residuals}
\end{figure} -->
<img src="images/kf_pos_approx_photo.png" width="250" />
<img src="images/vel_error_dist.png" width="250" />


### Estimated KF Velocities Error
<!--     \parencite{normality_test1} \parencite{normality_test2}} -->
<!-- \begin{figure}
    \centering
    \includegraphics[scale=0.9]{images/qqplot_pos_error.png}
    \includegraphics[scale=0.9]{images/qqplot_vel_error.png}
    \caption{Q-Q plot of error distributions.}
    % \label{fig:kf_ht_vel_residuals_qqplot}
\end{figure} -->
<img src="images/qqplot_pos_error.png" width="250" />
<img src="images/qqplot_vel_error.png" width="250" />

-  D'agostino and Pearson normality test results for the position errors: $\text{Test Statistic}=1.642$, and $p=0.440$.
-  For a significance value of $\alpha=0.05$, we fail to reject H0, so we infer normality, since $p>\alpha$.

## Kalman-Filter: Simulation Parameters
<!-- \parencite{sheldon_applied_prob} -->

\begin{align}
A = \begin{bmatrix}
1 & 0 & \Delta t & 0 \\
0 & 1 & 0 & \Delta t \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}, 
& \quad \quad 
H = \begin{bmatrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
\end{bmatrix},
& 
\hat{x} = \begin{bmatrix}
    x_k \\ y_k \\ \dot{x}_k \\ \dot{y}_k
\end{bmatrix} 
= \begin{bmatrix}
    x_k \\ y_k \\ v^{(x)}_k \\ v^{(y)}_k
\end{bmatrix} 
\end{align}
\begin{align*}
    Q = \sigma_Q^2 \cdot \mathbb{I}_{4 \times 4}, 
    & \quad \text{(process noise, dynamics uncertainty)}\\
    R = \sigma_R^2 \cdot \mathbb{I}_{2 \times 2}, 
    & \quad\text{(measurement noise)}\\
    P = \sigma_P^2 \cdot \mathbb{I}_{4 \times 4}, 
    & \quad\text{(estimates uncertainty)}
\end{align*}
with $\sigma_Q^2 = 10^{-3}$, $\sigma_R^2 = 0.5$, and $\sigma_P^2 = 500$.

