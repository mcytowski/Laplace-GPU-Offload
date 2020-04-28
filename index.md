---
layout: lesson
root: .
permalink: index.html  # Is the only page that don't follow the partner /:path/index.html
---
# Organisers
Tom Papatheodore (ORNL), Maciej Cytowski (PawseySC), Chris Daley (LBL)

OpenACC and OpenMP are often seen as competing solutions for directive-based GPU offloading. Both models allow the programmer to offload computational workloads to run on GPUs and to manage data transfers between CPU and GPU memories. OpenACC is said to be a descriptive approach to programming GPUs, where the programmer uses directives to tell the compiler where data-independent loops are located and lets the compiler decide how/where to parallelize the loops based on the architecture (via compiler flags). OpenMP, on the other hand, is said to be a prescriptive approach to GPU programming, where the programmer uses directives to more explicitly tell the compiler how/where to parallelize the loops, instead of letting the compiler decide.
 
It’s common to hear programmers ask, “which programming model should I use?”, “which approach is more portable?”, “are one of these models going to replace the other?”, etc. In this tutorial, we will not attempt to argue for one programming model over the other or specifically try to compare their performance profiles. Instead, we will elaborate on the differences between the two approaches briefly outlined above and give participants the opportunity to explore how these differences manifest themselves in a program during hands-on exercises. We will also give a current snapshot of the compiler implementations available for the OpenACC and OpenMP (offload) specifications.


> ## Prerequisites
>
> No particular prerequisites are required.  All command line actions can be copied from this course.  If you have a working 
> knowledge of the basics of the unix command line (ie: mv, ls, cp) you will have slightly easier time in this course.
{: .prereq}
