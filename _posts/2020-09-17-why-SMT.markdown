---
title: "SMT doesn't make sense on small processors? Wrong!"
layout: post
---

This post stems from a discussion on Reddit about SMT. Credit to /u/nekoxp for a lot of the stuff below.


# Why SMT

Usually, architects implement Simultaneous Multi-Threading when the architects catch their processors waiting for long-running and unpredictable dependency-chained operations, wasting resources and keeping other threads on hold. There is a bit to unpack here, and let's do it in reverse order.

So "putting other threads on hold" part means that it makes no sense to implement SMT on one-thread processors. So no SMT on your laundry machine microcontroller. Also, for the part on "wasting resources", it makes slightly less sense to do SMT on small processor cores that don't have a lot of execution units - - it's one thing to waste just an ALU waiting for a page walk, and another thing to waste 8 Vector/scalar units, 2 branch units, and 8 load/store units on the IBM Power 9 (to be fair, I am referencing a core that's specifically designed for 8-way SMT, which is the most SMT ever).

![Lots and Lots of Execution Units in IBM Power9-SMT8](/images/2020-09-17-why-SMT-powerSMT8.png)

Getting back to the operations we wait for, it might be helpful to explain what these long-running dependency generating operations might be. The dependency-chain part is important: if there are no dependency chains, out of order processing would have eliminated the wait. The unpredictability is also important. A predictable long-running operation would be disk I/O. It always takes a long time and this opens up the opportunity to do non-simultaneous multi-threading:

Suppose that I'm waiting for disk. Then compared to SMT it actually makes more sense for the OS to intervene and swap my current thread out and another thread in until disk IO finishes. This is especially true in the age of DMA, as the DMA controller can be set to alert the OS to swap my thread back in upon disk return.

Instead, a better example for this is memory and network. For memory, there's really no way to tell if the cache hierarchy returns the requisite data in 3 cycles (after, say, a L1 hit) or 3000 cycles (after, say, 24 memory accesses to get the physical address and find the data). Similarly, there's no telling when a packet would arrive from net. Hence for applications with a lot of out-of-cache memory access and a lot of networking, SMT makes sense.

![Lots and Lots of Memory Accesses in a Page Table Walk](/images/2020-09-17-why-SMT-pagetablewalk.png)

# Why SMT Sometimes Good on Tiny Cores?

Infrastructure! Prime example, ARM Neoverse E1 and N1. N1 all about compute, no SMT. E1 all about throughput, hence SMT!

I'm note entirely sure about how the N1 and E1 compare in other aspects, other than the E1 sounds like it's smaller than the N1, and appears in smaller clusters. It would be swell if somebody could find something on the execution units they each have. Or I might someday.
