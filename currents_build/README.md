# ExoShape

ExoShape is a derived software by Stanley Durrleman, Copyright (C) INRIA (Asclepios team), All Rights Reserved, 2006-2009, version 1.0.

## Installation

To install ExoShape (a.k.a. "currents") please see the instuctions on the main [README](https://github.com/g-rov/lausm/blob/main/README.md) file of the repository. If these instructions don't work, please follow the alternative installation method listed below. 

The core of the registration algorithm is coded in MATLAB(R) and can be run on MATLAB(R) version 7.1 or higher. Optimization routines are coded as mex-files and therefore must be compiled. Optimization based on FFTs must be linked with fftw3 library distributed under the GNU GPL license at www.fftw.org. The fftw3 library is included in the `currents_build` folder. If the pre-included library is not working or if the use an alternative fftw3 library is desired, please download it from the [FFTW](https://www.fftw.org/) website. 

## Alternative Installation Method

In Matlab, compile projConvol.cpp, convol.c and gridOptim.c as follows
```
mex file.c -lfftw3 -Linstall_dir/lib -Iinstall_dir/include

i.e.

mex convol.c libfftw3.a -I"./"
mex convol.c ~/code/fftw/bin/lib/libfftw3.a -I~/code/fftw/bin/include
```

### Test

Run this command line to test the compilation:
```
match2vtks('./test/source.vtk','./test/target.vtk','./test/output','1','0.0001')
```
It should take about 15 mins to run. Then, check in Paraview that the 'output.vtk' and 'expected_output.vtk' look the same

### Building notes

* Set up your mex compiler in MATLAB (look online for any compatibility issues or patches for your OS - MATLAB version)

* Be sure to specify where the library and include files are:

```
/Users/username/code/fftw/bin/lib/libfftw3.a
/Users/username/code/fftw/bin/include/fftw3.h
```
&emsp;&emsp;and compile the mex files using either of these command lines
```
mex file.c /Users/username/code/fftw/bin/lib/libfftw3.a -I/Users/username/code/fftw/bin/include
mex file.c -lfftw3 -L/Users/username/code/fftw/bin/lib/ -I/Users/username/code/fftw/bin/include/
```

## LICENSE

Based on MATCHINE v1.0 software.

Copyright Université Paris Descartes

Contributor: Joan Alexis GLAUNES (2006) alexis.glaunes .at. mi.parisdescartes.fr


This software is a computer program whose purpose is to calculate an optimal
diffeomorphic transformation in 3D-space that allows to match two datasets
like points, curves or surfaces.

This software is governed by the CeCILL-B license under French law and
abiding by the rules of distribution of free software. You can use,
modify and/ or redistribute the software under the terms of the CeCILL-B
license as circulated by CEA, CNRS and INRIA at the following URL
http://www.cecill.info.

As a counterpart to the access to the source code and rights to copy,
modify and redistribute granted by the license, users are provided only
with a limited warranty  and the software's author, the holder of the
economic rights, and the successive licensors have only limited
liability.

In this respect, the user's attention is drawn to the risks associated
with loading, using, modifying and/or developing or reproducing the
software by the user in light of its specific status of free software,
that may mean that it is complicated to manipulate, and that also
therefore means that it is reserved for developers and experienced
professionals having in-depth computer knowledge. Users are therefore
encouraged to load and test the software's suitability as regards their
requirements in conditions enabling the security of their systems and/or
data to be ensured and, more generally, to use and operate it in the
same conditions as regards security.

The fact that you are presently reading this means that you have had
knowledge of the CeCILL-B license and that you accept its terms.

&nbsp; 

Any use of this code should make reference to:
> S. Durrleman, X. Pennec, A. Trouve and N. Ayache, Statistical Models of Sets of Curves and Surfaces based on Currents, Medical Image Analysis, (2009), DOI: 10.1016/j.media.2009.07.007

and to at least one of the following references:

For the use of 'landmark or measure matching':
> J. Glaunes, L. Younes and A. Trouve, Diffeomorphic matching of distributions: A new approach for unlabelled point-sets and sub-manifolds matching, Proc. of the 2004 IEEE Computer Society Conference on Computer Vision and Pattern Recognition. (CVPR'04), Vol. 2, pp. 712--718, DOI: 10.1109/CVPR.2004.81

For the use of 'curve matching':
> J. Glaunes, A. Qiu, M. Miller, L. Younes, Large Deformation Diffeomorphic Metric Curve Mapping, International Journal of Computer vision, Springer, 2008, Vol. 80, No. 3, pp. 317--336, DOI: 10.1007/s11263-008-0141-9

For the use of 'surface matching':
> M. Vaillant and J. Glaunes, Surface Matching via Currents, Proc. of Information Processing in Medical Imaging (IPMI'05), Lecture Notes in Computer Science vol. 3565, Springer 2005, pp. 381--392
