PIC1D-PETSc
===========
version 2012-08-16 00:38:26-04:00
---------------------------------

PIC1D-PETSc is a code simulating 1D electrostatic plasma by solving
Vlasov-Poisson equation using particle-in-cell (PIC) method.  PIC1D-PETSc is a
reimplementation of Zhihong Lin's
[PIC1D](http://phoenix.ps.uci.edu/zlin/pic1d/).  This reimplementation
demonstrates the possibility of formulating PIC method in a vector-matrix form
and tests the practicability of implementing the vector-matrix PIC formulation
by PETSc.


Copying
-------

System requirements
-------------------

PIC1D-PETSc requires MPI-2 and [PETSc](http://www.mcs.anl.gov/petsc/) with real
scalars.  PIC1D-PETSc has been tested with
[MPICH2](http://www.mcs.anl.gov/research/projects/mpich2/) 1.4,
[OpenMPI](http://www.open-mpi.org/) 1.5 and PETSc 3.2.

PIC1D-PETSc also requires a Fortran 90 compiler for compilation.  PIC1D-PETSc
has been tested with [GNU Fortran](http://gcc.gnu.org/fortran/) 4.6.

The visualization app requires [Python](http://www.python.org/) 2.7+,
[NumPy](http://numpy.scipy.org/) 1.4+ and
[matplotlib](http://matplotlib.sourceforge.net/) 1.1+ to run.


Usage
-----

### Know about the directory tree

+ `README.md` -- This instruction.
+ `Makefile` -- For using GNU make to compile and run PIC1D-PETSc.
+ `build/` -- Place to compile PIC1D-PETSc.  Object files generated during
compilation will be put here.
	+ `build/Makefile` -- For compiling PIC1D-PETSc.
+ `run/` -- Place to run PIC1D-PETSc.  Output data will be put here.
	+ `run/Makefile` -- For launching PIC1D-PETSc.
+ `src/` -- Place to store source files.
	+ `src/pic1dp.F90` -- The main program file.
	+ `src/pic1dp_global.F90` -- Module for managing global constants,
	variables and subroutines.
	+ `src/pic1dp_input.F90` -- Module for managing input parameters and
	functions.  Change input parameters in this file.
	+ `src/pic1dp_particle.F90` -- Module for managing marker particles.
	+ `src/pic1dp_field.F90` -- Module for managing field quantities.
	+ `src/pic1dp_interaction.F90` -- Module for managing field-particle
	interactions.
	+ `src/pic1dp_output.F90` -- Module for managing output.
	+ `src/wtimer.F90` -- A wall clock timer using MPI_Wtime().
	+ `src/gaussian.F90` -- A Gaussian random number generator using polar form
	of Box-Muller transform.
+ `visual/` -- Visualization app for PIC1D-PETSc written in Python.
	+ `visual/visual.py` -- The main program file for the visualization app.
	+ `visual/XPetscBinaryIO.py` -- An extended PetscBinaryIO class.  The
	original PetscBinaryIO class is provided in
	`${PETSC_DIR}/bin/pythonscripts/`, where `${PETSC_DIR}` is your PETSc install
	directory.


### Input preparation

PIC1D-PETSc reads input parameters from `src/pic1dp_input.F90`.  Each parameter
has detailed description in the comment lines above it.

The default `src/pic1dp_input.F90` gives a Bohm-Gross wave with wave vector k =
0.4.  Theoretical result gives real frequency = 1.28505698 and Landau damping
rate = -0.06612800 (negative sign indicates damping).  Numbers are normalized
by electron Debye length and plasma oscillation frequency, respectively.


### Compilation

It is recommended to use GNU make (or a compatible alternative) to compile the
code with the included Makefile.  Before compiling, open the Makefile and go to
the place after the license part, you will see a few parameters, such as
compiler name, compiling options, MPI executor, etc., defined there.  Adjust
them according to your system environment.  After saving your adjustments, and
making sure MPI Fortran 90 compiler and PETSc are installed correctly, you can
compile PIC1D-PETSc by executing in the terminal:

	make

The compiled binary file and other compilation are put in `/build`.  To remove
the compiled binary and other compilation files, execute:

	make cleanbuild


### Running

After successfully compiling PIC1D-PETSc, and making sure your MPI environment
is correctly configured, you can then run PIC1D-PETSc by executing:

	make run


### Output data and visualization

The output files are put in `/run`.  The running progress is logged to
`/run/pic1dp.log`.  The output data is written to `/run/pic1dp.out`.  The
`/run/pic1dp.out.info` file is a byproduct of the PETSc I/O and has no use for
PIC1D-PETSc.

**When you execute `make run`, the output files of previous run will be erased
before PIC1D-PETSc runs.  If you think the output files are useful, move them
immediately to a safe place.**

To run the visualization app, first make sure Python, NumPy, and matplotlib are
installed correctly, then execute:

	make visual

Or alternatively you can execute `/visual/visual.py` directly.
`/visual/visual.py` has one optional argument, which is the path of the output
data.  If the argument is absent, it tries to read data from current directory.

The visualization app shows 7 panels.  The left 4 show time history of various
quantities.  The right 3 show quantities on a specific time.

Clicking in the upper left panel changes the time for the right 3 panels.
Clicking and dragging in the lower left panel chooses a time range for the 2
panels in the 2nd column.  The growth/damping rate calculation is based on the
plot end points of the 2nd column upper panel.


Contact
-------

Get latest updates on the project website:
<http://wdeng.info/codes/pic1dp>.

Report bugs, request new features at:
<https://github.com/wenjundeng/pic1dp/issues>.

Contribute patches, new features by forking the project on GitHub:
<https://github.com/wenjundeng/pic1dp>, applying your contributions to the
forked project, and then submitting a pull request at:
<https://github.com/wenjundeng/pic1dp/pulls>.

