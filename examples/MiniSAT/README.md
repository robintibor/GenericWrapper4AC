#Example for using the Generic Wrapper for MiniSAT

This example shows how we used the generic wrapper in the Configurable SAT Solver Challenge (CSSC).
As a SAT solver, we use here the well-known solver [MiniSAT](http://minisat.se/).

As a general interface to SAT solvers, we provide the class `SATCSSCWrapper.SatCSSCWrapper`.
It implements the two necessary functions:

  * `get_command_line_args()`
  *  `process_results()`
  
The function `get_command_line_args()` simply loads a solver-specific script, which can be specified on the command line by `--script`. 

The function `process_results()` does the following steps:

  1. Reading the output file of the solver
  1. Searches for `SATISFIABLE` or `UNSATISFIABLE` and checks the result if possible.
    1. `SATISFIABLE` can be checked with one of the following ways:
      * With the option `--sat-checker`, you can provide the binary of the SAT checker tool from the SAT competition; this will explicitly check the returned variable assignment.
      * With the option `--sol-file`, you provide a CSV file (whitespace separated) that includes an entry for each instance in the format `<instance name> <SATISIFABLE|UNSATISFIABLE>`
      * The instance specific is set to either `SAT` or `UNSAT`
    1.   `UNSATISFIABLE` can be checked with the two latter ways of checking `SATISFIABLE`
    
The script `MiniSATWrapper.py` implements only the function to get the MiniSAT call. It gets the same inputs as `get_command_line_args()` and returns a string.

You can call the MiniSAT wrapper with:

`python examples/MiniSAT/SATCSSCWrapper.py --script examples/MiniSAT/MiniSATWrapper.py examples/MiniSAT/gzip_vc1071.cnf SAT 10 0 42 -rnd-freq 0 -var-decay 0.001 -cla-decay 0.001 -gc-frac 0.000001 -rfirst 1000`

This will run `MiniSat` on the instance `gzip_vc1071.cnf`, assuming that it is a `SATISFIABLE` instance, using at most 10 CPU seconds and a random seed of 42. This will be translated in the following call:

`examples/MiniSAT/minisat -rnd-seed=42 -cla-decay=0.001 -var-decay=0.001 -gc-frac=0.000001 -rfirst=1000 -rnd-freq=0 examples/MiniSAT/gzip_vc1071.cnf`

The output of the wrapper call will include:

`Result for ParamILS: SAT, 0.004, -1, -1, 42, SAT checker was not given; could not verify SAT`

With this line, ParamILS/SMAC will know that `MiniSat` returned `SATISFIABLE` and needed 0.004 CPU seconds.
 