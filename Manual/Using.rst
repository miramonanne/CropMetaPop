*****************
Using CropMetaPop
*****************

This section describes how to use CropMetaPop.

============
Installation
============

The source code of CropMetaPop is available on the website www.cropmetapop.org. It requires: 
  * **simuPOP(version 1.1.6 or 1.1.7)**: CropMetaPop uses simuPOP to simulate the evolution of populations . (http://simupop.sourceforge.net)
  * **igraph (optional)**: CropMetaPop uses the python module igraph to create default networks. If the network is directly given, this module is not necessary.(http://igraph.org/python/)
  
It is possible to install these libraries manually (Window/Linux/Mac). It is also possible to use the bash script *env_Crop.sh* available in the source code on a Mac or Linux (64 bits). This script creates an environment where simuPOP (version 1.1.7) and igraph are installed. In this case, the user will have to enter in this environment to use CropMetaPop. 

A standalone version of CropMetaPop also exist in the website.  


=====================
Launching CropMetaPop
=====================

CropMetaPop is a console program. A simulation is defined using a settings file. The settings file is then passed as parameter in the console when CropMetaPop is launched.
::

  > python CropMetaPop.py settings.txt

====================================
Input Files : settings of simulation
====================================

The settings file is a text file with one parameter per line in a key:value scheme. For example:
::

  generation:5

sets the parameter generation to 5. The order of appearance of the parameters in the settings file does not matter. If a parameter appears several times, only the last one will be taken into account. 

-------------
Default value
-------------

Most of the parameters have default values. The default value of a parameter is taken into account when the parameter is not filled in the settings file. The default values are given in this manual and are specified by "(default: )". By providing default values help in keeping the settings file short and clear. All these default values are stored in the 'default_setting.txt' file located in the source code.

---------------
Parameter types
---------------

Parameters may be of different types: integer, double, string, vector or external file. The type of a parameter is specified in brackets '[]'.

~~~~~~~~
Integer
~~~~~~~~

Integer is a whole-number. Two forms are accepted : 1000 or 1e3.

~~~~~~
Double
~~~~~~

Double is a floating-point number. Two forms are accepted : 0.001 or 1e-3.

~~~~~~
String
~~~~~~

String is a text argument.

~~~~~~~
Vector
~~~~~~~

Vector allows to pass several numbers (integer or double) as parameter. Vector is enclosed between curly brackets '{ }', and numbers are separated by comma ','. 

::

  nb_pop:5
  init_size:{100,200,300,400,500}

Usually vector are defined in whole. For example, the size of vector for *init_size* is 5 as the number of population (*nb_pop*).  
If the vector is a repetition of a pattern, it is also possible to define only the pattern. In this case, the pattern is automatically repeated as needed.

::

  nb_pop:5
  init_size:{200,100}

In this example, there is 5 populations, however the initial size is only specified for 2 populations. As 2 is lower than 5 a warning message will be returned and the populations will automatically have the following initial size: init_size:{200, 100, 200, 100, 200}. If all the values of a vector are identical, only one number is needed. In the following example, the two specifications of the initial size are equivalent:

::

  nb_pop:5
  init_size:{100,100,100,100,100}
  init_size:100

~~~~~~~~~~~~~~
External file
~~~~~~~~~~~~~~

In general, arguments are written directly after the parameter name on a single line. However, some parameters have to be given in external text file. The format file is csv (comma-separated values). The parameter value for external file is the name of the file. If the file is in the current folder, the name is sufficient but otherwise the path to the file is needed. To simplify, it's possible to use the character '*' before the name of file if it is located in the same folder that the settings file.

::
  
  col_network:settings/matrixCol.csv
  col_network:*matrixCol.csv
  
~~~~~~~~
Comments
~~~~~~~~

It is possible to write comments in the settings file. Comments has to start with '#' character. For multi-line comments, this character has to be used at the beginning of each line.

============
Output Files
============

CropMetaPop can produce up to 6 output files:

  * 2 permanent files which are created at each simulation:
  
    * a file giving, for each replicate and each population, the number of each mono-locus genotype, at each marker per generation
    * a file giving the time of simulation, the warnings message and a summary of settings.
  
  * 4 optional files:
  
    * a file giving, for each replicate and each population, the number of each multi-locus genotype, per generation
    * a file giving, for each replicate and each population, the number of each multi-locus haplotype, per generation
    * a file giving the list of the number of individuals who moved from one population to another through migration per generation
    * a file giving the list of the number of individuals who moved from one population to another through colonization per generation

  
  * A copy of the settings file and others external file will be created with outputs.
    
=====================
Minimal settings file
=====================

This section describes the parameters needed for a minimal settings file and describes the simulated model.

In CropMetaPop, one parameter is needed in every settings file:

::
  
  carr_capacity: 1000
  
  
This minimal settings file allows to perform a simulation with :
- only one population consisting of 1000 hermaphrodites individuals. 
- the population evolves under random mating
- during one generation by keeping the population size constant and equal to the carrying capacity. 
- the default population has one marker with 2 alleles, initialized at 0.5. 

