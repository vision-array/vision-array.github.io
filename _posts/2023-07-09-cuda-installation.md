---
title: CUDA Installation on Ubuntu 22.04 in 10 mins
date: 2023-07-09 6:00:00 -500
categories: [hardware, gpu, cuda, ubuntu]
tags: [gpu, cuda, linux, ubuntu, drivers]
---

Hi All! This blog will help installing CUDA on Ubuntu 22.04 in the shortest way possible without having to go through the driver hassles.

> This blog assumes that the system doesn't have any pre-installed NVidia Drivers and addresses the NVidia CUDA as CUDA

Before proceeding to Install CUDA Toolkit on Ubuntu, let's try to understand what components of CUDA get installed and why they are needed.

### Components of the CUDA Toolkit and the Driver Package
When we perform the toolkit installation, both the CUDA Toolkit along with a compatible NVIDIA Display Driver Package gets installed. The NVIDIA Display Driver Package consists of both the CUDA Driver(libcuda.so - (so - shared object)) and the GPU Kernel driver(nvidia.ko - (ko - kernel object)) as shown in the figure below which is taken from the [CUDA Compatability Page](https://docs.nvidia.com/deploy/cuda-compatibility/)

I am planning to write a more detailed summary as to what each of the components do, but for now : The libcuda.so acts as the interface between the CUDA application and the NVIDIA driver, while nvidia.ko handles the low-level communication with the GPU hardware.

![CUDA Components from NVidia](https://docs.nvidia.com/deploy/cuda-compatibility/graphics/CUDA-components.png)

> The driver that comes along the toolkit supports all the features introduced in that version of the CUDA Toolkit.As it is stated on the NVIDIA website, it is highly recommended to install a compatible nvidia driver when installing cuda on a system.


### Verifying for a CUDA-capable GPU  

```bash
$ lspci | grep -i nvidia
```
### Verfiying the linux version support 

```bash
$ uname -m && cat /etc/*release
```
### Verfiying gcc for CUDA

```bash
$ gcc --version
```
### Verifying the System Kernel 

```bash
$ uname -r
```
### CUDA Installation from NVIDIA Developer 

 We will be using the Network repo installation by downaloading the toolkit from the Developer Link: https://developer.nvidia.com/cuda-downloads

> Once on the page, you might need to create an NVidia Developer Account to download the toolkit

You should see something similar to this on the Downloads page with the CUDA Toolkit 12.2 version.
![CUDA Toolkit Download Website](/assets/img/07-09-cuda-installation/07-09-cuda-installation-11-8-website.png)

> Lets install CUDA 11.8 as it is the latest version that is supported by the majority the current ML/DL training and inference framework versions like tensorflow, pytorch, tensorrt etc. 

After scrolling down, you should see an option: 
[`Archive of Previous CUDA Releases`](https://developer.nvidia.com/cuda-toolkit-archive), select that. 

Once on the new page, select the [`CUDA Toolkit 11.8.0 (October 2022), Versioned Online Documentation`](https://developer.nvidia.com/cuda-11-8-0-download-archive)

Select all the fields as required for the system 
![CUDA-System Fields ](/assets/img/07-09-cuda-installation/07-09-cuda-installation-11-8-fields.png)

 > To find out the system architecture in ubuntu, use the command below in a terminal 

```bash
$ uname -m
```
Follow all the commands 
```bash 
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-ubuntu2204-11-8-local_11.8.0-520.61.05-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2204-11-8-local_11.8.0-520.61.05-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2204-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
```
The last command has to be modified to ensure that the `apt-get` package manager installs the correct version i,e cuda 11.8.

```bash 
sudo apt-get -y install cuda-11-8
```

Hope this helps! 


