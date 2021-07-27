# Parallel solver dummies for preCICE

This repository currently contains three solver dummies for the coupling library [preCICE](https://github.com/precice/precice) that can be executed in parallel. The solver dummies can also be mixed with each other, i.e., each solver can play any role in the coupling!

Current solver dummies included
- C++ solver dummy: `cpp/solverdummy-cpp-parallel.cpp`
- Fortran solver dummy: `fortran/solverdummy-fortran-parallel.f90`
- Python solver dummy: `python/solverdummy-python-parallel.py`


## Requirements

All dummies:
- preCICE v2 (only tested with v2.2.0)

C++ dummy:
- CMake
- A C++ compiler
- MPI (only tested with OpenMPI)

Fortran dummy:
- CMake
- A Fortran compiler
- MPI (only tested with OpenMPI)

The Python dummy additionally needs:
- Python 3
- mpi4py
- [Python bindings for preCICE](https://github.com/precice/python-bindings)

If you have preCICE and the Python bindings installed, you should have all requirements installed.

## Compilation


### C++

Compile the C++ solver dummy and copy it back to the `cpp` directory:

```
cd cpp
mkdir build
cd build
cmake ..
make
cp solverdummy-cpp-parallel ..
cd ..
```

### Fortran

Compile the Fortran solver dummy and copy it back to the `fortran` directory:

```
cd fortran
mkdir build
cd build
cmake ..
make
cp solverdummy-fortran-parallel ..
cd ..
```

## Test solvers

`N` and `M` refer to the number of ranks one want to use. Start with `1` in order to run the serial computation and increase `N` and `M` afterwards to have truly parallel computations.

Further information:
- `N` and `M` may be different numbers.
- The solver have to run in different shells or you have to send the first solver to the background.
- You pass the participant's name and the corresponding mesh name on the terminal. The name and mesh have to be one of the following
    - `SolverOne` and `MeshOne`
    - `SolverTwo` and `MeshTwo`
- Replace `mpirun` by the suitable MPI wrapper of your machine.
- Step into the directory of the solver you want to use, i.e. `cd cpp/` for the C++ solver.
- If you copy the solvers into other directories, please adapt the file `precice-config-parallel.xml` accordingly.

**Note** You can mix the different dummies arbitrarily with each other. Some examples are given below:
### Setup 1: C++ and C++

```
mpirun -n N ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n M ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverTwo MeshTwo
```

Example:
```
mpirun -n 4 ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n 2 ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverTwo MeshTwo
```

### Setup 2: Python and Python

```
mpirun -n N python3 solverdummy-python-parallel.py ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n M python3 solverdummy-python-parallel.py ../precice-config-parallel.xml SolverTwo MeshTwo
```

Example:
```
mpirun -n 4 python3 solverdummy-python-parallel.py ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n 2 python3 solverdummy-python-parallel.py ../precice-config-parallel.xml SolverTwo MeshTwo
```

### Setup 3: C++ and Python

```
mpirun -n N ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n M python3 solverdummy-python-parallel.py ../precice-config-parallel.xml SolverTwo MeshTwo
```

Example:
```
mpirun -n 4 ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n 2 python3 solverdummy-python-parallel.py ../precice-config-parallel.xml SolverTwo MeshTwo
```

### Setup 4: Fortran and C++

```
mpirun -n N ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n M ./solverdummy-fortran-parallel ../precice-config-parallel.xml SolverTwo MeshTwo
```

Example:
```
mpirun -n 4 ./solverdummy-cpp-parallel ../precice-config-parallel.xml SolverOne MeshOne
mpirun -n 2 fortran ./solverdummy-fortran-parallel ../precice-config-parallel.xml SolverTwo MeshTwo
```

## Remarks

- The codes are heavily based on the solver dummies of [preCICE](https://github.com/precice/precice) and the corresponding [Python bindings](https://github.com/precice/python-bindings).