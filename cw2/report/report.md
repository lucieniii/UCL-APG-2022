# COMP0119 Assignment2

**Name** Lucien Li

**Student No.** 22081006

**Platform Information**

```text
OS: 				macOS Ventura
Processor: 			M2 Pro Simulating Intel by Rosetta
Environment:		Python 3.9.15
Python Packages:	trimesh, open3d, numpy, scipy, matplotlib, cv2, tqdm
```

## Uniform Laplace

**Test mesh: **`meshes/curvatures/lilium_s.obj`

| ![1-1](imgs/1-1.png) | ![1-2](imgs/1-2.png) |
| -------------------- | -------------------- |

**Legend: **

![1](plots/1.png)

### Mean curvature

Mesh file saved in `res/curvature/H_lilium_s.obj`.

| ![1-3](imgs/1-3.png) | ![1-4](imgs/1-4.png) |
| -------------------- | -------------------- |

### Gaussian curvature

Mesh file saved in `res/curvature/K_lilium_s.obj`.

| ![1-5](imgs/1-5.png) | ![1-6](imgs/1-6.png) |
| -------------------- | -------------------- |

### Comparison and discussion

The curvature shown by the mean curvature has no obvious local features as the mesh used for the test is not very smooth. This is very similar to the behaviour of noise. In contrast, the Gaussian curvature shows distinct local features and the calculated curvature is somewhat tolerant of noise.

## First and Second Fundamental forms

Please see my notebook for the process of calculation.

Take unit vectors $\overline{\mathbf{t}}$ on the tangent plane at $(a, 0, 0)$, where 
$$
\overline{\mathbf{t}}=(u_t, v_t), u_t=\cos(\theta), v_t=\sin(\theta), \theta\in[0,2\pi]
$$
. The computed curvatures $\kappa(\overline{\mathbf{t}})$ are shown in the plot below.

<img src="plots/2.png" alt="2" style="zoom:72%;" />

The curvature consists of a maximum value at $\theta=\frac{\pi}{2},\frac{3\pi}{2}$ and a minimum value at $\theta=0,\pi$, in line with our perception.

## 3 Non-uniform (Discrete Laplace-Beltrami)

**Legend: **

![1](plots/1.png)

**Test mesh: **`meshes/curvatures/lilium_s.obj`

**Cot Mean curvature** Mesh file saved in `res/curvature/H_cot_lilium_s.obj`.

|                   |      **Origin**      |  **Mean curvature**  | **Gaussian curvature** | **Cot Mean curvature** |
| :---------------: | :------------------: | :------------------: | :--------------------: | :--------------------: |
| **Perspective 1** | ![1-1](imgs/1-1.png) | ![1-3](imgs/1-3.png) |  ![1-5](imgs/1-5.png)  |  ![3-1](imgs/3-1.png)  |
| **Perspective 2** | ![1-2](imgs/1-2.png) | ![1-4](imgs/1-4.png) |  ![1-6](imgs/1-6.png)  |  ![3-2](imgs/3-2.png)  |

**Test mesh: **`meshes/curvatures/plane.obj`

**Mean curvature** Mesh file saved in `res/curvature/H_plane.obj`.

**Gaussian curvature** Mesh file saved in `res/curvature/K_plane.obj`.

**Cot Mean curvature** Mesh file saved in `res/curvature/H_cot_plane.obj`.

|                   |      **Origin**      |   **Mean curvature**   | **Gaussian curvature** |  **Cot Mean curvature**  |
| :---------------: | :------------------: | :--------------------: | :--------------------: | :----------------------: |
| **Perspective 1** | ![3-3](imgs/3-3.png) | ![3-5](imgs/3-6-H.png) | ![3-7](imgs/3-7-K.png) | ![3-9](imgs/3-10-HC.png) |
| **Perspective 2** | ![3-4](imgs/3-4.png) | ![3-6](imgs/3-5-H.png) | ![3-8](imgs/3-8-K.png) | ![3-10](imgs/3-9-HC.png) |

### Comparison and discussion

Compared to mean curvature, the cotangent mean curvature is more tolerant of noise and also exhibits more pronounced local curvature features on `lilium_s.obj`.

But on a simple mesh like `plane.obj`, there is no significant difference in the performance of the three curvatures.

## 4 Modal analysis

Mesh file saved in `res/reconstruction`.

|                   |      **Origin**      |         k=5          |         k=15         |        k=1000        |
| :---------------: | :------------------: | :------------------: | :------------------: | :------------------: |
| **Perspective 1** | ![4-1](imgs/4-1.png) | ![4-3](imgs/4-3.png) | ![4-5](imgs/4-5.png) | ![4-7](imgs/4-7.png) |
| **Perspective 2** | ![4-2](imgs/4-2.png) | ![4-4](imgs/4-4.png) | ![4-6](imgs/4-6.png) | ![4-8](imgs/4-8.png) |

### Comparison and discussion

It is clear that a larger $k$ will result in a better reconstruction. As the model contains more vertices, the Laplacian matrix $\mathbf{L}$ is larger, there are more eigenvectors and the computation is more time consuming. When $k$ is small, the reconstruction does not restore the model features very well. When $k= 1000$, the model features are roughly restored, but the computation time is also longer, taking approximately $2.5$ minutes to compute on my device, compared to a few seconds for $k = 5$ and $15$.

## 5 Explicit Laplacian mesh smoothing

**Test mesh: **`meshes/curvatures/plane.obj`

**Denote: ** $\lambda$ - smoothing speed, $t$ - number of smoothing iterations

$t=100$

|                   |        Origin        |    $\lambda=0.01$    |    $\lambda=0.05$    |    $\lambda=0.1$     |      $\lambda=1$      |
| :---------------: | :------------------: | :------------------: | :------------------: | :------------------: | :-------------------: |
| **Perspective 1** | ![4-1](imgs/5-1.png) | ![4-1](imgs/5-5.png) | ![4-1](imgs/5-3.png) | ![4-1](imgs/5-7.png) | ![4-1](imgs/5-9.png)  |
| **Perspective 2** | ![4-1](imgs/5-2.png) | ![4-1](imgs/5-6.png) | ![4-1](imgs/5-4.png) | ![4-1](imgs/5-8.png) | ![4-1](imgs/5-10.png) |

When $t=100$, $\lambda=0.05\sim0.1$ is a good step.

If $\lambda$ is to large (like $1.5$), the model will be distorted:

<img src="imgs/5-11.png" alt="4-1" style="zoom: 50%;" />

## 6 Implicit Laplacian mesh smoothing

$t=100$

**Test mesh: **`meshes/smoothing/fandisk_ns.obj`

|                   |        Origin        |    $\lambda=0.05$    |    $\lambda=1.5$     |
| :---------------: | :------------------: | :------------------: | :------------------: |
| **Perspective 1** | ![4-1](imgs/5-1.png) | ![4-1](imgs/6-3.png) | ![4-1](imgs/6-1.png) |
| **Perspective 2** | ![4-1](imgs/5-2.png) | ![4-1](imgs/6-4.png) | ![4-1](imgs/6-2.png) |

**Test mesh: **`meshes/smoothing/plane_ns.obj`

|                   |        Origin        |    $\lambda=0.05$    |     $\lambda=1.5$     |
| :---------------: | :------------------: | :------------------: | :-------------------: |
| **Perspective 1** | ![4-1](imgs/6-5.png) | ![4-1](imgs/6-7.png) | ![4-1](imgs/6-9.png)  |
| **Perspective 2** | ![4-1](imgs/6-6.png) | ![4-1](imgs/6-8.png) | ![4-1](imgs/6-10.png) |

$t=10$

**Test mesh: **`meshes/smoothing/fandisk_ns.obj`

|                   |        Origin        |    $\lambda=0.05$     |     $\lambda=1.5$     |
| :---------------: | :------------------: | :-------------------: | :-------------------: |
| **Perspective 1** | ![4-1](imgs/5-1.png) | ![4-1](imgs/6-15.png) | ![4-1](imgs/6-16.png) |
| **Perspective 2** | ![4-1](imgs/5-2.png) | ![4-1](imgs/6-17.png) | ![4-1](imgs/6-18.png) |

**Test mesh: **`meshes/smoothing/plane_ns.obj`

|                   |        Origin        |    $\lambda=0.05$     |     $\lambda=1.5$     |
| :---------------: | :------------------: | :-------------------: | :-------------------: |
| **Perspective 1** | ![4-1](imgs/6-5.png) | ![4-1](imgs/6-11.png) | ![4-1](imgs/6-13.png) |
| **Perspective 2** | ![4-1](imgs/6-6.png) | ![4-1](imgs/6-12.png) | ![4-1](imgs/6-14.png) |

### Comparison and discussion

The model will not be distorted like explicit method when $\lambda$ is too large, and it takes fewer iterations to achieve the same results as explicit method.

## 7 Evaluate

When sigma of noise = 0.1, the Implicit Laplacian mesh smoothing can still smooth it, but when sigma of noise = 1, the smoothing is not so good.

Please see my note book and saved meshes to see the effect.
