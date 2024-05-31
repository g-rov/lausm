# Left Atrial Unfolding to a Standardized 2D Map


Author: Georgios Rovas

## About


This code generates a *standardized 2D map* of the left atrium by unfolding a 3D mesh and its variables.
The code can be used for any variable, such as those derived from imaging (CT, CMR, MRI, LGE-CMR),
electrophysiological variables derived from catheterization, and hemodynamical or mechanical variables derived
from CFD or FEM analyses.

This code was updated to utilize more recent libraries and run on Windows and Linux systems.

This code was used for the following publication:

>
>
>

Please cite this reference when using this code.

## Pipeline

The pipeline is split in four parts.

* run_standardization: standardizes meshes from a raw mesh. Depends on [VMTK], [VTK] and [MeshLab]
* run_currents: registers mesh to an atlas using currents registration. Depends on [VTK], [MATLAB] and ./currents_build
* run_sum: computes standardized unfold map. Depends on [VMTK] and [VTK]
* run_quantification: computes regional quantification (extent or mean value). Depends on [VMTK] and [VTK]

## Dependencies

The scripts in this repository were successfully run with:
- [Python] 3.6.4
- [NumPy] 1.13.0
- [VMTK] 1.4.0
- [VTK] 8.1.0
- [ITK] 4.13.0
- [MATLAB] R2024a
- [MeshLab] 1.3.3
- [FFTW] 3.3.5

## Installation

### LAUSM

Clone the repository and cd into it:
```sh
$ git clone https://github.com/
$ cd lausm
```

Set paths according to your system in `constants.py` if the dependencies are already installed.
```
MATLAB_BIN_PATH
CURRENTS_BUILD_PATH
MESHLABSERVER_PATH
```

The default parameters of the algorithm can be modified in `main.py` inside the `get_parameters()` method.

### Meshlab

Download [MeshLab 1.3.3] and install it. Update the installation location in `constants.py`.

### MATLAB

Download and install [MATLAB]. Install the MinGW-w64 Compiler from the MATLAB add-ons menu.

### Python, NumPy, VMTK and VTK

The easiest way to install everything you need is by using [conda]. It includes Python, NumPy, VMTK and VTK.

First, download and install [conda], Anaconda or [miniconda] package:

Create an environment and activate it:

```
$ conda create --name vmtk python=3.6.4
$ conda activate vmtk

```

Install vmtk, vtk, itk and NumPy:
```
conda install -c vmtk vmtk=1.4.0 itk vtk
```

Anytime you wish to use vmtk (e.g. to run the code in this repository) you will need to activate the environment:
On Windows:
```
conda activate vmtk
```
or on Linux:
```
source activate vmtk
```

### Currents

From the MATLAB command prompt execute the following staring from the parent directory:
```
cd currents_build
mex -I./ convol.c -llibfftw3-3 -L./
mex -I./ gridOptim.c -llibfftw3-3 -L./
mex -I./ projConvol.cpp -llibfftw3-3 -L./
match2vtks('./test/source.vtk','./test/target.vtk','./test/output','1','0.0001')
```

If compliled with the instructions above, there is no need to change `CURRENTS_BUILD_PATH` in `constants.py`.

## Usage

```
main.py [-h] --meshfile MESHFILE --datatype DATATYPE
               [--pvcliptype PVCLIPTYPE]
               [--paired_unfold_disk PAIRED_UNFOLD_DISK]
               [--mitral_clip_type MITRAL_CLIP_TYPE] [--use_seed_selector]
               [--use_laa_seed] [--use_similarity] [--use_glyphs]
               [--visualize] [--skip_standardization] [--skip_currents]
               [--skip_sum] [--skip_quantification]


  -h, --help              show this help message and exit
  --meshfile MESHFILE     Full path to mesh file in VTK format
  --datatype DATATYPE     Data type to process: tavf | force | lge | lat
                          TAVF: (T)ime-averaged wall shear stress, (A)ge of blood,
                          bipolar (V)oltage, and (F)ibrosis
                          Force|LGE|LAT: Older datatypes for catheter contact force,
                          LGE-CMR and local activation time.  
  --pvcliptype PVCLIPTYPE How to clip the pulmonary veins: short | long
  --paired_unfold_disk PAIRED_UNFOLD_DISK
                          Full path to a paired unfold disk. The edges of the
                          current unfold will be overlaid on the paired unfold
                          disk
  --mitral_clip_type MITRAL_CLIP_TYPE
                          The algorithm will compute a mitral plane based on the
                          body and PVs centroids. If the meshfile already
                          contains a mitral plane clip, use the 'manual' option
                          to preserve it
  --use_seed_selector     Seed selection will always run for a new case. To
                          select new seeds, activate this flag
  --use_laa_seed          Activate seed for appendage
  --use_similarity        The method uses an affine transform to initialize mesh
                          registration. Activate this flag to use a similarity
                          transform
  --use_glyphs            Activate glyph on visualization. Recommended for spare
                          measurements (e.g. some lat meshes)
  --visualize             Activate visualization
  --skip_standardization  Skip run_standardization step
  --skip_currents         Skip run_currents step
  --skip_sum              Skip run_sum step
  --skip_quantification   Skip run_quantification step
```

## Test data
```
python ./main.py --meshfile ./data/test/test.vtk --datatype tavf
```

Data was created by using a mesh from the [LASC] challenge and adding mock scalars onto it: force, lge, and lat.

Run the command lines below and the PNG output should be identical to the one in `./data/expected_output`.

```
python ./main.py --meshfile ./data/force/mock_force.vtk --datatype force

python ./main.py --meshfile ./data/lge/mock_lge.vtk --datatype lge --paired_unfold_disk ./data/force/mock_force_FTI_disk_uniform.vtp

python ./main.py --meshfile ./data/lat/mock_lat.vtk --datatype lat

python ./main.py --meshfile ./data/lat/mock_lat.vtk --datatype lat --use_glyph --skip_standardization --skip_currents
```

## Dependencies

The scripts in this repository were successfully run with:
- [Python] 2.7.14
- [NumPy] 1.13.3
- [VMTK] 1.4
- [VTK] 8.0.1
- [MATLAB] R2017b
- [MeshLab] 1.3

## Version History

v2. https://github.com/



v1. https://github.com/catactg/sum

> Standardized unfold mapping: a technique to permit left atrial regional data display and analysis.
> Williams SE, Tobon-Gomez C, Zuluaga MA, Chubb H, Butakoff C, Karim R, Ahmed E, Camara O, Rhode KS.
> J Interv Card Electrophysiol. 2017 Sep 7. doi: 10.1007/s10840-017-0281-3.

Please cite these reference when using this code.

## License

BSD 2-Clause



[Python]:http://www.python.org
[NumPy]:http://www.numpy.org
[VMTK]:http://www.vmtk.org
[VTK]:http://www.vtk.org
[MATLAB]:http://www.mathworks.com
[MeshLab]:http://www.meshlab.net
[MeshLab 1.3.3]:https://sourceforge.net/projects/meshlab/files/meshlab/MeshLab%20v1.3.3/
[conda]:https://conda.io/docs/
[Miniconda]:https://docs.anaconda.com/free/miniconda/
[ITK]:https://itk.org/
[FFTW]:https://www.fftw.org/
