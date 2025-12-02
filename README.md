# CSCI-3656-Group-Project: Prey Predator Steady State Analysis with NonlinearSolve.jl and DifferentialEquations.jl

This project demonstrates how to compute and analyze steady state equilibria of classical prey predator systems using the Julia packages **NonlinearSolve.jl** and **DifferentialEquations.jl**. The workflow includes defining nonlinear systems, finding equilibria with multiple solvers, solving the corresponding ODE models, and visualizing the results.

## Overview

Prey predator interactions are commonly modeled with nonlinear differential equations. Steady states occur when both population derivatives are equal to zero. These fixed points help describe long term behavior such as collapse, coexistence, or oscillation. This repository provides examples of:

* Setting up nonlinear functions for use with **NonlinearSolve.jl**
* Solving for equilibria using multiple rootfinding algorithms
* Simulating dynamics with **DifferentialEquations.jl**
* Plotting timeseries and phase portraits
* Exploring more advanced prey predator models such as the Holling type II functional response

## Lotka Volterra System

The classical model is given by the pair of equations:

```
x' = αx - βxy
y' = -γy + δxy
```

The functions are implemented in two forms:

* A version returning an array for nonlinear solving
* An in place version for ODE simulation

These allow consistent use across both libraries.

## Solving the Nonlinear System

A steady state is computed by constructing a `NonlinearProblem` and solving it with a chosen solver. Example:

```julia
nonLinearProb = NonlinearProblem(lvNonLinear, u0, p)
nonLinearSol = solve(nonLinearProb, NewtonRaphson(); store_trace = Val(true))
```

This returns the equilibrium populations for the system. Additional solvers like `Broyden`, `DFSane`, and `TrustRegion` are also tested and benchmarked.

## ODE Simulation

To visualize the trajectory of the system from an initial condition, the in place ODE form is used with `ODEProblem` and solved with an efficient method such as `Rodas5P`.

Plots include:

* Population vs time
* Horizontal lines marking equilibrium values
* Phase portrait with the steady state highlighted

These visualizations confirm how trajectories approach or move around equilibria.

## Solver Benchmarking

A comparison is run across several nonlinear solvers. The following are reported for each:

* Convergence status
* Number of iterations
* Final solution vector
* Execution time

This helps demonstrate the behavior of different rootfinding algorithms when given the same model and initial guess.

## Holling Type II Extension

The same workflow is repeated for a model with a Holling type II functional response. This introduces saturation in predation and changes the analytic form of equilibria. The nonlinear solver may converge to different valid or trivial equilibria depending on solver choice and initial conditions.

The extended functions follow the same structure as the original ones, making it easy to switch between models.

## Requirements

To run all code, ensure the following packages are installed:

```julia
using NonlinearSolve
using DifferentialEquations
using Plots
using Profile
using ProfileSVG
```

## How to Use

1. Define parameters `p` and initial populations `u0`.
2. Create and solve the nonlinear system.
3. Simulate the ODE with the equilibrium or any other initial condition.
4. Visualize results with the plotting utilities provided.
5. Experiment with alternative solvers or models.

## Suggested Extensions

* Include stability analysis through Jacobian eigenvalues
* Explore bifurcations by sweeping parameters
* Add stochastic versions of the model
* Provide performance comparisons using BenchmarkTools

## License

Add your preferred license here.

## Acknowledgments

Built using the Julia scientific computing ecosystem, particularly **SciML** packages that enable flexible and efficient modeling of dynamical systems.
