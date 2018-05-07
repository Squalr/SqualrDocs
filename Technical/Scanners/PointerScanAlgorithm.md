## Pointer Scan Algorithm

Pointer scans are unfortunately an [NP-Hard](https://en.wikipedia.org/wiki/NP-hardness) problem that can be thought of as an "all possible paths" problem.

A useful pre-reading is the paper "SigPath: A Memory Graph Based Approach for Program Data Inspection and Modification", which covers another pointer scanning algorithm. Their algorithm is too slow for our purposes, but inspires some of the ideas behind this algorithm.

### Problem Definition

Let's define a candidate pointer as an address with a value that appears to be a pointer to another address contained in the heap, within +/- a tolerance (generally < 4kb).

Let L represent the maximum pointer scan depth, and L_i represent the current depth

Let D be the destination or target address, to which we would like to be able to reliably locate in the process.

Let {H} be the set of all candidate pointers in the heap. Let {H(L_i)} represent all candidate pointers at a particular depth.

Let {S} be the set of all candidate pointers in static memory

This problem becomes: How do we use this information to reliably locate D across multiple program runs. This can be broken down into the following cases:
1. {S} already contains D. This means that D is static, and a pointer scan is uneccessary.
2. {S} points directly to D, but does not contain it. In other words, D is in a heap allocated directly by {S}
3. {S} points to D through an arbitrary {H(L)}. This case will require the bulk of our attention.

The intuitive but naive approach would be to find all paths from {S} to D through {H}, but unfortunately this grows exponentially with L, resulting in an algorithm that is not very practical.

