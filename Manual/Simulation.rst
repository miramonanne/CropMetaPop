**********
Simulation
**********

This section describes general parameters of a simulation:

**generations [Integer] (Default: 1)**

Number of generations to perform during the simulation. 

**replicates [Integer] (Default:1)**

Number of replicates to create when running CropMetaPop. Replicates uses the same set of parameter values, but outputs may differ due to stochastic processes.

**folder [String] (Default:simulation)**

This parameter specifies the name of the folder where all the output files are stored. This folder is created by default in the working directory but it is possible to give another location by writting this location before the name of folder. It is possible to use the '*' before the name of folder to locate this folder in the same directory that the settings file.
If the user wants to store the outputs directly in the working directory, the folder parameter has to be listed in the settings file followed by an empty argument and the argument of parameter *folder_time* will be 0. If the output files produced by CropMetaPop already existe in the given directory, they overwrite the previous ones. 

**folder_time [0 or 1] (Default:1)**

This parameter allows to specify if the name of folder contains or not the date of the simulation.

  * **0**: the folder name does not contain the date of the simulation
  * **1**: the folder name is dynamic, i.e. comprises the date of the simulation and the starting time with the following format: "_yyyy_mm_dd_hh:mm:ss" (Year_Month_Day_Hour:Minute:Second). This dynamic folder name allows to store each simulation in separate and unique folder, avoiding that previous outputs are overwritten.

**seed [Integer]**

This parameter specifies the seed which is used to initialize the random number generator. The seed is a whole number. The seed used in one simulation is available in output file of CropMetaPop and can be used again to reproduce the same results.
