# gauparse
 This is a parser for Gaussian16 log file that works with vanilla Python 3.6+ without installing libraries.

## Usage
```sh
$ gauparse [-h] [-l] [-m] [-r] [-s] [-x] [-a] [-v] gaussian_log_file
```
The following standard output is obtained when gauparse script is executed to the log file of a geometry optimization with 5 steps.
```
#= 0.0001007369(   0.0632 kcal/mol), level=SCF
001|##########################################################
002|#######
003|#
004|:    0.0000002585
005| Emin = -76.462047342
        Item               Value     Threshold  Converged?
 Maximum Force            0.000003     0.000015     YES
 RMS     Force            0.000002     0.000010     YES
 Maximum Displacement     0.000004     0.000060     YES
 RMS     Displacement     0.000003     0.000040     YES
Stationary point with lowest energy
```

- The relative energy change of each step in the geometry optimization is plotted by using “#”. 
    - The value at the first line of the standard output indicates the energy gap per "#". This value is determined by the terminal window size.
    - When the relative energy from the energy minimum is less than the energy gap represented by a single "#", the value in Hartree is displayed after a colon.
    - At the energy minimum, “Emin = {absolute energy}” is output, followed by the result of the geometry optimization convergence checking.

## Options
- **-m** : Generate a restart file with the most stable geometry in the geometry optimization.
    - When the original input file name was "test.gjf", gauparse with this option gemerates a restart file like "test.**M026**.gjf".[^M] Three digits after "M" shows the number of geometry optimization steps.[^AboutStep] When the calculation was **n**ormally **t**ermine**d**, "**ntd**" appears instead of the three digits.  
    - The restart file has the same keywords as the original input file.
- **-l** : Generate a restart file with the last geometry in the geometry optimization.[^dl]
    - When the original input file name was "test.gjf", gauparse with this option gemerates a restart file like "test.**L026**.gjf".[^L]
- **-s** : This is option for scan(modredundant) calculation. When the original input file name was "test.gjf", gauparse with this option gemerates a file like "test.**S026**.gjf"[^S] that contains all stationary point geometries obtained in the optimization.[^link] This file is only for visualizing structural changes  in GaussView and should not be used for recalculation.
    - When the obtained Stationary points are less than two, the option will switch to "-m" to generate minimal energy geometry.
- **-x** : Generate a xyz file rather than restart file. Use with -m or -l.
- **-a** : Generate a input/xyz file with all geometries in the optimization.

## Caveats  
- When the calculation was done with xqc or yqc options and the computational level is HF or DFT, the plotted energy  is not for quadratically convergent SCF but for conventional SCF. Therefore, some descrepancy with the exact energy can occur. 

[^M]: M refers to Minimal.
[^dl]: The last structure whose energy was evaluated. If the energy evaluation of the last structure has not been completed, the structure just before it is obtained.
[^L]:L refers to Last.
[^S]:S refers to Stationary.
[^link]: Several inputs are joined with"--Link1--".
[^AboutStep]: When the optimization was done without the analytic force calculation (i.e. enonly), the number of plotted energy does not correspond with the number of the geometry convergence checking.

