# `CCOpt.jl` Paper examples
This repo collects all the disperate code which is used in the numerical experiments in the paper which pertains to `CCOpt.jl`.
The details of the steps to run the examples are provided below.

## `MacMPEC`
In order to run the `MacMPEC` experiments one can run the following script from the root of the CCOpt.jl directory:
```julia
include("scripts/macmpec/run_macmpec.jl")
solnames,names,stats = benchmark_macmpec(;range=:)
```

## `SC-OPF`
Follow the instructions in the provided `MPCCBenchmark.jl` repository.

## Building `libMad` and `CasADi`
After installing the prerequisites for the two packages running the following script will build both pieces of software:
```bash
mkdir <path to libMad>/libMad/build_cmake
cd <path to libMad>/libMad/build_cmake
cmake ..
make clean
JULIA_CUDA_USE_COMPAT="false" make install
cd install
tar -pcvzf libMad_ccopt.tar.gz *
cd <path to casadi>/casadi/build
cmake .. -DWITH_IPOPT=ON -DWITH_HSL=ON -DWITH_MOCKUP_HSL=ON -DWITH_BUILD_IPOPT=ON -DWITH_BUILD_MUMPS=ON -DWITH_MADNLP=ON -DWITH_BUILD_MADNLP=ON -DLIBMAD_TAR_PATH=file://<path to libmad binaries>/libMad_ccopt.tar.gz  -DWITH_EXAMPLES=ON -DWITH_CCOPT=ON -DWITH_BUILD_CCOPT=ON -DWITH_PYTHON=ON -DWITH_PYTHON3=ON -DPYTHON_PREFIX=../lib -DWITH_DEEPBIND=ON -DCMAKE_INSTALL_PREFIX=../lib
make -j8 install
```

## Using `libMad` from Matlab
In order to run the examples in the `nosbench` and `LCQPTest` repositories, you can run matlab with the following command:
```
JULIA_CUDA_USE_COMPAT="false" JULIA_HSL_LIBRARY_PATH="<path to hsl binaries>/libHSL_binaries.v2025.7.21/lib/" LD_PRELOAD="<path to casadi>/lib/lib/julia/libunwind.so:/lib/x86_64-linux-gnu/libstdc++.so.6" ./matlab
```
and make sure to follow the installation instructions for `nosnoc` found in the `nosnoc` foler in this repository.