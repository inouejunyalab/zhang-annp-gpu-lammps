## ANNP-GPU-Lammps (GPU-accelerated artificial neural network potential for molecular dynamics simulation)

## Description: 
-This package is used to implement of artificial neural network potential (ANNP), which can be accelerated by using GPU card.
-You can compile these into LAMMPS package according to the following procedures. It can support the CUDA- and OpenCL-enabled GPU card.
-All the potential parameters (Ni) are obtained from Dr. S. Desai, Dr. S.T. Reeve and co-workers (References 1 and 2), but the format is defined by us, as can be see the "ni_annp_potential_2.ann" file. 

-The files in the lib folder are the library, which should be complied into lammps/lib/GPU package.
-The files in the src folder are the source files, which provide the interface to lammps.
-If you compile the lammps package, please be careful about the GPU_ARCH, which must be consistent with your GPU cards

## Installation:
1) Download all files in "lib", "src" directors and the potential file 

2) copy all files in "lib" directory into lammps/lib/gpu directory
   cp ./lal_* ../lammps/lib/gpu;
   (a) set correct value of GPU_ARCH in "Makefile.linux"
   (b) make -f Makefile.linux
   Note: the "n_Block" in "lal_annp.cpp" file can be changed to make sure that most of cores on GPU card busy

3) copy "pair_annp.*" in "src" directory into lammps/src/MANYBODY directory
   cp ./pair_annp.* ../lammps/src/MANYBODY;
4) copy "pair_annp_gpu.*" in "src" directory into lammps/src/GPU directory 
   cp ./pair_annp_gpu.* ../lammps/src/GPU;
   add the name of the two "pair_annp_gpu*" files into Install.h file in GPU directory

5) make mpi

6) If you want to use OpenCL library, it's better using the cmake to compile lammps by following procedures:
   mkdir build_opencl
   cmake ../cmake -C ../cmake/presets/basic.cmake -D PKG_GPU=on -D GPU_API=opencl -D GPU_PREC=mixed -D GPU_ARCH=sm_61;
   make;
   sudo make install
   Note: the definition of shared memory in cuda must be changed into local (__shared__ ----> __local) in the "lal_annp.cu" file  


## MD simulation in Lammps:
1) the Newton third law must be opened:  
   newton on
2) pair_style	annp
   pair_style	* * ni_annp_potential.ann Ni


## Tested systems, GPU cards, Lammps version:
1) Ubuntu 16.04 (System)
2) Nvidia Quadro p5000 (GPU card)
3) September 2021 stable version (Lammps)


## References:
1) S. Desai, S.T. Reeve, J.F. Belak, Comput. Phys. Commun. 270, (2022).
2) J. Behler, M. Parrinello, Phys Rev Lett, 98 (2007).
3) S. Plimpton, J. Comput. Phys. 117, 1 (1995).


## Update and release:
[GitHub] (https://github.com/inouejunyalab/Meng_Zhang/tree/main/annp-gpu-lammps/ni/)


## Contact Information:
Email: meng_zhang@metall.t.u-tokyo.ac.jp (M. Zhang), junya_inoue@metall.t.u-tokyo.ac.jp (J. Inoue)
Please contact us if you have any questions or suggestion for the implementation