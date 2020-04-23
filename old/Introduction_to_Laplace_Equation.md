
# Introduction to Laplace Equation

## Historical note
Laplace's equation is a second-order partial equation named after [Pierre-Simon, marquis de Laplace](https://en.wikipedia.org/wiki/Pierre-Simon_Laplace), a French scholar and polymath whose work was a foundation of many scientific fields including engineering, mathematics, physics, statistics and astronomy.

![Pierre-Simon, marquis de Laplace](https://upload.wikimedia.org/wikipedia/commons/3/39/Laplace%2C_Pierre-Simon%2C_marquis_de.jpg)

One of the most famous Marquis de Laplace quotes is: *Nature laughs at the difficulties of integration*.

## Problem description

The Laplace equation is commonly used in physics to describe various phenomena, including heat transfer.

The 2D Laplace equation is often written as:
$$
\nabla ^2 u = \frac{\partial ^2 u}{\partial x^2} + \frac{\partial ^2 u}{\partial y^2} = 0
$$

The above equation can be discretised with the Finite Difference Method on a 2D grid. We will assume that each grid cell has side h. The discretisation will lead us to the following formula:
$$
\frac{(u _{i,j-1}+u _{i,j+1}+u _{i-1,j}+u _{i+1,j}-4u _{i,j})}{h^2}=0
$$

In the above formula $u_{i,j}$ represents the value of $u$ function in grid node with $(i,j)$ coordinates.

Note that the above equation can be simplified to:
$$
u _{i,j}=0.25(u _{i,j-1}+u _{i,j+1}+u _{i-1,j}+u _{i+1,j})
$$

Since the above formula has to hold for every element in the grid, the computational algorithm of solving Laplace equation is an iterative procedure. The value of $u _{i,j}$ in each iteration will be computed from the values of 4 neighbouring grid nodes ($u _{i,j-1},u _{i,j+1},u _{i-1,j},u _{i+1,j}$) computed in previous iteration. This process will be repeated until the solution converges. The example code used in this tutorial is implementing the described iterative process. 

In fact, this computational approach of iterative kernels which update array elements according to some fixed pattern, represents a whole class of algorithms. The so-called [stencil codes](https://en.wikipedia.org/wiki/Stencil_code) are most commonly found in the codes of computer simulations, e.g. for computational fluid dynamics in the context of scientific and engineering applications. 

### Initial and boundary conditions

The use of Finite Difference Methods and the resulting iterative alorithm implies that:
* the initial condition (initial values for each grid node) need to be provided as an input to the algorithm,
* the boundary condition for cells on the boundaries of the 2D domain need to be provided as an input to the algorithm.

Here we will use a similar approach to the one described in the [Software Engineering Course](https://software-engineering.readthedocs.io). At the start of the program execution, we will assign the initial value 0 to all grid cells. Moreover, we will set the boundaries of the grid to have the value 0 for the row/column where the first/second index are zero, and along the other two boundaries the temperature will increase linearly from 0 to 128.


## References

1. https://en.wikipedia.org/wiki/Pierre-Simon_Laplace
2. https://software-engineering.readthedocs.io/en/latest/laplace_equation.html
3. https://en.wikipedia.org/wiki/Stencil_code

