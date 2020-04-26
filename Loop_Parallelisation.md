# Step 2: Loop parallelisation

---
<span style="color:red">**GOAL**</span>

Apply basic OpenACC and OpenMP directives to parallelise loop nests.

---

In this section we will apply basic OpenACC and OpenMP directives to parallelise loop nests identified as the most computationally expensive in the previous profiling step.

Loop parallelisation directives can be placed right before each of the loop nests in the code. There is a difference on how this can be achieved in OpenACC and OpenMP.

## OpenACC
In OpenACC programmers will usually start with placing the following *kernels* directive right before the first *for* loop.
```c
#pragma acc kernels
```
This directive instructs the compiler to generate parallel accelerator kernels for the loop (or loops) following the directive.

This is what we will refer to as *descriptive approach* to programming GPUs, where the programmer uses directives to tell the compiler where data-independent loops are located and lets the compiler decide how/where to parallelise the loops based on the architecture.

In the case of our Laplace example the *kernels* directive can be applied as follows:
```c
// main computational kernel, average over neighbours in the grid
#pragma acc kernels
for(i = 1; i <= GRIDX; i++)
    for(j = 1; j <= GRIDY; j++)
        T_new[i][j] = 0.25 * (T[i+1][j] + T[i-1][j] +
                                    T[i][j+1] + T[i][j-1]);

// reset dt
dt = 0.0;

// compute the largest change and copy T_new to T
#pragma acc kernels
for(i = 1; i <= GRIDX; i++){
    for(j = 1; j <= GRIDY; j++){
      dt = fmax( fabs(T_new[i][j]-T[i][j]), dt);
      T[i][j] = T_new[i][j];
    }
}
```

## OpenMP



## Further analysis

TBD: this


| KEY TAKEAWAYS: Apply basic OpenACC and OpenMP directives to parallelise loop nests |
| --- |
