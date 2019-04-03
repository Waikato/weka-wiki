When running Ubuntu 18.04, you might see the following warning message(s) in the console:

```
Apr 03, 2019 5:40:10 PM com.github.fommil.netlib.BLAS <clinit>
WARNING: Failed to load implementation from: com.github.fommil.netlib.NativeSystemBLAS
Apr 03, 2019 5:40:10 PM com.github.fommil.netlib.BLAS <clinit>
WARNING: Failed to load implementation from: com.github.fommil.netlib.NativeRefBLAS
Apr 03, 2019 5:40:10 PM com.github.fommil.netlib.LAPACK <clinit>
WARNING: Failed to load implementation from: com.github.fommil.netlib.NativeSystemLAPACK
Apr 03, 2019 5:40:10 PM com.github.fommil.netlib.LAPACK <clinit>
WARNING: Failed to load implementation from: com.github.fommil.netlib.NativeRefLAPACK
```

You can easily fix this by installing the missing dependencies with this command:

```
sudo apt-get install libgfortran-6-dev
```

