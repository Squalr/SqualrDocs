## Pointer Scan Algorithm

Pointer scans are unfortunately an [NP-Hard](https://en.wikipedia.org/wiki/NP-hardness) problem that can be thought of as an "all possible paths" problem.

A useful pre-reading is the paper "SigPath: A Memory Graph Based Approach for Program Data Inspection and Modification", which covers another pointer scanning algorithm. Their algorithm is too slow for our purposes, but inspires some of the ideas behind this algorithm.

### Problem Definition

Let's define a candidate pointer as an address with a value that appears to be a pointer to another address contained in the heap, within +/- a tolerance (generally < 4kb).

Let D be the destination or target address, to which we would like to be able to reliably locate in the process.

Let {H} be the set of all candidate pointers in the heap

Let {S} be the set of all candidate pointers in static memory.
