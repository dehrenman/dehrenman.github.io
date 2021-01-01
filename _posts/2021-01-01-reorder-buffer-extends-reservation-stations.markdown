---
title: "The Re-Order Buffer (ROB) is an Extension of the Reservation Stations for the Tomasulo Algorithm"
layout: post
---

The Reorder Buffer frees up reservation stations by being a "bin" that the results of completed instructions can fall into, thereby making it so that reservation station entries don't have to sit there and wait for execution to complete on their associated functional units.

See [here](https://www.youtube.com/watch?v=TnxJA6uyP7Y&list=PLrE4w2q_V9BlRr1vmHLCCNQR-3shPkxOp&index=1) for an excellent trilogy of short, simple, but clear and complete videos on the ROB. 12 minutes total, understanding guaranteed.

(never trust anyone on the internet who guarantees you anything...but still they're good videos)

# A Little Background

Most modern processors implement out-of-order execution with Tomasulo's algorithm, which allows long-latency instructions (memory, fmul, fdiv) and instructions with unresolved dependencies (because their dependencies are produced by these long-latency instructions) to wait in Reservation Stations associated with each functional unit while other instructions coming afterward can continue to execute. The algorithm (really, it's less of an "algorithm" and more of an architecture or a procedure) dates back to the 1966 processor [IBM 360/91](https://en.wikipedia.org/wiki/IBM_System/360_Model_91).

An understanding of Tomasulo's original reservation-station based algorithm is critical to understanding modern out-of-order processors, but depending on the source ([this](https://www.youtube.com/watch?v=jyjE6NHtkiA&t=116s) being a particularly bad one), you may be assulted with confusing analogies, irrelevant anecdotes, and, worse of all, long, tedious, manually performed cycle-accurate processor simulations that give more of an illusion of understanding than anything else.

I found the GeorgiaTech videos recommended in the opening of this page after lots of browsing and wasting 40 minutes watching all 3 takes of the bad videos... and I want you to avoid having to do this. Again, the good videos are [here](https://www.youtube.com/watch?v=TnxJA6uyP7Y&list=PLrE4w2q_V9BlRr1vmHLCCNQR-3shPkxOp&index=1).
