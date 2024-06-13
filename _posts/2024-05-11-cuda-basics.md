---
title: CUDA Basics
date: 2024-05-11 6:00:00 -500
categories: [cuda, AI, Deep Learning, hardware]
tags: [cuda, ML, AI, deep learning, hardware, gpu]
math: true
---

# CUDA basics 
CUDA 
Basic Syntax for defining a kernel with a grid size and thread blocks

Host Code: Code that runs on CPU 

Device Code: Code that runs on GPU 

Grid: collection of all threads launched for a kernel - distributed into thread blocks

Thread blocks: a set of threads in a grid grouped together 



Syntax given for a 32 thread : 
```
dim3 block(4,1,1);
or 
dim3 block(4); ( it will be implicit that the number of threads in the other two dimensions, y and z will be 1 and 1 respectively)
```
So, in the above code - it means that we want each thread block to have four threads each. 
```
dim3 grid(8, 1, 1); 
or 
dim3 block(8); (so it is implicit similar to the block declaration)
```
and that the grid defined for the kernel has 8 thread-blocks 

The first keyword `dim3` is used to denote the number of dimensions of the thread block and the grid size. It is a cuda structure defined for storing the grid and the block dimensions

So. lets say we have a 64 core gpu (each core able to run one single thread), then we can define a kernel with the following thread-block and grid dimensions ( using all the three dimensions). 
```
dim3 block(4, 4, 4); 
dim3 grid(1, 1, 1);


or 

dim3 block(8, 2, 1);
dim3 grid(4, 1, 1); # meaning there are four thread blocks in grid.x and 
then each thread block: thread.blk.x - 8 threads in x and thread.blk.y - 2 threads in y 
```
Also, there are limitations to the total number of threads in a thread block based on the x, y, z dimensions as follows:
- x: 1024 
- y: 1024 
- z: 64 
and for the grid with the totla number of thread blocks as: 
- x: 2^32 - 1 
- y: 65536 
- z: 65536
So, once we define the sizes of the - we can use them in writing kernel functions as follows: `Simple function declaration and definition`
```
Simple GPU function declaration and definition
----------------------------------------------------------

print_helloworld<<<grid, block >>>(); # declaration - generally in the main function for simple programs 

# it is essentially to say function_name<<< num of blocks, num of threads per block>>>();
----------------------------------------------------------

__global__ void print_helloworld(){
    printf("Hello World!");
}
```
The function definition is similar to that of a C/C++ function defintion with a return type before the function name and then the function with the argument list in the parenthesis.

We also have the following modifiers - we use the `__global__` modifier for the above example

```
__device__ 
__host__
__global__
``` 

