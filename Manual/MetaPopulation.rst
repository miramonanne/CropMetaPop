***************
Meta-Population
***************

This section describes general parameters concerning the metapopulation.

**nb_pop [Integer] (Default:1)**

This parameter allows to set the number of populations. The numbering of populations begins to 0.

**init_size [Integer/vector]**

This parameter allows to set different population sizes during initialization. If the argument is a single value, the initial population size is the same for all populations. It is possible to set an initial size for each population by providing a vector of length equal to the number of populations.

If the initial population size (parameter *init_size*) is not specified, populations are initialized with carrying capacity values.

**carr_capacity [Integer/vector]**

This parameter is the only parameter required in the settings file. This parameter allows to set the carrying capacities of the populations. The carrying capacity of a population is the maximal number of individuals supported by the patch (field) where the population grows. If the argument is a single value, the carrying capacity is the same for all the populations. It is possible to set the carrying capacity for each population by providing a vector of length equal to the number of populations.

::

  nb_pop:2
  carr_capacity: 100
  
  nb_pop:2
  carr_capacity:{100,200}
 
In the first example, all populations have a carrying capacity equal to 100 individuals. In the second example, the population 0 has a carrying capacity equal to 100 individuals while the population 1 has a carrying capacity equal to 200 individuals. 

**nb_marker [Integer] (Default:1)**

This parameter allows to set the number of markers per individual. The numbering of markers begins to 0

**nb_allele [Integer/vector] (Default:2)**

This parameter allows to set the number of alleles per marker. The numbering of alleles begins to 0

::

  nb_marker:3
  nb_allele:2
  
In the above example, there is 3 markers with 2 alleles for each of them. 

::

  nb_marker:3
  nb_allele:{2,3,4}

In the above example, there is 3 markers. The first marker has 2 alleles, the second has 3 alleles and the third has 4 alleles.  


CropMetaPop recognizes 4 forms of input format for the initialization of individual genotypes, corresponding to 4 parameters: *init_AlleleFrequency*, *init_AlleleFrequency_equal*, *init_GenotypeFrequency* and *init_GenotypeFrequency_equal*. Only one parameter has to be used in the settings file. If none of them is used, the populations are initialized with the same frequency for every allele of the same marker. This frequency is equal to one over the number of alleles per marker.

**init_AlleleFrequency [External file]**

This parameter allows to set the initial allele frequencies of each population. This frequency may be different depending to the population. This parameter requires a text file using comma as separator (type csv). Each line of this file is organized as follows: *population, marker, frequencyAllele_0, frequencyAllele_1, ...*, with:

  * *population [Integer]*: number of populations
  * *marker* [Integer]: number of markers
  * *frequencyAllele_i [Double]*: frequency ranging from 0 to 1 for the allele i of the marker. 

Allele frequencies have to sum to 1 for every line. The initial frequencies for all markers for all populations must be given. If the frequency of one allele is not given, it is considered equal to 0.

::

  nb_pop:2
  nb_marker:3
  nb_allele:{2,3}
  init_AlleleFrequency:*freq.csv

file : freq.csv
::

   0, 0, 0.5, 0.5
   1, 0, 0.5, 0.5
   0, 1, 0.2, 0.3, 0.5
   1, 1, 0.5, 0  , 0.5
   0, 2, 0.2, 0.2
   1, 2, 1
   
 
It is possible to use the parameter *init_AlleleFrequency_equal* to set the same initial allele frequency for all populations:

**init_AlleleFrequency_equal [External file]**

This parameter allows to set the same initial allele frequencies for each population. This parameter requires a text file using comma as separator (type csv). Each line of this file is organized as follows: *marker, frequencyAllele_0, frequencyAllele_1, ...*, with: 

  * *marker [Integer]*: number of the marker
  * *frequencyAllele_i [Double]*: frequency range from 0 to 1 for the allele i of the marker. 

Allele frequencies have to sum to 1 for every line. The initial allele frequencies for all markers must be given. If the frequency of one allele is not given, it is considered equal to 0. 

::

  nb_pop:2
  nb_marker:3
  nb_allele:{2,3}
  init_AlleleFrequency_equal:*freq.csv

file : freq.csv  
::

   0, 0.5, 0.5
   1, 0.2, 0.3, 0.5
   2, 1
 
**init_GenotypeFrequency [External file]**

This parameter allows to set the initial multi-locus genotype frequencies for each population. This frequency may be different depending on the population. The composition in terms of multi-locus genotypes can also be different depending on the population. This parameter requires a text file using comma as separator (type csv). Each line of this file is built as follows: *population, genotypeMarker_1, genotypeMarker_2, ... ,frequency*, with:

  * *population [Integer]*: number of populations
  * *genotypeMarker_i [String]*: The genotype of the marker i must be write as *allele/allele*.
  * *frequency [Double]*: frequency between 0 and 1 of the multi-locus genotype in the population. 

In this file, all the markers have to be filled. All the multi-locus genotypes per population have to be provided. Multi-locus frequencies have to sum to 1 per population. 


::
  
  nb_pop:2
  nb_marker:3
  nb_allele:{2,3}
  init_GenotypeFrequency:*freq.csv

file : freq.csv  
::

   0, 1/1, 1/1, 0.5
   0, 1/0, 0/0, 0.5
   1, 0/0, 0/0, 0.3
   1, 1/0, 1/1, 0.5
   1, 0/0, 1/1, 0.2
   2, 1/1, 0/0, 1

It is possible to use the parameter *init_GenotypeFrequency_equal* if all the populations have the same initial genotypes:

**init_GenotypeFrequency_equal [External file]**

This parameter allows to set the multi-locus genotype frequencies per population during initialization. This parameter requires a text file using comma as separator (type csv).  Each line of this file is built as follows: *genotypeMarker_0, genotypeMarker_1, ... ,frequency*, with:

  * *genotypeMarker_i [String]*: The genotype of the marker i must be write as *allele/allele*.
  * *frequency [Double]*: frequency between 0 and 1 of the multi-locus genotype. 

In this file, all the markers have to be filled. The multi-locus genotypes frequencies has to sum to 1.

::

  nb_pop:2
  nb_marker:3
  nb_allele:{2,3}
  init_GenotypeFrequency_equal:*freq.csv

file : freq.csv
::

   1/1, 1/1, 0.25
   0/0, 0/0, 0.25
   0/0, 1/1, 0.5
   
**geneticMap [External file]**

This file allows to give the position of each marker on chromosome. This parameter requires a text file using comma as separator (type csv). Each line of this file is built as follows: *marker, chromosome, distance*, with:

* *marker [Integer]*: number of marker
  * *chromosome [Integer]*: number of chromosome
  * *distance [Double]*: This distance between the extremity of chromosome and the marker, in morgan.

All the markers have to be filled.

::
  
  geneticMap:*genetic.csv

::

  0, 1, 0.5
  1, 2, 1
  2, 2, 2.5

The following formula uses the distance between two adjacent markers (D) to calculate the recombination rate between pair of markers (Haldane): 

.. math::
   recombination = \frac{1}{2}(1-e^{-2*D}) 

If the parameter *geneticMap* is not given, each marker is considered as independent and the recombination rate is fixed to 0.5.


