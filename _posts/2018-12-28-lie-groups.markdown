---
layout: post
title:  "Lie Groups and Rigid Body Kinematics"
permalink: /lie-groups/
excerpt: "SO(2), SO(3), SE(2), SE(3), Lie algebras"
mathjax: true
date:   2018-12-28 11:00:00
mathjax: true

---
Table of Contents:
- [Why do we need Lie Groups?](#whyliegroups)
- [Lie Groups](#liegroups)
- [SO(N)](#son)
- [SO(2)](#so2)
- [SO(3)](#so3)
- [SE(2)](#se2)
- [SE(3)](#se3)
- [Conjugation in Group Theory](#conjugation)
- [The Lie Algebra](#lie-algebra)

<a name='whyliegroups'></a>

## Why do we need Lie Groups?

Rigid bodies have a state which consists of position and orientation. When sensors are placed on a rigid body (e.g. a robot), they provide measurements in the body frame. Suppose we wish to take a measurement $$y_b$$ from the body frame and move it to the world frame, $$y_w$$. We can do this via left multiplication with a transformation matrix $${}^{w}T_{b}$$, a member of the matrix Lie groups, that transports the point from one space to another space:

$$
y_w = {}^{w}T_{b} y_b
$$

<a name='liegroups'></a>

## Lie Groups

When we are working with pure rotations, we work with Special Orthogonal groups, $$SO(\cdot)$$. When we are working with a rotation and a translation together, we work with Special Euclidean groups $$SE(\cdot)$$.

Lie Groups are unique because they are **both a group and a manifold**. They are continuous manifolds in high-dimensional spaces, and have a group structure. I'll describe them in more detail below.

<a name='son'></a>

### SO(N)

Membership in the Special Orthogonal Group $$SO(N)$$ requires two matrix properties:
-	$$R^TR = I $$
- 	$$ \mbox{det}(R) = +1 $$

This gives us a very helpful property: $$R^{-1} = R^T$$, so the matrix inverse is as simple as taking the transpose.  We will generally work with $$SO(N)$$, where $$N=2,3$$, meaning the matrices are rotation matrices $$R \in \mathbf{R}^{2 \times 2}$$ or $$R \in \mathbf{R}^{3 \times 3}$$.

These rotation matrices $$R$$ are not commutative.

<a name='so2'></a>

### SO(2)

$$SO(2)$$ is a 1D manifold living in a 2D Euclidean space, e.g. moving around a circle.  We will be stuck with singularities if we use 2 numbers to parameterize it, which would mean kinematics break down at certain orientations.

$$SO(2)$$ is the space of orthogonal matrices that corresponds to rotations in the plane.

**A simple example**:
Let's move from the body frame $$b$$ to a target frame $$t$$:

$$
P_t = {}^tR_b(\theta) P_b
$$

$$
\begin{bmatrix}
5 \mbox{ cos} (\theta) \\
5 \mbox{ sin} (\theta)
\end{bmatrix} = \begin{bmatrix} cos(\theta) & -sin(\theta) \\ sin(\theta) & cos(\theta) \end{bmatrix} * \begin{bmatrix} 5 \\ 0 \end{bmatrix}
$$

As described in [3], another way to think of this is to consider that a robot can be rotated counterclockwise by some angle $$\theta \in [0,2 \pi)$$ by mapping every $$(x,y)$$ as:

$$
(x, y) \rightarrow (x \mbox{ cos } \theta − y \mbox{ sin } \theta, x \mbox{ sin } \theta + y \mbox{ cos } \theta).
$$

<a name='so3'></a>

### SO(3)

There are several well-known parameterizations of $$R \in SO(3)$$:
- (1.) $$R \in \mathbf{R}^{3 \times 3}$$ full rotation matrix, 9 parameters -- there must be 6 constraints
- (2.) Euler angles, e.g. $$(\phi, \theta, \psi)$$, so 3 parameters
- (3.) Angle-Axis parameters $$(\vec{a}, \phi)$$, which is 4 parameters and 1 constraint (unit length)
- (4.) Quaternions ($$q_0,q_1,q_2,q_3)$$, 4 parameters and 1 constraint (unit length)

There are only 3 degrees of freedom in describing a rotation. But this object doesn't live in 3D space. It is a 3D manifold, embedded in a 4-D Euclidean space.

Parameterizations 1,3,4 are overconstrained, meaning they employ more parameters than we strictly need. With overparameterized representations, we have to do extra work to make sure we satisfy the constraints of the representation.

As it turns out $$SO(3)$$ cannot be parameterized by only 3 parameters in a non-singular way.


**Euler Angles**
One parameterization of $$SO(3)$$ is to imagine three successive rotations around different axes. The Euler angles encapsulate yaw-pitch-roll: first, a rotation about the z-axis (yaw, $$\psi$$). Then, a rotation about the pitch axis by $$\theta$$ (via right-hand rule), and finally we perform a roll by $$\phi$$.

<div class="fig figcenter fighighlight">
  <img src="/assets/euler_angles.jpg" width="65%">
  <div class="figcaption">
    The Euler angles.
  </div>
</div>

The sequence of successive rotations is encapsulated in $${}^{w}R_b$$:

$$
{}^{w}R_b = R_R(\phi) R_P(\theta) R_Y (\psi)
$$

As outlined in [3], these successive rotations by $$(\phi, \theta, \psi)$$ are defined by:

$$
R_{yaw} = R_y (\psi) = \begin{bmatrix}
\mbox{cos} \psi & -\mbox{sin} \psi  & 0 \\
\mbox{sin} \psi & \mbox{cos} \psi & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

$$
R_{pitch} = R_p (\theta) = \begin{bmatrix}
\mbox{cos} \theta & 0 & \mbox{sin} \theta \\
 0 & 1 & 0 \\
 -\mbox{sin} \theta & 0 & \mbox{cos} \theta \\
\end{bmatrix}
$$

$$
R_{roll} = R_R (\phi) = \begin{bmatrix}
1 & 0 & 0 \\
0 & \mbox{cos} \phi & -\mbox{sin} \phi \\
0 & \mbox{sin} \phi &  \mbox{cos} \phi \\
\end{bmatrix}
$$

You have probably noticed that each rotation matrix $$\in \mathbf{R}^{3 \times 3}$$ above is a simple extension of the 2D rotation matrix from $$SO(2)$$. For example, the yaw matrix $$R_{yaw}$$ performs a 2D rotation with respect to the $$x$$ and $$y$$ coordinates while leaving the $$z$$ coordinate unchanged [3].

<a name='se2'></a>

### SE(2)

The real space $$SE(2)$$ are $$3 \times 3$$ matrices, moving a point in homogenous coordinates to a new frame. It is important to remember that this represents a rotation followed by a translation (not the other way around).

$$
T = \begin{bmatrix} x_w \\ y_w \\ 1 \end{bmatrix} = \begin{bmatrix}
 R_{2 \times 2}& & t_{2 \times 1}  \\
& \ddots & \vdots  \\
0 & 0 & 1
\end{bmatrix} * \begin{bmatrix} x_b \\ y_b \\ 1 \end{bmatrix} 
$$

By adding an extra dimension to the input points and transformation matrix $$T$$ the translational part of the transformation is absorbed [3].

<a name='se3'></a>

### SE(3)

The real space $$SE(3)$$ are $$4 \times 4$$ matrices, the real space resembles:

$$
\begin{bmatrix}
& & & \\
& R_{3 \times 3} & &  t_{3 \times 1} \\
& & & \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

<a name='conjugation'></a>

## Conjugation in Group Theory

Surprisingly, movement in $$SE(2)$$ can always be achieved by moving somewhere, making a rotation there, and then moving back. 

$$
SE(2) = (\cdot) SO(2) (\cdot)
$$

If we move to a point by vector movement $$p$$, essentially we perform: $$B-p$$, then go back with $$p + \dots $$.

$$
B^{\prime} = \begin{bmatrix}
R & t \\
0 & 1
\end{bmatrix}_B = \begin{bmatrix}
I & p \\
0 & 1
\end{bmatrix} \begin{bmatrix}
R & 0 \\
0 & 1
\end{bmatrix} \begin{bmatrix}
I & -p \\
0 & 1
\end{bmatrix}_B
$$


<a name='lie-algebra'></a>

## Connecting Spatial Coordinates and Spatial Velocities: The Lie Algebra

The Lie Algebra maps spatial coordinates to spatial velocity.

In the case of $$SO(3)$$, the Lie Algebra is the space of skew-symmetric matrices



Rigid Body Kinematics: we want a differential equation (ODE) that links $$\vec{\omega}$$ and $${}^{^w}R_b$$


in particular,
$$  {}^{^w} \dot{R}_r = f({}^{^w}R_b, \vec{\omega}) $$

$$
\begin{aligned}
\frac{ \partial }{\partial t}(R^R = I)
 \\
\dot{R}^TR + R^T \dot{R} = 0 \\
\dot{R}^TR = -R^T \dot{R} 
\end{aligned}
$$

we define $$\Omega = R^T \dot{R} $$

$$
\Omega^T = (R^T \dot{R} )^T = \dot{R}^TR  = -R^T \dot{R}  = -\Omega
$$


Skew-symmetric! $\Omega^T = -\Omega$


In fact, 

$$
\Omega = \begin{bmatrix}
0 & -\omega_z & \omega_y \\
\omega_z & 0 & -\omega_x \\
-\omega_y & \omega_x & 0
\end{bmatrix}
$$

where $$\vec{\omega} = \begin{bmatrix} \omega_x \\ \omega_y \\ \omega_z \end{bmatrix}$$ is the angular velocity

Notation! the "hat" map

$$
\bar{\omega}^{\hat{}} =\Omega 
$$

the "v" map

$$
\Omega^{v} = \bar{\omega} 
$$


So we have

$$
\begin{aligned}
\dot{R} = R \Omega \\
\Omega = \bar{\omega}^{\hat{}}
\end{aligned}
$$

\item In terms of frames, we have Poisson's kinematic equation

\item The intrinsic equation is (where $$\vec{\omega}_b$$ in the body frame is $\Omega_b$):

$$
{}^{^w}\dot{R}_b =  {}^{^w}R_b \Omega_b
$$


The "strap-down" equation (extrinsic)

$$
{}^{^b}\dot{R}_w =  - \Omega_b {}^{^b}R_w
$$

Take vector on aircraft, put it into the coordinate frame of the world



$$
{}_{^w}R_b = \begin{bmatrix} | & | & | \\ {}^{^w} \hat{x}_b & {}^{^w}\hat{y}_b & {}^{^w} \hat{z}_b \\ | & | & |  \end{bmatrix}
$$


$$
\Omega \vec{v}_b = \omega \times \vec{v}_b
$$

Can we use this for filtering? Can we use $$\dot{R} = R \Omega $$ directly in an EKF, UKF
We'd have to turn it into discrete-time 
Naively, you might do the first order Euler approximation:

$$
\frac{ R_{t + \delta_t} -R_t}{\delta t} \approx R_t \Omega_t
$$

This does not work!

You won't maintain orthogonality! The defining property of $$SO(3)$$ was orthogonality, so

$$
R_{t + \delta t} \not\in SO(3)
$$

and pretty soon

$$
\begin{aligned}
R_{t + \delta t}^T R_{t + \delta t} \neq I \\
\mbox{det }(R_{t+\delta t}) \neq +1
\end{aligned}
$$

The 3x3 matrix will not be recognizable of a rotation

## References

[1] Frank Dellaert. Lecture Presentations of MM 8803: Mobile Manipulation, taught at the Georgia Institute of Technology, Fall 2018.

[2] Mac Schwager. Lecture Presentations of AA 273: State Estimation and Filtering for Aerospace Systems, taught at Stanford University in April-June 2018. 

[3] Steven M. LaValle. *Planning Algorithms*. Cambridge University Press, 2006, New York, NY, USA. 









**Rigid Body Kinematics and Filtering**

Kinematics with Euler Angles



\begin{equation}
\dot{R} = f(R, \vec{w}), (\dot{\phi}, \dot{\theta}, \dot{\psi}) = f(\phi, \theta, \psi, \vec{\omega})
\end{equation}

\item 

\begin{equation}
\vec{\omega} = \begin{bmatrix} w_x \\ w_y \\ w_z \end{bmatrix} = \begin{bmatrix} 1 & 0 & -\mbox{sin} \theta \\ 0 & \mbox{cos} \phi & \mbox{sin} \phi \mbox{cos} \phi \\ 0 & -\mbox{sin} \phi & \mbox{cos} \phi \mbox{cos} \theta \end{bmatrix} \begin{bmatrix} \dot{\phi} \\ \dot{\theta} \\ \dot{\psi} \end{bmatrix}
\end{equation}

\item We want to invert this matrix and solve for $\begin{bmatrix} \dot{\phi} \\ \dot{\theta} \\ \dot{\psi} \end{bmatrix}$