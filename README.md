# build_kokkos on ThetaGPU

1. login ThetaGPU compute node
  1.1 module load cobalt/cobalt-gpu
  1.2 qsub -n 1 -t60 -A Comp_Perf_Workshop -I -q
  
  
2.  mkdir kokkos
    cd kokkos

    git clone https://github.com/kokkos/kokkos.git
    
    git clone https://github.com/kokkos/kokkos-tutorials.git
    
   
   
3.  cd kokkos

    mkdir build
    cd build

    module load cmake
    
    
4. Install on Nvidia A100 GPU 
export KOKKOS=/home/wangyf/THETA/perf_workshop/CompPerfWorkshop/06_kokkos-raja/kokkos/kokkos

cmake .. \
    -D CMAKE_CXX_COMPILER="$KOKKOS"/bin/nvcc_wrapper\
    -D CMAKE_CXX_FLAGS=-fopenmp \
    -D CMAKE_BUILD_TYPE=Release \
    -D CMAKE_INSTALL_PREFIX="${PWD}"/install \
    -D Kokkos_ENABLE_CUDA=On \
    -D Kokkos_ENABLE_CUDA_LAMBDA=On \
    -D Kokkos_ENABLE_SERIAL=On \
    -D Kokkos_ENABLE_OPENMP=On \
    -D Kokkos_ARCH_AMPERE80=On \
    -D Kokkos_ENABLE_CUDA_UVM=On \
    -D CUDA_ROOT=/soft/cuda/cuda_11.2.1_460.32.03_linux \

5. make -j install

export CMAKE_PREFIX_PATH="${PWD}"/install:"${CMAKE_PREFIX_PATH}"

6. Compile exercise

cd kokkos-tutorials/Exercises/04/Solution
mkdir build
cd build
cmake ..
make

6. export CUDA_VISIBLE_DEVICES=0
7. export OMP_PROC_BIND=spread
8. export OMP_PLACES=threads

Then run
./04_Exercise
