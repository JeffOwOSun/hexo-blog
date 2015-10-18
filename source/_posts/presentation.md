title: COMP5621 Paper Presentation
id: 114
categories:
  - Uncategorized
date: 2015-04-29 15:04:55
tags:
---

The script for the paper presentation for COMP5621 Computer Network Course in 2015 Spring @ HKUST

<!--more-->

Today I’m going to show you a framework of software routers called PacketShader, A GPU-accelerated Software Router Framework. So first of all, what is a GPU?

GPU is short for graphics processing unit. It’s a very common piece of equipment insider every computer that handles the rendering of displays. The most common use case for a GPU is for 3D gaming, and if you have any background in PC gaming you’ll know that a faster GPU gives better visual experience. But when we think about routing, this procedure of looking up the port forwarding table and matching the longest subsequence of destination IP address, we come a question, is GPU really a suitable tool for network processing?

To answer this question we need to go a step further to get a closer look of GPU architecture. And the most significant characteristics of a GPU is that it’s highly parallel. This is the architecture of NVIDIA GTX480, which was once the flagship graphic card in the year 2012\. It consists of 15 streaming multiprocessors, or SMs, and each SM in turn consists of 32 streaming processors, or SPs. You multiply 15 with 32 and it gives 480 streaming processors in total. by comparison, a typical CPU consists only of 4 cores altogether. 480 vs 4, this is why we say GPU is parallel. And one reason for GPUs to achieve this parallelism is that its stream processors are simpler than cores in a CPU, so there is space for more cores.

Interestingly the process of packet forwarding is also highly parallel. Imagine I am one processor, and a packet come in, I pick it up, look at the destination IP address, and I check the forwarding table to do a longest prefix matching and put the packet inside the correct box and I can continue on. If there are two processors then they can work at the same time without interfering each other. If everyone of you is a processor, sure enough you can still work at the same time without problem. This inherent parallelism of network processing perfectly matches the parallel nature of GPU architecture. And in fact, GPU may even be a better choice than CPUs.
<div></div>
In practice, a GPU works in collaboration with CPU. CPU handles network input and output and other stuff, while it offloads compute-intensive and memory intensive jobs to GPU, including the longest prefix matching algorithms. Let’s look at the the basic workflow of packet shader, doing the IPv6 longest prefix matching.

The first step is called pre-shading. In this step, CPU collects incoming packets from network interface cards and put them in the input queue for further process. This bulking process makes it faster to transmit large number of packets.

The next step is called shading. First of all, the CPU transmits data including port forwarding table and the bulk of packets to the GPU memory and binary code called Kernel to be run on GPU also to the device memory.

Then CPU sends a signal that triggers the Kernel. The kernel distributes the packets to each SP and SPs do the longest prefix matching. When the matching is done, information of the outgoing ports for the packets is again copied back to Host Memory

The last step is called Post-shading, and the packets are sent out through designated ports.

We see that GPU is suitable for processing packet forwarding. In fact there are other reasons that makes it a better candidate over CPU.

First of all, GPU is engineered with higher memory bandwidth. It allows lower latency and faster random access. The random access is important for packet routing, because normally the forwarding table would be stored in a hash table, and looking at hash tables is a process of random memory access.

The second advantage is that because SPs are simpler in structure, there is more space to build Arithmetic Logic Units, or ALUs, so that the raw computing power is even higher than that of CPU.

The third reason which is by no means trivial, is that GPU’s are cheap. In the paper, the test environment consists of two CPUs and two GPUs. We can see that the Unit price for NVIDIA GTX480 is almost half of that of Intel Xeon CPU

Through comparison we can see that GPU acceleration indeed gives a better performance. The first diagraph is a preliminary test on longest prefix matching without interacting with Network Interface Cards. We can see that as number of packets increases, the GPU displays a dominating performance boost.

The second diagram shows that in the application of IPv6 Forwarding, which is very intense because IPv6 has 128bit long IP addresses, the GPU keeps giving multi-10 gigs speed even when the packet size is small.

In conclusion, GPU with its parallel computing ability, enables software routers to achieve multi 10 Gbps performance.
