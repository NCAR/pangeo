Getting Started with Pangeo
===============

This notebooks in this repository are designed to run on the Cheyenne High-Performance Computer.

## Prerequisites

There are some basic requirements before the rest of this setup can work for you.  These include:

1. Have `git` installed (use `module load git` to get a recent-ish version)

1. Have a Github account (accounts are free)

1. Have a modern web browser (Chrome, Safari, etc.)

1. Basic understanding of your machine's command-line interface (e.g., Mac OS's Terminal application)

## Setup Python on Cheyenne

#### Download Miniconda

    # for linux
    $ wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh

    # for osx
    $ curl https://repo.continuum.io/miniconda/Miniconda3-latest-MacOSX-x86_64.sh -o miniconda.sh

    # for windows
    # go to: https://conda.io/miniconda.html

#### Install Miniconda

    $ bash miniconda.sh
    # follow instructions

At this point, you may need to make sure that `conda` is available in your path.
(Installing Miniconda should give you the option of putting a line at the end of your profile/login script to add `conda` to your path when you log in, again.
You may need to `source` your profile/login script or re-login or open a new terminal window to activate this change.)

#### Install Pangeo Environment

The main python packages needed to create notebooks that take advantage of cheyenne's computational power are `jupyterlab`, `dask` (and `dask-jobqueue`), and `xarray`.
The environment included in this repository also adds `matplotlib` and `cartopy` to provide plotting capability.

*This step can take some time.  If it fails, just run it again and it will pick up where it left off.*

    $ conda env create --file pangeo-cheyenne.yaml

#### Set Default Behavior for `dask-jobqueue`

Copy `jobqueue.yaml` to `~/.config/dask/` (you may need to make that directory first).
This file contains some machine-specific defaults for using the `dask-jobqueue` package on cheyenne.

#### Activate the Pangeo Environment

This step assumes that you're shell is `bash` or something like it.
If you are using `csh` or `tcsh`, you may need to run the following commands in a `bash` login (e.g., type `bash`, hit enter, and continue).

    $ source activate pangeo-cheyenne

#### Run a Quick Test

    $ python

    >>> import xarray as xr
    >>> xr.show_versions()
    >>> xr.tutorial.load_dataset('air_temperature')
    >>> xr.tutorial.load_dataset('rasm')

## Launching Jupyter and Dask on Cheyenne

There is a script that can be run from you local machine in `../client-side/`.
