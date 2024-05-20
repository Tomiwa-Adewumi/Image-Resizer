# Image Shrinking
Welcome to the Image Shrinking project! This application intelligently reduces the width of an image by removing a vertical path of uninteresting pixels. The goal is to retain the most significant features of the image by focusing on areas with high contrast.

![image shrinking example](a_res/output.gif)

## Overview
The image shrinking algorithm works by identifying and removing the least interesting vertical path of pixels from the top to the bottom of the image. "Uninteresting" areas are those where pixels are similar to their neighbors, measured using the gradient (rate of change in pixel intensity).

### Gradient Operators
The similarity of pixels is estimated using gradient operators. In this project, we utilize two gradient operators:
* Sobel Operator
* Prewitt Operator

### Gradients
Gradients help identify edges in images. Both Sobel and Prewitt operators have vertical and horizontal components calculated through image convolution. Here’s how they look:

Sobel:

$$G_x = \begin{bmatrix}
+1 & 0 & -1 \\
+2 & 0 & -2\\
+1 & 0 & -1
\end{bmatrix} * A $$

$$G_y = \begin{bmatrix}
+1 & +2 & +1 \\
0 & 0 & 0\\
-1 & -2 & -1
\end{bmatrix} * A $$

Prewitt:

$$G_x = \begin{bmatrix}
+1 & 0 & -1 \\
+1 & 0 & -1\\
+1 & 0 & -1
\end{bmatrix} * A$$

$$G_y = \begin{bmatrix}
+1 & +1 & +1 \\
0 & 0 & 0\\
-1 & -1 & -1
\end{bmatrix} * A $$

In either case the magnitude of the gradient (at each pixel) is computed as:

$$G= \sqrt{{G_x}^2 + {G_y}^2}$$

This gradient magnitude gives a measure of how interesting a pixel is. Higher values indicate more significant changes in pixel intensity, highlighting edges.
![side by side original image and gradient](a_res/orig_grad.png)

(original image (left): Gregory Shamus via Getty Images), gradient (or energy) image of the original (right)

## Low Energy Path
To shrink the image, we find a low-energy path of pixels from the top to the bottom, where the sum of the gradient values along the path is minimized. This path represents the least significant part of the image.
![low energy path](a_res/low_path.png)
The process can be visualized as finding the shortest path in a directed acyclic graph (DAG), where the pixels are nodes, and edges exist between each pixel and its southern neighbors. The cost of an edge is determined by the gradient value of the destination pixel.

## Implementation
### Deliverable 1: Prewitt Operator
Your first task is to implement the Prewitt operator. Complete the public int prewitt(int x, int y) method in the EdgeAvoidanceImageShrinking class. The Prewitt operator is similar to the Sobel operator, with slight differences in the matrices.

### Shortest Path Calculation
The shortest path is calculated by evaluating all possible starting positions in the first row and computing the shortest path from each position using the gradient image. The shortest path is then traced back from the bottom to the top row.

![shortest path example](a_res/short_path.png)
shortest path example
