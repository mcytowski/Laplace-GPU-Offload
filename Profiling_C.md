# Step 1: Profiling

> **GOAL** Use basic profiling techniques to identify the most computationally expensive parts of the code.

In this section we will use the GNU Gprof performance analysis tool to profile our Laplace code.

First, the code needs to be compiled with *-pg* and *-g* options. This will enable the generation of line-by-line profiling information for gprof.

```
bash-4.2$ make -f makefile.gprof
gcc -pg -g -c -o laplace.o laplace.c
gcc -pg -g  -o laplace laplace.o
```

Next, we will execute the Laplace code as usual.

```
bash-4.2$ srun -n 1 -u ./laplace 4000
Iteration  100, dt 0.457740
Iteration  200, dt 0.228780
Iteration  300, dt 0.152236
Iteration  400, dt 0.113971
Iteration  500, dt 0.091044
Iteration  600, dt 0.075800
Iteration  700, dt 0.064912
Iteration  800, dt 0.056725
Iteration  900, dt 0.050384
Iteration 1000, dt 0.045296
Iteration 1100, dt 0.041149
Iteration 1200, dt 0.037686
Iteration 1300, dt 0.034756
Iteration 1400, dt 0.032248
Iteration 1500, dt 0.030079
Iteration 1600, dt 0.028180
Iteration 1700, dt 0.026504
Iteration 1800, dt 0.025014
Iteration 1900, dt 0.023681
Iteration 2000, dt 0.022483
Iteration 2100, dt 0.021399
Iteration 2200, dt 0.020413
Total time was 64.238252 seconds.
```
After completing the task, the *gmon.out* file containing profiling information will be generated in the working directory.

Now, we can use the *gprof* profiler to generate the profiling report.  

```bash
bash-4.2$ gprof -lbp laplace gmon.out
```
The set of options used above (*-lbp*) will generate only line by line profiling information with no call graph analysis.
```
Flat profile:

Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total
 time   seconds   seconds    calls  Ts/call  Ts/call  name
 23.77     15.27    15.27                             main (laplace.c:55 @ 40089e)
 18.85     27.38    12.11                             main (laplace.c:46 @ 4007a1)
 14.79     36.88     9.50                             main (laplace.c:56 @ 400949)
 13.84     45.76     8.89                             main (laplace.c:46 @ 400831)
 10.55     52.54     6.77                             main (laplace.c:47 @ 4007e7)
  8.21     57.81     5.28                             main (laplace.c:47 @ 40080c)
  5.22     61.16     3.35                             main (laplace.c:45 @ 40085b)
  2.78     62.95     1.79                             main (laplace.c:54 @ 400985)
  2.14     64.33     1.37                             main (laplace.c:54 @ 400892)
  0.16     64.43     0.10                             main (laplace.c:46 @ 400808)
  0.14     64.52     0.09                             main (laplace.c:45 @ 400795)
  0.03     64.54     0.02                             init (laplace.c:83 @ 400ab1)
  0.02     64.55     0.01                             main (laplace.c:44 @ 40086c)
  0.00     64.55     0.00        1     0.00     0.00  init (laplace.c:77 @ 400a92)
  ```
As can be seen, the majority of time is spent, as expected, in two parts of the code:
* the main computational kernel
    ```c
    for(i = 1; i <= GRIDX; i++)
        for(j = 1; j <= GRIDY; j++)
            T_new[i][j] = 0.25 * (T[i+1][j] + T[i-1][j] +
                                        T[i][j+1] + T[i][j-1]);    
    ```
* the loops computing the largest change in the temperature
```c
for(i = 1; i <= GRIDX; i++){
    for(j = 1; j <= GRIDY; j++){
      dt = MAX( fabs(T_new[i][j]-T[i][j]), dt);
      T[i][j] = T_new[i][j];
    }
}
```

Those two loop nests are the candidates for directive-based GPU optimisation.

> **KEY TAKEAWAYS**
> 1. We have analysed the performance of the Laplace code with the use of GNU Gprof profiler.
> 2. We have identified the most computationally expensive parts of the code - two loop nests executed in each iteration of the solver.
