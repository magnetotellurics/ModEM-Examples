Controlled Source Electromagnetic (CSEM) Examples
=================================================

> **IMPORTANT NOTES**: Important notes on the CSEM Version of ModEM
>
> The EM1D source code associated with CSEM has been removed from the repository
> as it contained propitary source code that cannot be released under an
> open-source license. We hope to have an updated EM1D version avialble for
> open-source released by the end of summer 2025.
>
> The EM1D code works well below with the examples given, however, work has not
> been done to validate the results of ModEM+EM1D and results should be
> considered experimental.
>
> The Dipole1D version of the code is able to run, but the results have not
> been well validated. The examples below work well with EM1D, but not with
> Dipole1D. Other configurations might perform better for Dipole1D, but
> results should be considered experimental unless more work can be done
> to validate results in a meaniningful way.
>
> We are still providing the CSEM version as is as it could be a good starting
> place for future ModEM CSEM work. 

ModEM can run on CSEM data by using the [CSEM branch of the
ModEM-Model][csem-branch] repository (which we hope to merge into the main
branch soon). This README will walk you through using ModEM to run these
simple, CSEM examples.

[csem-branch]: https://github.com/magnetotellurics/ModEM/tree/CSEM

# Setting up

## Checking out ModEM-Model

First, clone the [ModEM repository][modem-model] from GitHub:

```bash
$ git clone https://github.com/magnetotellurics/ModEM.git
Cloning into 'ModEM-Model'...
remote: Enumerating objects: 18608, done.
remote: Counting objects: 100% (199/199), done.
remote: Compressing objects: 100% (54/54), done.
remote: Total 18608 (delta 162), reused 152 (delta 145), pack-reused 18409
(from 1)
Receiving objects: 100% (18608/18608), 73.43 MiB | 13.34 MiB/s, done.
Resolving deltas: 100% (14565/14565), done.
```

Then, from inside your clone, checkout the CSEM version:

```bash
$ cd ModEM-Model
$ git checkout CSEM
branch 'CSEM' set up to track 'origin/CSEM'.
Switched to a new branch 'CSEM'
```
[modem-model]: https://github.com/magnetotellurics/ModEM/

## Configuring ModEM CSEM version

Unlike ModEM, the CSEM branch contains a single CSEM configuration file. We can
use it to create a CSEM capable version of ModEM. There are two CSEM forward
solvers available with the ModEM CSEM branch [Dipole1D][dipole1d] and EM1D.

EM1D is included within the CSEM branch, but Dipole1D is not due to Dipole1D's
license. Thus, Dipole1D will need to be downloaded before it is run,
thankfully the Configuration script can automatically download and extract
Dipole1D into the correct location. See [Dipole1D
Configuration](#dipole1d-configuration)

[dipole1d]: https://marineemlab.ucsd.edu/Projects/Occam/1DCSEM/

### Configuration Script options

Running the Configuration script without arguments will produce the usage
message:

```bash
$ ./CONFIG/Configure
Usage: ./CONFIG/Configure with the following options:
Compiler: Choose from supported compilers: [ gfortran | ifort ]
Makefile: Provide a name for your output Makefile name.
[Debug or Release]: Choose whether you want to compile the Debug or Release version.
[MPI or Serial]:  Choose whether you want to compile the parallel (MPI) or
 serial version.
[MF or SP or SP2]:  Choose between the Matrix Free (MF), or the Modified System
    of Eqs 1 (SP), or the Modified System of Eqs 2 (SP2) of the code.
[MT or MT+CSEM]:  Compile MT or MT+CSEM. In Case of MT+CSEM, choose in the
    following option whether Dipole1D or EM1D or both will be used to get for the
    secondary field formulation.
[Dipole1D or EM1D or Dipole1D+EM1D]:  (Optional) - Choose whether you have
    Dipole1D, or EM1D or both codes in the source files folder '/3D_MT/CSEM_module'
Optional: Enviornment variables: 'FC' 'FFLAGS' 'CPPFLAGS' 'LDFLAGS' 'LDLIBS'
are respected
```

### Dipole1D Configuration

> **WARNING/NOTE:** It appears at this time that the examples below do not run
> well with Dipole1D as they were made to run with EM1D. It is possible that the
> Dipole1D does not produce good results at this time. However, we are still
> providing the Dipole1D instructions/interface for future development and work.

To configure a ModEM executable that uses Dipole1D, you can use the following
configuration:

```bash
$ cd ModEM-Model/src/
$ ./CONFIG/Configure gfortran Makefile Release Serial SP2 MT+CSEM Dipole1D
Dipole1D is not currently in ./3D_MT/CSEM_module/Dipole1D/
Would you like to have this script automatically download it now? [Yes/No]:
```

If you don't have Dipole1D downloaded and extracted in the
`src/3D_MT/CSEM_module/Dipole1D` directory you will get the above message.
Passing 'Yes/yes/Y/y' will automatically download and extract Dipole1D into the
correct location.

If there are problems with the download script, or you would like to download
Dipole1D by yourself, you can download Dipole1D and extract the source code
into: `src/3D_MT/CSEM_module/Dipole1D`. (i.e. ensure that `Dipole1D.f90` and
other Dipole1D.f90 files are in `src/3D_MT/CSEM_module/Dipole1D`).

After downloading CSEM (Via the script), you'll get the following message:

```bash
# Using compile cmd gfortran from cmd line
# Using compiler optimization options -cpp -DCSEM_DIPOLE1D -O3
# -ffree-line-length-none -dI -fallow-argument-mismatch from cmd line
# Using compiler MPI flags -cpp -DCSEM_DIPOLE1D from cmd line
# Using ./objs/3D_MT/csemBuild for object file output directory
# Using Link options -lblas -llapack from cmd line
# Using Library path  from cmd line
# Using search path from cmd line:
 .:MPI:INV:SENS:UTILS:FIELDS:FIELDS/FiniteDiff3D:3D_MT:3D_MT/DICT:3D_MT/ioMod:3D_MT/modelParam:3D_MT/modelParam/modelCov:3D_MT/modelParam/modelParamIO:3D_MT/CSEM_module:3D_MT/FWD_SP2:3D_MT/SP_Topology:3D_MT/FWD:3D_MT/FWD/Mod2d:3D_MT/CSEM_module:3D_MT/CSEM_module/Dipole1D
 Couldn't find source file for module EM1D
```

The last warning message can be ignored because we did not request EM1D.

### EM1D Configuration

> **NOTE:** The EM1D code that CSEM version of ModEM uses in the CSEM version of
> the ModEM CSEM branch is currently not able to be included in an open source
> release.
>
> Work is being done to replace properitary code with open source code so the EM1D
> version can be released. Tenetaivley we hope to have this version avaliable by
> the end of summer 2025. 

In order to run the EM1D version of ModEM, you will need to include the location
of the [FFTW library][fftw] installation. To do so you can pass the installation
in the `FFTW` environment variable:

```bash
export FFTW=/path/to/FFTW/
```

(The FFTW should point to the folder that contains both the `lib/` and the
`include/` directories.)

For more information on installing the FFTW library see: [FFTW
Installation](#fftw-install).

```
$ export FFTW=/path/to/FFTW_INSTALL/
$ ./CONFIG/Configure gfortran Makefile Release Serial SP2 MT+CSEM EM1D
```

[fftw]: https://www.fftw.org/

# Examples

## Example 0

Example 0 is a synthetic CSEM example with a grid of dimensions 30 x 30 x 40.
The top layers are ocean with flat bottom topography, overlying conductive
marine sediments. Aside from a thin resistive body (3D) buried in the sediments,
the background is 1D.

There are two data sets: one (dTest.dat) with 2 Tx locations (each with 1
frequency) and 9 receiver locations. The second (d_05.dat) haa 9 Tx locations
(each at 2 frequencies) and 45 Rx loctions. 

Data values were generated by running (a version of!) the forward code, and
errors were added -- nominally 5% electric field amplitude, but there is a
minimum (as distance between Tx and Rx grows, E amplitudes become  very small!).

### Forward

To run the forward model on Example 0 run:

```
$ ./Mod3DMT -F mTrue.rho dTest.dat forward.dat
or
$ ./Mod3DMT -F mTrue.rho d_05.dat forward.dat
```

### Inversion

To run the inversion, you will need use the prior model and the covariance
file provided:

```bash
$ ./Mod3DMT -I m0.rho dTest.dat 1e4 1e-6 mTrue.cov
```

> **Note:** The inversion may take a siginificant amount of time to run
> to get a RMS near 1.0. In my test it took about 130 iterations.

We have included an example log and ending rho file using the above parameters.

## Example 1 and 2

This is a slightly bigger CSEM example, The model parameter fila "mTrue.rho" is
semi-realistic with grid of dimensions 62 x 66 x 50). The top layers are ocean
and there is variable bottom topography, overlying conductive marine sediments.
A thin resistive rectangular body is buried in the sediments, and the basement
is overlain by a very resistive salt body.

The data set here is larger "but it is still a toy" -- 9 Tx locations (each with
3 frequencies) and 100 receiver locations (in a 10 x 10 grid).

Data values were generated by running (a version of!) the forward code, and
errors were added -- nominally 5% electric field amplitude, but there is a
minimum (as distance between Tx and Rx grows, E amplitudes become  very small!)

You will be able to run the forward and inversion in similar ways to what
is described above.

> **Note:** The inversion may take a siginificant amount of time to run
> to get a RMS near 1.0. In my test it took about ~270 iterations.

# Creating your own Covariance files

In order to run the inverse on CSEM data and models, you will need to create a
covariance file where the water is masked.

To do this, you can use the ModEM-Tools PyModEM tool called `modem_cov` to mask
the water (See [PyModEM README.md][pymodem] for PyModEM installation
instructing).

[pymodem]: https://github.com/magnetotellurics/ModEM-Tools/tree/main/python/PyModEM

Let's create a covariance file where we set the x, y and z smoothing to be `0.3`
and where we mask water (values that match ln(0.3) +/- 0.00005.):

```bash
$ modem_cov mTrue.rho --mask_water -s 0.3 0.3 -z 0.3 -n 1
```

You can also specify different values of water conductivity to mask.

```bash
$ modem_cov mTrue.rho --mask_water --water_cond 0.5 -s 0.3 0.3 -z 0.3 -n 1
```

If you have troubles or difficulties masking the value of water, try changing 
the value of epsilon via the `-e/--esp` argument to better match values that
are *near* your requested value and what is in the rho file:

```bash
$ modem_cov mTrue.rho --mask_water --water_cond 0.5 -e 0.005
```
# FFTW Install

When compiling the FFTW library, insure you run configure with both
`--enable-float` and `--enable-mpi`:

```
$ ./configure --enable-float --enable-mpi --prefix=/my/install/location
```

In the above example, you should set the FFTW variable to:
`/my/install/location/`.

For more information on installing FFTW please see the `INTSALL` file of the
FFTW library source code.
