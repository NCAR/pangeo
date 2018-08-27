## `launch-pangeo-on-cheyenne`

This script should be run on your local machine.
It will ssh to cheyenne and submit a job to the share queue to launch jupyter lab.
Once the job starts running, it will determine the name of the compute node running the job.
It will then ssh back to cheyenne to create a tunnel forwarding the jupyter port (probably 8888).
At that point, you should be able to go to [http://localhost:8888](http://localhost:8888) and enter your jupyter token.
Once the tunnel is established, the website will open automatically.

### Assumptions

1. You must have installed conda on cheyenne, and have a conda environment that contains `jupyterlab`.
See `../server-side/README.md` for instructions on setting up conda on cheyenne.
By default, `launch-pangeo-on-cheyenne` thinks this environment is named `pangeo-cheyenne`.
If you already have a conda environment you wish to use, specify it with the `-e` option.

1. Required packages:

   * `dask`
   * `xarray`
   * `jupyterlab`
   * `dask-jobqueue=0.3.0`

1. Recommended packages for plotting:

   * `matplotlib`
   * `Cartopy`

1. The current implementation requires you to open up a new tunnel to the `dask` port once you have started the dashboard.
```
        $ ssh -N -l [USERNAME] -L 8787:[COMPUTE NOTE]:8787 cheyenne.ucar.edu
```
8787 is the default port, but if it is in use by another user then a different one will be presented.
There is a package named `nbserverproxy` that can be used to forward this port through 8888 (the jupyter port),
but it doesn't work with the current `dask-jobqueue` release.
This repository will be updated once `dask-jobqueue` has been updated.

1. `jupyterlab` will launch from `/glade/work/$USER`

### Options

1. This script assumes your username on cheyenne matches your local username. If this is not the case, specify your cheyenne username with `-u USERNAME`

1. If you have specified the environment variable `$PBS_ACCOUNT` on cheyenne, that account will be charged by `qsub`.
If you do not have that environment variable defined, or you want to charge a different account, use `-A ACCOUNT`

1. If you get disconnected and need to re-establish the ssh tunnel, use the `-H REMHOST` option
(where `REMHOST` is the name of the compute node you are running).
The host name should be printed as part of the output of the script the first time you run it:
```
Job host:  r8i7n9
```
Alternatively, you can also use the `-i JOBID` option, where `JOBID` is also available from the script output
```
Job ID: 1692005
```
or from running `qstat` on cheyenne.
Note that `-i` will prompt you for your yubikey log in twice -
once to determine the host name, and again to actually create the tunnel.

1. When you are done with jupyter, you can kill the cheyenne job with `-k JOBID_TO_KILL`
(see above for tips on determining the job ID)

1. By default, 4 hours of walltime will be requested.
To run jupyter for a longer (or shorter), use `-w H:MM:SS`

### When things go wrong

To see the output from jupyter, log in to cheyenne and run `qpeek -f JOBID`.
Occasionally jupyter lab will hang on the compute node.
Sometimes it will bounce back in a few seconds,
other times the only solution is to kill the job and relaunch jupyter lab.
