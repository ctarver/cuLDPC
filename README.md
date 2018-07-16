# cuLDPC
#### CUDA implementation of LDPC decoding algorithm

## Description
This is a CUDA-based software implementation of LDPC decoding algorithm. The code was developed by the authors for research purpose. 

This detailed explanation of the algorithm can be found from the following papers (you can find them in /doc directory):

> 1. G. Wang, M. Wu, Y. Sun and J. R. Cavallaro, "A massively parallel implementation of QC-LDPC decoder on GPU," 2011 IEEE 9th Symposium on Application Specific Processors (SASP), San Diego, CA, 2011, pp. 82-85.

> 2. G. Wang, M. Wu, B. Yin and J. R. Cavallaro, "High throughput low latency LDPC decoding on GPU for SDR systems," 2013 IEEE Global Conference on Signal and Information Processing (GlobalSIP), Austin, TX, 2013, pp. 1258-1261.

> 3. M. Wu and G. Wang, Massively Parallel Signal Processing for Wireless Communication Systems, GPU Technology Conference (GTC) 2013. March 18-21, 2013, San Jose, California.

## Algorithms
The code implemented Quasi-cyclic LDPC code decoder. The set up is:
* 802.16m WiMax and 802.11n
* Min-sum algorithm and SPA algorithm

## Disclaimer
When the code was developed in 2013, the authors used the following development environment: 
* a PC installing Ubuntu Linux OS
* Intel i7 CPU
* 16GB RAM
* four NVIDIA Titan GPUs and one GTX-690 GPU
* CUDA v4/v5.

The code was debugged based on the above machine. There might be issues directly run the code on older or newer GPUs. Due to the device limitation, we cannot test the code on any other devices. 

The code may not reflect all the optimizations in the paper, but it implemented most of the ideas in the above papers. You can easily tweak the parameters to make the code work for your systems.

The code is provided as is, and it can be used for any purpose, e.g., studying LDPC decoding, perform LDPC-related research, and so on. If you use this code for your research, please cite the papers listed above. 

## 2018 Revist
In 2018, the code was revisited. It was reran on a system with the following specs:
* Intel(R) Core(TM) i7-3930K CPU @ 3.20GHz
* Ubuntu 16.04.3
* Linux kernel 4.4.0-112
* Cuda 9.1

To get things to compile, I found that I needed to add -D_FORCE_INLINES on the nvcc commands in the Makefile. Otherwise, I would get an error saying that ‘memcpy’ was not declared in this scope while building.  

### Build and Run Procedures
cd into the src directory and run make
After it finishes compliling, run ./app for the simulation to begin.

### Running
When running ./app, it runs for a very long period of time. Looking at the nvidia-smi command, it seems to be working on only 1 GPU. When looking at htop, the command seems to be saturating only 1 core of the CPU. The nvidia-smi output is below. 

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 387.26                 Driver Version: 387.26                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX TITAN   Off  | 00000000:04:00.0 Off |                  N/A |
| 36%   56C    P8    12W / 250W |     13MiB /  6082MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce GTX TITAN   Off  | 00000000:05:00.0  On |                  N/A |
| 35%   52C    P8    25W / 250W |    403MiB /  6079MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   2  GeForce GTX TITAN   Off  | 00000000:08:00.0 Off |                  N/A |
| 30%   41C    P8    10W / 250W |     13MiB /  6082MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   3  GeForce GTX TITAN   Off  | 00000000:09:00.0 Off |                  N/A |
| 58%   80C    P0   125W / 250W |    384MiB /  6082MiB |     93%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    1      3065      G   /usr/lib/xorg/Xorg                           235MiB |
|    1      3826      G   compiz                                       152MiB |
|    3     27200      C   ./app                                        371MiB |
+-----------------------------------------------------------------------------+
```


The  console periodically updates with somthing like:

```
=================================
GPU CUDA Demo
SNR = 3.0 dB
# codewords = 200960, # streams = 16, CW=2, MCW=40
number of iterations = 0.0 
CPU time: 12407.197266 ms, for 500 simulations.
Throughput = 118.847145 Mbps
Throughput (kernel only) = inf Mbps
Throughput (kernel + transer time) = 24.122707 Mbps

h2d (llr): size=0.737280 MB, bandwidthInMBs = 92.046425 MB/s
d2h (hd): size=0.737280 MB, bandwidthInMBs = inf MB/s
kernel time = 0.000000 ms 
h2d time = 3819.404053 ms 
d2h time = 0.000000 ms
memset time = 1.062716 ms 
time difference = 8586.730469 ms 
# codewords = 12560, CW=2, MCW=40
total bit error = 96000
BER = 6.63e-03, FER = 6.37e+00

=================================
```

## License
    Copyright 2016 Guohui Wang

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
