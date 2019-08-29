---
title: "Speculation Techniques used in modern processors"
layout: post
---

I found a fleeting mention of speculation techniques reading the introduction section of "FoMR: Speculative Superoptimization: Boosting Performance via Speculation-Driven Dynamic Binary Optimization".

It lists the following:
- branch prediction
- loop stream detection
- load value prediction
- memory disambigulation
And added an annoying ", etc." to the end of the list.

In this post I'm going to attempt to explain each of the above, and speculate a bit about the "etc." at the end of the list.

## Why Speculate

Instructions are not atomic operations. On a classic RISC (Restricted Instruction Set Computer), each instruction actually goes through five steps forming the classic 5-step RISC pipeline.

### Single-cycle speculation: Get ahead in the pipeline

Because each stage of the pipeline calls for a different resource on the CPU, the CPU can actually be running multiple instructions at the same time, just at different stages of the pipeline, as shown below where `t` is time and `i` is the instruction number.

![Using the whole pipeline at once with each instruction at a different step](/assets/600px-Fivestagespipeline.png)

Now suppose that we're happily chugging along this pipeline, and the next instruction is a conditional jump. The most natural response is to not start the instruction fetch stage of the next instruction until this instruction completes executing.  But that wastes basically four cycles of time, and in short loops may carry HUGE performance costs.

So we speculate. Using simple rules, we guess what the current instruction will yield and start running the next instruction based on that speculated result, before the actual result is out. If we guessed right, good! If not, we discard the result of the next instruction we guessed, find the proper next instruction based on the true result of the current instruction, and redo the execution.

This results in huge savings (##TODO##: how much?)

### Multi-cycle speculation: grab dat memory data

There are some situations where an instruction takes multiple cycles, such as when it needs to
wait for something to be loaded from memory to cache. In these cases, what needs to be done
is to speculatively do the cache loading waaay ahead of time. This topic deserves a bit more,
so I'm putting a ##TODO## tag here.




I'm giving a short summary here, but I'll write up another article on the pipeline later.

- IF (Instruction Fetch)

   The instruction is fetched from memory (`*pc`) and stored into the instruction register.
   At the end of the IF stage, PC is incremented.

- ID (Instruction Decode)

   The instruction is resolved to the correct processing unit and the memory addresses
   involved are resolved.

- EX (Execute)
   
   The control unit of the CPU passes information yielded by the ID stage to the relevent
   parts of the CPU, such as the ALU, the bitshifter, and perhaps a multiplier / divider.

- MEM (Memory accessa)

   If memory access needs to be done, it is done here.
   I'm not sure why this happens before the execution stage. I'll need to ask and make an update
   ##TODO## Update this.

- WB (Register Writeback)

  Results are written back to the register file.
  I wonder what "file" means in "register file".



### Branch Prediction

