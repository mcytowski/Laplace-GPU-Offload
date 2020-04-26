# Step 3: Data transfers

> **GOAL** Apply data transfer OpenACC and OpenMP directives to improve the performance of the code.

Non-optimal memory management (e.g. excessive memory transfers) can heavily impact the performance of any GPU accelerated code. Therefore it is very important to understand how memory is being mapped and copied between host and device.  

When using PGI compiler for OpenACC this can be achieved by using **-Minfo=accel** compiler option. The information about memory transfers will be printed to stdout.

```c
pgcc -O3 -acc -Minfo=accel -c -o laplace_acc.o laplace_acc.c
main:
     43, Generating implicit copyin(T[:][:]) [if not already present]
         Generating implicit copyout(T_new[1:2048][1:2048]) [if not already present]
     44, Loop is parallelizable
     45, Loop is parallelizable
         Generating Tesla code
         44, #pragma acc loop gang, vector(4) /* blockIdx.y threadIdx.y */
         45, #pragma acc loop gang, vector(32) /* blockIdx.x threadIdx.x */
     53, Generating implicit copyin(T_new[1:2048][1:2048]) [if not already present]
         Generating implicit copy(T[1:2048][1:2048]) [if not already present]
     54, Loop is parallelizable
     55, Loop is parallelizable
         Generating Tesla code
         54, #pragma acc loop gang, vector(4) /* blockIdx.y threadIdx.y */
         55, #pragma acc loop gang, vector(32) /* blockIdx.x threadIdx.x */
         56, Generating implicit reduction(max:dt)
```

As can be seen from the above report arrays **T** and **T_new** are being copied multiple times in and out between host and device. This copying occurs in every iteration of the algorithm.

> **NOTE** We should acknowledge the importance of *-Minfo=accel* compiler feedback option of the PGI compiler for OpenACC. GCC and Clang does not provide similar functionality for OpenMP.

The impact of memory transfers on the current performance of the GPU kernel can be also measured by e.g. *nvprof* profiler by running:

```bash
bash-4.2$ srun -u -n 1 nvprof ./laplace_mp 4000
```

As can be seen from the report generated below for the OpenMP version of the code, memory transfers represent more than 98% of the compute time (HtoD stands for Host to Device, DtoH stands from Device to Host).

```
==228979== Profiling application: ./laplace_mp 4000
==228979== Profiling result:
            Type  Time(%)      Time     Calls       Avg       Min       Max  Name
 GPU activities:   51.25%  60.3494s     11236  5.3711ms  1.1520us  9.4626ms  [CUDA memcpy HtoD]
                   47.73%  56.2051s     11237  5.0018ms  1.4080us  10.967ms  [CUDA memcpy DtoH]
                    0.84%  987.39ms      2247  439.43us  430.30us  463.33us  __omp_offloading_47c4f666_4f0059e6_main_l56
                    0.18%  217.23ms      2247  96.677us  95.583us  98.144us  __omp_offloading_47c4f666_4f0059e6_main_l45
```


OpenMP

Nesting of target regions, either dynamically or statically, is not allowed.
General mapping rules are as follows:
Pointers are mapped as zero-length array sections with zero base for both explicit and implicit mapping.
If a zero-length array section that is derived from a pointer variable is mapped, that variable is initialized with the address of the corresponding storage location on the device. If the corresponding storage does not exist, that is, it has not been mapped before, the pointer variable is initialized to NULL.
The data environment of a target region is defined by the implicit and explicit mapping of variables between the host and device:
Implicit mapping
The compiler determines which variables must be mapped to, from, or both to and from the device data environment. Scalar variables that are not explicitly mapped are implicitly mapped as firstprivate if defaultmap(tofrom:scalar) is not specified.
Explicit mapping
You can use the map clause on the target region to explicitly list variables to be mapped to, from, or both to and from the device data environment.
A listed data variable cannot appear in both a data-sharing clause and the map clause on the same target construct.
