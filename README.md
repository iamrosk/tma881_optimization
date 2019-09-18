[//]: # (To preview markdown file in Emacs type C-c C-c p)

# Assignment 1: Optimization
The goal of the first assignment is to practice how to time and profile code,
and to inspect some aspects that contribute to performance.

## Time
Parametric study of **optimization flags**:

- `-O0`: the average elapsed time of 10 loops is 2.195925845 sec (min).
- `-O1`: the average elapsed time of 10 loops is 2.198843204 sec.
- `-O2`: the average elapsed time of 10 loops is 2.199601650 sec (max).
- `-O3`: the average elapsed time of 10 loops is 2.196532865 sec.
- `-Os`: the average elapsed time of 10 loops is 2.196100256 sec.
- `-Og`: the average elapsed time of 10 loops is 2.199355819 sec.

Example of compilation to assembly code for `-O0`:
`gcc time_sum.c -S -O0 -o time_sum_O0.s`

## Inlining
Benchmarking of `mainfile` program:
![mainfile benchmark](./img/benchmark_mainfile.png)

Benchmarking of `separatefile` program:
![separatefile benchmark](./img/benchmark_separatefile.png)

Benchmarking of `inlined` program:
![inlined benchmark](./img/benchmark_inlined.png)

The `separatefile` program is **slower** possibly because the compiler
misses the information about the program, needed to optimize the
code (e.g. automatic inlining).
To remedy this, one might try to enable **link-time optimizer**.

### `nm` tool
When examining the first two executables for symbols that correspond to
`mul_cpx_mainfile` and `mul_cpx_separatefile`, they can be found in
both cases:

`0000000000401120 T mul_cpx_mainfile`

`00000000004011a0 T mul_cpx_separatefile`

## Locality
The time of row and column summations compiled with `-O0, -march=native` flags was:

- Average (from 10000) elapsed time of **row** summation: 2.407893455 msec.
- Average (from 10000) elapsed time of **column** summation: 2.905988410 msec.

### Loop unrolling
Partially unrolling the loops by 5 elements improved the time **for the column
summation only**:

- Average (from 10000) elapsed time of **row** summation: 2.454047617 msec.
- Average (from 10000) elapsed time of **column** summation: 2.637670321 msec.


The original code compiled with 2nd level optimization (`-O2`) gave faster resuts:

- Average (from 10000) elapsed time of **row** summation: 1.031412517 msec.
- Average (from 10000) elapsed time of **column** summation: 1.420934522 msec.

### Loop unrolling
Partially unrolling the loops by 5 elements improved the time **for both cases**:

- Average (from 10000) elapsed time of **row** summation: 1.027971991 msec.
- Average (from 10000) elapsed time of **column** summation: 1.336912766 msec.

However, this approach gave no speed-up for a smaller number of repetitions (500).
