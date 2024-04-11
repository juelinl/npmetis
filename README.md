# Project: C++ Metis K-Way Partitioning Wrapper with NumPy Integration

# Overview:
This project provides a user-friendly C++ wrapper library around the robust Metis K-Way graph partitioning algorithm, enabling seamless usage with versatile NumPy arrays for input and output data representation.

# Objectives:
Metis Integration: Core functionality focuses on encapsulating the Metis K-Way partitioning API within a well-designed C++ interface and provides easy-to-use binary.
NumPy Compatibility: Implement conversion mechanisms between Metis data structures and NumPy arrays, ensuring effortless data flow in common scientific computing pipelines.

Components:
1. C++ Files `cppmetis`: Define classes, functions, and data types for representing graphs (adjacency structures) and interacting with the Metis library.
2. Wrapper functions for Metis API calls.
3. Build scripts: Manage project compilation and linking, including dependencies on Metis and NumPy libraries.
4. Metis v5.0, ParMetis v4.0, MT-Metis v0.7, cnpy, oneTBB

# Potential Use Cases:
You want to use Metis to conduct kWay partitioning but found it difficult to use/slow.

# Prerequisites:
## Single Threaded: 
C++ compiler supoorting C++ standard 20. Ninja, CMake.

## Multi Threaded:
OpenMP

## Distributed:
MPI

# How to compile:
```bash
./build.sh
```

The output binary files will be in the `./bin` directory.

# How to use:
```shell
./bin/main \
--num_partition="[number of partitions (Required)]" \
--indptr="[path to indptr file (Required)]" \
--indices="[path to indices file (Required)]" \
--output="[path to output file (Required)]" \
--node_weight="[path to node_weight file (Optional)]" \
--edge_weight="[path to edge_weight file (Optional)]" \
```

To use the multi-threaded version, simply change `./bin/main` with `./bin/mt_main`.

To use the distributed version, simply change `./bin/main` with `mpirun -np [number of process] ./bin/mpi_main`. Note, `./bin/mpi_main` is buggy and any contribution is welcome! :smile:

All the input files must end with .npy and are NumPy arrays stored in int64_t format.

Metis assumes the graph is symmetrical, meaning that if `(u -> v)` then `(v -> u)` and the graph must be undirected.

The `indptr` file must be ended with either `indptr_sym.npy` or `indptr_xsym.npy`. 

If the `indptr` file ends with `indptr_xsym.npy`, additional steps will be taken to convert it into symmetrical graphs.
If the `indptr` file ends with `indptr_sym.npy`, it will be provided to Metis directly.

Similarly, the `indices` file must be ended with either `indices_sym.npy` or `indices_xsym.npy`. 

If the `indices` file ends with `indices_xsym.npy, additional steps will be taken to convert it into symmetrical graphs.
If the `indices` file ends with `indices_sym.npy`, it will be provided to Metis directly.

The `node_weight`, if provided, must have a length equal to the number of nodes in the graph.

The `edge_weight`, if provided, must have a length equal to the number of edges in the graph.

The output files must be ended with `.npy` and the output will be a int64_t NumPy array.