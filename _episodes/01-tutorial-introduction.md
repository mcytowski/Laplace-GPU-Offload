# Tutorial Introduction

## Organisers
Tom Papatheodore (ORNL), Maciej Cytowski (PawseySC), Chris Daley (LBL)

## Abstract

OpenACC and OpenMP are often seen as competing solutions for directive-based GPU offloading. Both models allow the programmer to offload computational kernels to GPUs and to manage data transfers between CPU and GPU memories. OpenACC is said to be a descriptive approach to programming GPUs, where the programmer uses directives to tell the compiler where data-independent loops are located and lets the compiler decide how/where to parallelise the loops based on the architecture (via compiler flags). OpenMP, on the other hand, is said to be a prescriptive approach to GPU programming, where the programmer uses directives to more explicitly tell the compiler how/where to parallelise the loops, instead of letting the compiler decide.

It’s common to hear programmers ask, *which programming model should I use?*, *which approach is more portable?*, *are one of these models going to replace the other?*, etc.

<p style="text-align: center;"><b>In this tutorial, we will not attempt to argue for one programming model over the other or specifically try to compare their performance profiles.</b></p>

Instead, we will elaborate on the differences between the two approaches briefly outlined above and give participants the opportunity to explore how these differences manifest themselves in a program during hands-on exercises. We will also give a current snapshot of the compiler implementations available for the OpenACC and OpenMP (offload) specifications.

The hands-on part of this tutorial uses a basic serial implementation of a 2D Laplace equation solver (written in C programming language). Participants will be guided step-by-step through both OpenACC and OpenMP implementations.    

## Description

### Goals

### Targeted audience
Participants are expected to be familiar with GPU architecture, the concept of offloading computations to accelerators and one of the two discussed programming models (OpenMP or OpenACC). Participants should be familiar with C/C++ programming language.

### Detailed outline
* Talks (1.5h)
    * Welcome and Introduction (10 min)
    * Directive-based GPU offloading - current status and future (25 min presentation + 5 min Q&A)
    * Compiler support for OpenACC and OpenMP GPU offloading (25 min presentation + 5 min Q&A)
    * Summit/Ascent Introduction & login information (20 min)
* Afternoon session (2h): Hands-on Exercise - 2D Laplace equation solver in OpenACC and OpenMP

### Description of hands-on exercises
Participants will develop OpenACC and/or OpenMP versions of the Laplace equation iterative solver. Participants will follow a detailed step-by-step developer guide which will be available on github for both OpenACC and OpenMP codes. Participants will use PGI and IBM compilers on Summit/Ascent system (ORNL) as well as PGI and Clang compilers on Topaz (Pawsey). Additionally participants will be provided with information about advanced compiler options (e.g. compiler feedback) and profiling.
Participants, depending on their experience and interest, will be able to choose different paths:
1. Step-by-step implementation of both OpenMP and OpenACC versions - intermediate participants with little experience on both OpenMP and OpenACC
2. Step-by-step implementation of OpenX and experimenting with compiler options and profiling - advanced participants with little experience on OpenY

All materials will be published on github repository for further reference and reuse.

## Logistics
### Length
1/2 day tutorial

### Percentage split: beginner, intermediate, advanced
Intermediate (50%) and Advanced (50%)

### Requirements for attendees
Participants are required to bring their own laptops with SSH client for the hands-on session. Participants will be provided with access to the HPC platform.

### Estimated number of attendees
The targeted number of attendees: 40-60

## References
1. https://devblogs.nvidia.com/openacc-example-part-1/
2. https://devblogs.nvidia.com/openacc-example-part-2/
3. https://sc19.supercomputing.org/presentation/?id=tut110&sess=sess183
4. https://www.hpcwire.com/2016/06/29/compilers-openacc-openmp-back/
5. https://github.com/dappelha/openmp-laplace
