# Kernel Density Estimation Toolbox for Matlab R2014b

The repositories contain code from [Kernel Density Estimation Toolbox for Matlab (R13)] (http://www.ics.uci.edu/~ihler/code/kde.html)

A number of bugs related to compiling the code on Windows with Matlab 2014b and Visual Studio 2010 have been fixed in this repo as well as the mex files have been pre-compiled for Matlab 2014b and Visual Studio 2010 on Windows 7.

This fix include:

- Static Type casting of double to enum
- Fix for ambigous function calls for: log and pow functions in C++
- clearing warning related to automatic conversion of int to unsigned int

- - - 

# COPYRIGHT / LICENSE 
The kde package and all code were written by Alex Ihler and Mike Mandel, and are copyrighted under the (lesser) GPL:

    Copyright (C) 2003 Alexander Ihler

This program is free software; you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License as published by the Free Software Foundation; version 2.1 or later. This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details. You should have received a copy of the GNU Lesser General Public License along with this program; if not, write to the Free Software Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA 02111-1307, USA.

The authors may be contacted via email at: ihler (at) alum (.) mit (.) edu 

Please visit : [http://www.ics.uci.edu/~ihler/code/kde.html](http://www.ics.uci.edu/~ihler/code/kde.html) for further information.

# Usage Documentation
## Constructors

Method |  Explanation
:-----------|:------------
 kde( )       |        empty kde  
 kde( kde )     |      re-construct kde from points, weights, bw, etc.
 kde( points, bw )       |        construct Gauss kde with weights 1/N
 kde( points, bw, weights)        |          construct Gaussian kde
 kde( points, bw, weights,type)       |       potentially non-Gaussian 
 marginal( kde, dim)   |     marginalize to the given dimensions 
 condition( kde, dim, A)     |  marginalize to ~dim and weight by K(x_i(dim),a(dim))
resample( kde, [kstype] ) 	| draw N samples from kde & use to construct a new kde
reduce( kde, ...) | construct a "reduced" density estimate (fewer points)
joinTrees( t1, t2 ) | make a new tree with t1 and t2 as the children of a new root node


## Accessors: (data access, extremely limited or no processing req'd)
Method |  Explanation
:-----------|:------------
getType(kde) | return the kernel type of the KDE ('Gaussian', etc)
        |
getDim  | get the dimension of the data
getNpts | get the # of kernel locations
getNeff | "effective" # of kernels (accounts for non-uniform weights)
    	|
getPoints(kde) 	|  Ndim x Npoints array of kernel locations
adjustPoints(p,delta) |  shift points of P by delta (by reference!)
rescale(kde,alpha) | rescale a KDE by the (vector) alpha
					|
getBW(kde,index) | return the bandwidth assoc. with x_i (Ndim x length(index))
adjustBW(kde,newBW) | set the bandwidth(s) of the KDE (by reference!) Note: cannot change from a uniform -> non-uniform bandwidth
ksize 	| automatic bandwidth selection via a number of methods
-  	LCV | 1D search using max leave-one-out likelihood criterion
- 	HALL, HJSM 	| Plug-in estimator with good asymptotics; MISE criterion
-  	ROT, MSP | Fast standard-deviaion based methods; AMISE criterion
-  	LOCAL | Like LCV, but makes BW propto k-th NN distance (k=sqrt(N))
  	 |
getWeights 	| [1 x Npts] array of kernel weights
adjustWeights | set kernel weights (by reference!)
  	 |
sample(P,Np,KSType) 	| draw Np new samples from P and set BW according to KSType

## Display: (visualization / description)
Method |  Explanation
:-----------|:------------
plot(kde...) |  plot the specified dimensions of the KDE locations
hist(kde...) | discretize the kde at uniform bin lengths display : text output describing the KDE
double     | boolean evaluation of the KDE (non-empty)

## Statistics: (useful stats & operations on a kde)
Method |  Explanation
:-----------|:------------
mean     | find the (weighted) mean of the kernel centers
covar 	|find the (weighted) covariance of the kernel centers
knn(kde, points, k) | find the k nearest neighbors of each of points in kde
entropy | estimate the entropy of the KDE
kld | estimate divergence between two KDEs
evaluate(kde, x[,tol]) | evaluate KDE at a set of points x
evaluate(p, p2 [,tol]) | same as above, x = p2.pts (if we've already built a tree)
evalIFGT(kde, x, N) | evaluate using the N-term IFGT (requires uniform BW Gaussian kernels)
evalIFGT(p, p2, N) |
evalAvgLogL(kde, x) | compute Mean( log( evaluate(kde, x) ))
evalAvgLogL(kde, kde2)| same as above, but use the weights of kde2
evalAvgLogL(kde) | self-eval; leave-one-out option
llGrad(kde,kde2) | estimate the gradient of log-likelihood for kde evaluated at the points of kde2
entropyGrad(p) | estimate gradient of entropy (uses llGrad)
miGrad(p,dim) | estimate gradient for mutual information between p(dim), p(~dim)
klGrad(p1,p2) | estimate gradient direction of KL-divergence

##  Mixture products: (NBP stuff) (GAUSSIAN KERNELS ONLY) 
Method |  Explanation
:-----------|:------------
productExact | exact computation (N^d kernel centers)
productApprox | accessor for other product sampling methods
      - prodSampleExact |sample N points exactly (N^d computation)
  	- prodSampleEpsilon | kd-tree epsilon-exact sampler
  	- prodSampleGibbs1 | seq. index gibbs sampler
  	- prodSampleGibbs2 | product of experts gibbs sampler
  	- prodSampleGibbsMS1 | multiresolution version of GS1
  	- prodSampleGibbsMS2 | multiresolution version of GS2
  	- prodSampleImportance | "mixture" importance sampling
  	- prodSampleImportGauss | gaussian importance sampling
  	 

