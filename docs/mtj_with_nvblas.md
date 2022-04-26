(The following is based on a [post](https://list.waikato.ac.nz/hyperkitty/list/wekalist@list.waikato.ac.nz/message/TJIEPO2CYGBJE5NFYPIP26SL4WFMWEZC/) 
from Eibe Frank on the Weka mailing list.)

Here is an example of running MTJ with NVBLAS (NVIDIA's BLAS wrapper) on Ubuntu:

1. Installed
   
    [https://prdownloads.sourceforge.net/weka/weka-3-8-6-azul-zulu-linux.zip](https://prdownloads.sourceforge.net/weka/weka-3-8-6-azul-zulu-linux.zip)

    in

    ```
    /home/eibe/Desktop
    ```

2. Ran

    ```
    ~/Desktop/weka-3-8-6/weka.sh -main weka.core.WekaPackageManager -install-package netlibNativeLinux
    ```

3. To install CPU-based system BLAS/LAPACK, ran

    ```
    sudo apt-get install libblas-dev liblapack-dev
    sudo ln -s /usr/lib/x86_64-linux-gnu/libblas.so.3 /usr/lib/libblas.so.3
    sudo ln -s /usr/lib/x86_64-linux-gnu/liblapack.so.3 /usr/lib/liblapack.so.3
    ```

4. Downloaded and installed CUDA 11.6 from [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)

5. Copied example `nvblas.conf` from [https://docs.nvidia.com/cuda/nvblas/](https://docs.nvidia.com/cuda/nvblas/) into local directory using

    ```
    cat > nvblas.conf
    ```

6. Edited `nvblas.conf` to have
   
    ```
    NVBLAS_CPU_BLAS_LIB  /usr/lib/x86_64-linux-gnu/blas/libblas.so.3
    ```

7. Now, by adapting what's given at [https://github.com/fommil/netlib-java/wiki/NVBLAS](https://github.com/fommil/netlib-java/wiki/NVBLAS), issued

    ```
    export LD_LIBRARY_PATH=/usr/local/cuda-11.6/lib64:/usr/lib/x86_64-linux-gnu/blas/libblas.so.3
    ```

8. Then,

    ```
    ~/Desktop/weka-3-8-6/weka.sh -main weka.Run .RandomRBF -a 5000 > RandomRBF.a5000.arff
    LD_PRELOAD=libnvblas.so ~/Desktop/weka-3-8-6/weka.sh -main weka.Run .attributeSelection.PrincipalComponents -i RandomRBF.a5000.arff
    ```

9. Observation: Memory is being allocated on the GPU. Looking at `nvblas.log`, the GPU is used, but only for some `dgemm` operations. 
   However, according to [https://docs.nvidia.com/cuda/nvblas/](https://docs.nvidia.com/cuda/nvblas/), the `tremm` operation 
   (which is executed on the CPU) should also be supported by the GPU. 
   