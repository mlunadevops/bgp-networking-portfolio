## Introduction & Key Concepts

By default, BGP selects only a single best path to reach a destination network and does not perform load balancing[cite: 3]. When multiple paths exist between a local AS and a remote AS, BGP chooses just one based on its path selection algorithm.

To achieve load sharing with equal-cost paths, you can use the BGP configuration command:

 ```text
maximum-paths [1-6]

 ```
