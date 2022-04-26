The following is based on a [post](https://list.waikato.ac.nz/hyperkitty/list/wekalist@list.waikato.ac.nz/message/TJIEPO2CYGBJE5NFYPIP26SL4WFMWEZC/) 
from Eibe Frank on the Weka mailing list.

# CPU acceleration

WEKA implementations of algorithms that are based on standard linear algebra generally apply the MTJ library, which, 
optionally, can use a much faster native backend than the default pure-Java backend. Install the appropriate 
`netlibNative*` package for your platform using the WEKA package manager to get this acceleration.

To get further speed-ups, compile OpenBLAS or similar for your particular computer and link it with MTJ/NetlibJava, 
i.e., to make use of multi-threaded linear algebra (see info at [https://github.com/fommil/netlib-java](https://github.com/fommil/netlib-java)). 
There is good news for Mac OS X users: OS X comes with a system-optimised library (vecLib) and all you need to do 
is install the WEKA package netlibNativeOSX* to get high-speed matrix algebra.

Here is a list of WEKA schemes (not sure whether it’s complete) that can benefit from this (but only on sufficiently large data):

* GaussianProcesses
* PrincipalComponents
* LinearRegression
* M5P
* M5Rules
* MultivariateGaussianEstimator

There are also some schemes in various packages:

* LDA
* QDA
* FLDA
* LatentSemanticAnalysis
* Nystroem
* RotationForest
* LeastMedSq
* RBFNetwork

# GPU acceleration

Now, regarding GPUs, it's actually possible to set up a GPU-based backend for MTJ/NetlibJava. However, WEKA uses 
double-precision arithmetic and consumer-grade GPUs are optimised for single-precision arithmetic and very slow 
when using double-precision arithmetic. (Neural networks in deep learning libraries are generally trained using 
single-precision arithmetic.) Anyway, see instructions for using NVBLAS (Nvidia's BLAS wrapper) on Ubuntu 20.04 
in article [MTJ with NVBLAS](mtj_with_nvblas.md).

I tried PrincipalComponents (see instructions below for how I did this) and LDA with NVBLAS. However, in my experiments, 
only a small fraction of the BLAS operations were off-loaded to the GPU by NVBLAS, even though the names of the BLAS 
routines in `nvblas.log` are all in the list of GPU-based routines at

[https://docs.nvidia.com/cuda/nvblas/index.html#routines](https://docs.nvidia.com/cuda/nvblas/index.html#routines)

Perhaps it also depends on the parameter settings of those routines as to whether they are off-loaded to the GPU. 
It seems NVBLAS uses some heuristics to guess whether it's worthwhile to use the GPU instead of the CPU for certain 
input parameters.

So, apart from using wekaDeeplearning4j, there is this second way of using GPUs: by applying the NVBLAS backend 
with MTJ/netlib-java.

The third way to use a GPU from WEKA is to install XGBoost with GPU support in Python or R (note that the GPU support 
for XGBoost was actually developed here at Waikato by Rory Mitchell!). That can then be used in WEKA via the RPlugin 
and wekaPython. In 2020, when I tried this on a Windows machine, it only worked out of the box using wekaPython though, 
and not using R. Here is an email to a colleague that I wrote at the time:

The fourth way to use a GPU from WEKA is to use the kerasZoo package, which has

  [https://github.com/Waikato/weka-3.8/blob/master/packages/internal/kerasZoo/src/main/java/weka/classifiers/keras/KerasZooClassifier.java](https://github.com/Waikato/weka-3.8/blob/master/packages/internal/kerasZoo/src/main/java/weka/classifiers/keras/KerasZooClassifier.java)

However, it may need to be updated to work with the very latest version of WEKA.

Finally, I suppose any installed GPU-support in R/Python for any of other the learning schemes in MLR (version 1) or 
scikit-learn should be "transparently" available in WEKA via RPlugin and wekaPython respectively.

Unfortunately, in contrast to the MTJ and wekaDeeplearning4j options, wekaPython or RPlugin always incurs an overheard 
(both, in terms of time and memory) by transferring the data from WEKA to R or Python respectively (and then from RAM 
to GPU device memory!).
