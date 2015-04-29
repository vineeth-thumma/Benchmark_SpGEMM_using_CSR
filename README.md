# Benchmark_SpGEMM_using_CSR

<br><hr>
<h3>Introduction</h3>

This is the source code of two papers:

(1) Weifeng Liu and Brian Vinter, "An Efficient GPU General Sparse Matrix-Matrix Multiplication for Irregular Data," 
Parallel and Distributed Processing Symposium, 2014 IEEE 28th International (IPDPS '14), pp.370-381, 19-23 May 2014. <a href="http://dx.doi.org/10.1109/IPDPS.2014.47">PDF at IEEE Xplore</a>.

A preprint version and its IPDPS '14 slides can be found inside the repository.

(2) Weifeng Liu and Brian Vinter, "A Framework for General Sparse Matrix-Matrix Multiplication on GPUs and Heterogeneous Processors," Extended version of paper (1) submitted to Journal of Parallel and Distributed Computing (JPDC), 2015. <a href="http://arxiv.org/abs/1504.05022">PDF at arXiv</a>.

Contact: Weifeng Liu (weifeng.liu _at_ nbi.ku.dk) and Brian Vinter (vinter _at_ nbi.ku.dk).

<br><hr>
<h3>Versions</h3>

Now this work has two separate branches: CUDA and OpenCL. The CUDA version requires an nVidia GPU, CUDA SDK and CUSP library. The OpenCL version only needs an OpenCL-enabled GPU and OpenCL compiling environment. 

<br><hr>
<h3>Preparation</h3>

To use this SpGEMM program, the first thing you need to do is to change the 'Makefile' with correct CUDA installation path and OpenCL libs path. Further, if you are using a CUDA device, check its compute capability (e.g., 3.5, 5.0 or above) and change item like '-arch=sm_35' if needed. Then you can build the code.

<br><hr>
<h3>Execution</h3>

This program executes C=AB operation, where A, B and C are all sparse matrices. 

You can either (1) use bhSPARSE class and call its SpGEMM method in your own code, or (2) load an off-line square matrix file (*.mtx in matrix market format) as input matrix A, then benchmark C=A^2 operation. In 'main.cpp' file, see function 'test_small_spgemm()' or 'benchmark_spgemm()' for details.

Here are some command-line execution examples using CUDA version:

(1) run SpGEMM on a small matrix

`./spgemm -cuda -spgemm 0`

(2) run SpGEMM on poisson5pt matrices generated by CUSP

`./spgemm -cuda -spgemm 1`

(3) run SpGEMM on poisson9pt matrices generated by CUSP

`./spgemm -cuda -spgemm 2`

(4) run SpGEMM on poisson7pt matrices generated by CUSP

`./spgemm -cuda -spgemm 3`

(5) run SpGEMM on poisson27pt matrices generated by CUSP

`./spgemm -cuda -spgemm 4`

(6) run SpGEMM on a matrix loaded from a matrix market file

`./spgemm -cuda -spgemm cage4.mtx`

(7) run SpGEMM on a matrix loaded from a matrix market file

`./spgemm -cuda -spgemm /home/username/matrices/cage4.mtx`


Here are some command-line execution examples using OpenCL version:

(1) run SpGEMM on a small matrix

`./spgemm -cuda -spgemm 0`

(2) run SpGEMM on a matrix loaded from a matrix market file

`./spgemm -opencl -spgemm cage4.mtx`

(3) run SpGEMM on a matrix loaded from a matrix market file

`./spgemm -opencl -spgemm /home/username/matrices/cage4.mtx`

(4) run SpGEMM (using re-allocatable system memory of AMD APU) on a matrix loaded from a matrix market file

`./spgemm -opencl-hcmp -spgemm /home/username/matrices/cage4.mtx`

<br><hr>
<h3>Precision of value data type</h3>

The SpGEMM supports single precision and double precision floating-point numbers. The default data type is 64-bit double precision. If 32-bit single precision is required, change `typedef value_type` in 'common.h' in the CUDA version. For the OpenCL version, change `typedef value_type` in 'common.h' and `typedef vT` in files 'SpGEMM_EM_kernels.cl', 'SpGEMM_ESC_0_1_kernels.cl', 'SpGEMM_ESC_2heap_kernels.cl', 'SpGEMM_ESC_bitonic_kernels.cl' and 'SpGEMM_copyCt2C_kernels.cl'.

<br><hr>
<h3>Tested environments</h3>

The CUDA version has been tested on nVidia GeForce GT 650M, GTX 680, GTX Titan, GTX Titan Black and GTX 980 with CUDA SDK v6.0/v6.5, CUSP v0.4.0 and multiple operating systems (Mac OS X v10.9 and Ubuntu v12.04/v14.04).

The OpenCL version has been tested on AMD Radeon HD 7970, R9 290X and A10-7850k APU with OpenCL v1.2/v2.0 and Ubuntu v12.04/v14.04.
