**********
Life Cycle
**********

CropMetaPop relies on SimuPop, a discrete generation-based simulator. This means that a single individual undergoes only once during the life cycle without overlapping generations. Depending on the parameterization of the settings file some processes can be skipped. The order of the different processes of the life cycle is fixed. A simulation starts with "Breeding", i.e. only adults are present at the initialization of the simulation. A new life cycle starts again at the end of each generation up to the last generation, i.e. before the last generation a new life cycle starts with ”Breeding” and comes after "Output":

  * **Breeding**: adult plants produce new seeds for the starting generation. Selection and mutation are taken into account at this stage. After reproduction, the adult plants are removed and the new seeds constitute the new generation.
  * **Extinction**: Due to stochastic events populations may extinct.
  * **Seed circulation**: A seed lot sample can be tranferred from one or several populations to another one.
  * **Regulation**: After these three previous steps, the number of seeds may be too large compared to the carrying capacity. This regulation stage allows to control the population sizes. The remaining seeds become adult plants that will be used to create the next generation.
  * **Output**: Different options are available to customize the output files after the simulations.

========
Breeding
========

This stage performs mating and breeding to produce the new offspring generation. After that step, the adult plants are removed and the population is composed of the offsprings. CropMetapop allows the evolution of population size as the number of offsprings is the produce between the parameter *fecundity* and the number of adults (*number_adult*) (number_offspring = number_adult * fecundity).

**fecundity [Double] (Default:1)**
This parameter sets the average number of offspring produced per individual. Default value for *fecundity* is 1 corresponding to a constant population size.

::

  fecundity:2

---------
Selection
---------

In this version of CropMetaPop, local selection is modeled as a combination of soft selection and selection at the population level. Therefore, selection influences the breeding at two levels. At the population level, the effective number seeds produced in a population is adjusted with the mean fitness of the adults of this population (number_seed = number_offspring \∗ mean fitness). At the individual level, the parents of each offspring are drawn among all available parents within the population depending on their individual fitness, i.e. the higher the individual fitness, the higher the probability to be chosen.

CropMetaPop simulates local selection in different populations using two possibilities: either the same genotype may have different fitness values depending on the population (in this case, use the parameter *fitness*) or each population has its own optimum fitness (in this case, use the parameter *fitness_equal*).

There are two types of markers:
  * neutral markers do not influence the individual fitness
  * gene markers influence the individual fitness. Defining several gene markers allows to model a quantitative trait. The allelic effects at each locus must be set explicitly in the parameters *fitness* or *fitness_equal*


**fitness_equal [External file]**

This parameter allows to set a fitness value for each genotype at each selective marker. Each line of this file is built as *marker, genotype, value*. Marker in this file is automatically considered in gene region. Default individual fitness value is equal to the optimum of the population. 

  * *marker [Integer]*: marker's identifier 
  * *genotype [String]*: The genotype must be written as *Allele/Allele*. 
  * *value [Double]*: individual fitness value for the considered genotype at the considered marker. Individual fitness ranges from 0 to 1, by representing the relative reproductive success of an individual carrying this genotype for the considered marker. 

::

  nb_pop:3
  fitness_equal:*fit.csv

::

  0, 1/1, 0.5
  0, 1/0, 0.2
  0, 0/0, 0.2
  1, 1/1, 0.1
  1, 1/0, 0.8
  1, 0/0, 0.3

From this file, the absolute fitness (fit_abs) of one individual is calculated as the mean of fitness values of its genotype at each marker in gene region. In the above example, the markers 0 and 1 are gene markers. An individual with a genotype 1/1 for the marker 0 and the genotype 1/0 for the marker 1 has fit_abs = (0.5+0.8)/2 = 0.65.


**fitness [External file]**

This parameter allows to set the fitness values for each genotype at each gene marker in each population. Each line of this file is built as *population, marker, genotype, value*. Marker in this file is automatically considered as gene marker. Default fitness value is fix to the optimum of the population.

  * *population [Integer]*: number of number
  * *marker [Integer]*: marker's ID
  * *genotype [String]*: genotype must be written as *numberOfAllele1/numberOfAllele2*. 
  * *value [Double]*: selective value between 0 and 1 for the genotype of the marker in this population. It describes reproductive success of an individual with this genotype at this marker.

::

  fitness : *fit.csv
  
::

  0, 0, 1/1, 0.5
  1, 1, 1/0, 0.8
  2, 0, 0/0, 0.9
  2, 1, 0/1, 0.85


**optimum [Double/vector] (Default:1)**

This parameter specifies the optimum of fitness for each population. If the argument is a single value, the optimum is the same for all the populations. By passing a vector of optima, it is possible to assigned an optimum value of fitness for each population. 

::
  nb_pop:2
  optimum:0.8
  
  nb_pop:2
  optimum:{0.2,0.8}

In the first example, all the populations have an optimum of 0.8. In the second example, population 0 has an optimum of 0.2 whereas population 1 has an optimum of 0.8.

Finally, the fitness of one individual is given by:

.. math::
   fitness = e^{-\frac{1}{2} *(fit_{abs} - optimumPopulation)^2 }
   
The closest to the optimum of the population is the absolute fitness of an individual ,the highest is the final fitness.

-------------
Mating System
-------------

In our model, individuals are hermaphrodite and individuals have two possibilities for mating: 

  * *Selfing*: one parent from a given population is randomly assigned to self fertilize and create a new individual
  * *Random mating*: two parents from the same population are randomly assigned to create a new individual.
  
The proportion of selfing in the simulation is determined by the parameter *percentSelf*

**percentSelf [Double] (Default:0)**

This parameter allows to determine the proportion of offspring which will be created by selfing. It must be between 0 and 1. When this parameter is equal to 1,  all the offsprings are created by selfing whereas when it is equal to 0, all the offsprings are created by random mating.

::

  percentSelf:0.8
  
In this case, 80% of individuals are create by selfing and 20% by random mating.

--------
Mutation
--------

The mutation model used in CropMetaPop is a K-allele model. In this model, each allele has the same probability to mutate into another allele. The rate of mutation is defined by the parameter *mut_rate*.

**mut_rate [Double/vector] (Default:0)**

This parameter specifies the mutation rate by locus and generation. If the argument is a single value the mutation rate for all loci is the same. By passing a matrix of mutation rates it is possible to set the mutation rate for each single locus individually. By default no mutations occur. The mutation rate ranges from 0 to 1.

::

  nb_marker:2
  mut_rate:0.01
  
  nb_marker:2
  mut_rate:{0.1,0.05}
  
In the first example, the mutation rate for both markers is 0.01. However, in the second example, the marker 0 has a mutation rate equal to 0.01 and equal to 0.05 for the marker 1.
    
==========
Extinction
==========

This event allows to randomly make population extinct. When a population goes extinct, all individuals of the population are removed and the patch becomes empty.

**ext_rate [Double/vector] (default: 0)**

The extinction rate corresponds to the probability of a population to be extinct at each generation. It is specified by the parameter *ext_rate*. This event is skipped when the extinction rate is equal to zero . When this argument is a single value, all the populations have the same extinction rate. It is possible to set the extinction probability for each single population by passing a vector. The extinction rate ranges from 0 to 1.
::
  
  nb_pop:2
  ext_rate : 0.2
  
  nb_pop:2
  ext_rate : {0.2,0.3}
  
In the first example, the extinction rate for both populations is 0.2. Whereas in the second example, the extinction rate is 0.01 for population 0 and 0.05 for population 1.

================
Seed Circulation
================

Seed circulation is a two step process: the first one is composed by colonization and/or migration models, and the second one by a seed transfer model.
The colonization and migration models define the rules of seed circulation and the seed transfer model defines the quantity of seed in circulation.
Colonization occurs only to fill empty patch while migration can occur to fill empty or not empty patch. Networks of seed circulation are similarly defined for colonization and migration. Only the name of the parameters are different (*col_param* and *migr_param* respectively). 

------------------
Colonization model
------------------

Colonization accounts for cases when a farmer has lost his population due to an extinction event for instance and wants to obtain a new seed lot to replace it. It is then possible for him to obtain seeds from his social network through a direct neighbor who still grows the population.

**col_network [0,1,2,3,4,5,6 or external files] (Default 0)**
  
This parameter allows to choose the network topology of seed circulation for colonization, i.e the social network linking farmers who possibly can provide seed lots. A network topology is defined by a set of nodes (populations) and a set of edges (social links allowing seed circulation between two populations). Two ways are available to define the network topology: i) when the parameter ranges from 1 to 6, different network models can be selected; ii) when it is an external file, it is possible to directly provide a matrix of connection including colonization rates or an adjacency matrix.

  0. **No colonization**: populations are not connected  with each other (empty network).
  1. **Stepping stone 1D model**: each population is connected to 2 neighbors (chain).
  2. **Stepping stone 2D model**: each population is connected to 4 neighbors (grid).
  3. **Island model**: all the populations are connected with each other (complete network).
  4. **Erdős-Rényi model**: a random graph model where each pair of populations has the same probability to be connected (Erdős & Rényi, 1959).
  5. **Community model**:a random graph model based on the stochastic block model defined by at least two clusters and two probability of connection (within and between cluster). The within-cluster probability needs to be higher than the between-cluster probability to obtain a community network topopology (Snijders & Nowicki, 1997).
  6. **Barabási-Albert model**: it is a random graph model based on preferential attachment algorithm (Albert & Barabási, 2002). It is a discrete time step model and in each time step a single vertex is added. We start with a single vertex and no edges in the first time step. Then we add one vertex in each time step and the new vertex initiates some edges to old vertices. The probability that an old vertex is chosen is given by:

.. math::
  P[i] ~ k[i]^alpha

where k[i] is the in-degree of vertex i in the current time step (more precisely the number of adjacent edges of i which were not initiated by i itself) and alpha is a parameter given by the power arguments.


.. list-table:: Network models for colonization and their specific parameters
   :widths: 30 60
   :header-rows: 1
   :stub-columns: 1

   * - Model
     - Additional Parameters
   * - 0:No migration
     -
   * - 1: Stepping stone 1D model
     - col_rate
   * - 2: Stepping stone 2D model
     - col_rate
   * - 3: Island model
     - col_rate, col_directed
   * - 4:Erdős-Rényi model
     - col_rate, col_directed, col_nb_edge
   * - 5:Community model
     - col_rate, col_directed, col_nb_cluster, col_prob_intra, col_prob_inter
   * - 6:Barabási-Albert model
     - col_rate, col_directed, col_power

**col_rate [Double] (Default:0)**

This parameter defines the colonization rate between all pairs of populations of the metapopulation. The value of this parameter ranges from 0 to 1. The same colonization rate is applied to the network by using this parameter. It is also possible to choose different colonization rates by directly loading a matrix of connection specifying each colonization rate using an external file (see below). If this parameter is not filled, then populations are considered as independent, corresponding to a situation without colonization (equivalent to the option col_network=0). 

**col_directed [0,1] (Default:0)**

This parameter defines if the created network will directed.

  * **0**: the network won't be directed
  * **1**: the network will be directed
    
**col_nb_edge [Integer] (Default:0)**

This parameter sets the number of edges present in a network defined through a Erdős-Rényi model. If this parameter is not filled for an Erdős-Rényi model, then the number of edge is set to 0 and populations are considered as independent, corresponding to a situation without colonization (equivalent to the option col_network=0).

**col_nb_cluster [Integer] (default:1)**

This parameter allows to set the number of clusters present a network topology obtained through the Community model.

**col_prob_intra [Double] (Default:1)**

This parameter allows to set the within-cluster probability of connection in the Community model, i.e. the probability for two populations from the same cluster to be connected together. The value of this parameter ranges from 0 to 1.

**col_prob_inter [Double] (Defaut:1)**

This parameter allows to set the between-cluster probability of connection in the Community model, i.e. the probability for two populations from two different clusters to be connected together. The value of this parameter ranges from 0 to 1.
  
**col_power [Double] (Default:1)** 

This parameter allows to set the power of ...

It is also possible to give a self-defined connection matrix of desired network with an external file. Two ways are possible to define such colonization model: i) the external file is a matrix filled with O and 1, corresponding to an adjacency matrix and the *col_rate* option is used to defined the general colonization rate of the network; ii) the external file is filled with values ranging from 0 to 1, corresponding to local colonization rate.

::
  
  col_network:*matrix.csv
  col_rate:0.2
  
::
  
  0, 0, 1
  1, 0, 1
  0, 1, 0

or 

::
  
  col_network:*matrix.csv
  
::

  0  , 0  , 0.2
  0.2, 0  , 0.2
  0  , 0.2, 0
  
  
This 2 examples give the same results corresponding to the two above mentioned possibilities respectively.

---------------
Migration model
---------------

Migration accounts for cases when a farmer introduces a new source of seed into his population. It is then possible for him to obtain additional seeds from his social network through a direct neighbor who grows the population.

**migr_network [0,1,2,3,4,5,6 or external files] (Default 0)**
  
This parameter allows to choose the network topology of seed circulation for migration, i.e the social network linking farmers who possibility can provide seed lots. A network topology is defined by a set of nodes (populations) and a set of edges (social links allowing seed circulation between two populations). Two ways are available to define the network topology: i) when the parameter ranges from 1 to 6, different network models can be selected; ii) when it is an external file, it is possible to directly provide a matrix of connection including migration rates or an adjacency matrix.

  0. **No migration**: populations are not connected  with each other (empty network).
  1. **Stepping stone 1D model**: each population is connected to 2 neighbors (chain).
  2. **Stepping stone 2D model**: each population is connected to 4 neighbors (grid).
  3. **Island model**: all the populations are connected with each other (complete network).
  4. **Erdős-Rényi model**: a random connected graph model where each pair of populations has the same probability to be connected (Erdős & Rényi, 1959).
  5. **Community model**:a random connected graph model based on the stochastic block model defined by at least two clusters and two probability of connection (within and between cluster). The within-cluster probability needs to be higher than the between-cluster probability to obtain a community network topopology (Snijders & Nowicki, 1997).
  6. **Barabási-Albert model**: it is a random connected graph model based on preferential attachment algorithm (Albert & Barabási, 2002).

.. list-table:: Network models for migration and their specific parameters
   :widths: 30 60
   :header-rows: 1
   :stub-columns: 1

   * - Model
     - Additional Parameters
   * - 0:No migration
     -
   * - 1: Stepping stone 1D model
     - migr_rate
   * - 2: Stepping stone 2D model
     - migr_rate
   * - 3: Island model
     - migr_rate
   * - 4:Erdős-Rényi model
     - migr_rate, migr_directed, migr_nb_edge
   * - 5:Community model
     - migrl_rate, migr_directed, migr_nb_cluster, migr_prob_intra, migr_prob_inter
   * - 6:Barabási-Albert model
     - migr_rate, migr_directed, migr_power

     
**migr_rate [Double] (Default:0)**

This parameter defines the migration rate between all pairs of populations of the metapopulation. The value of this parameter ranges from 0 to 1. The same migration rate is applied to the network by using this parameter. It is also possible to choose different migration rates by directly loading a matrix of connection specifying each migration rate using an external file (see below). If this parameter is not filled, then populations are considered as independent, corresponding to a situation without migration (equivalent to the option migr_network=0). 

**migr_directed [0,1] (Default:0)**

This parameter defines if the created network will directed.

  * **0**: the network won't be directed
  * **1**: the network will be directed
    
**migr_nb_edge [Integer] (Default:0)**

This parameter sets the number of edges present in a network defined through a Erdős-Rényi model. If this parameter is not filled for an Erdős-Rényi model, then the number of edge is set to 0 and populations are considered as independent, corresponding to a situation without migration (equivalent to the option col_network=0).

**migr_nb_cluster [Integer] (default:1)**

This parameter allows to set the number of clusters present a network topology obtained through the Community model.

**migr_prob_intra [Double] (Default:1)**

This parameter allows to set the within-cluster probability of connection in the Community model, i.e. the probability for two populations from the same cluster to be connected together. The value of this parameter ranges from 0 to 1.

**migr_prob_inter [Double] (Defaut:1)**

This parameter allows to set the between-cluster probability of connection in the Community model, i.e. the probability for two populations from two different clusters to be connected together. The value of this parameter ranges from 0 to 1.
  
**migr_power [Double] (Default:1)** 

This parameter allows to set the power of ...


It is also possible to give a self-defined connection matrix of desired network with an external file. Two ways are possible to define such migration model: i) the external file is a matrix filled with O and 1, corresponding to an adjacency matrix and the *migr_rate* option is used to defined the general colonization rate of the network; ii) the external file is filled with values ranging from 0 to 1, corresponding to local migration rate.

::
  
  migr_network:*matrix.csv
  migr_rate:0.2
  
::
  
  0, 0, 1
  1, 0, 1
  0, 1, 0

or 

::
  
  migr_network:*matrix.csv
  
::

  0  , 0  , 0.2
  0.2, 0  , 0.2
  0  , 0.2, 0
  
  
This 2 examples give the same result corresponding to the two mentioned possibilities respectively.

-------------------
Seed transfer model
-------------------

The seed transfer model allows to set the parameters necessary to define the rules to quantify the seed in circulation during seed circulation events.

**col_transfer_model [String] (Default: "excess")** and 
**migr_transfer_model [String] (Default: "excess")**

These parameters determine the seed transfer model used to calculate how many seeds will be transferred among populations during the colonization event and the migration event respectively.

  * **excess**: this parameter value corresponds to the situation when a farmer provides seed to another farmer only if his population produces more seeds than its carrying capacity. Furthermore, the number of outgoing seeds is adjusted with the carrying capacity of the recipient population. The number of seed transferred is thus calculated as the minimum between the donor capacity to provide seed and the recipient capacity to receive seeds. If a farmer provides seeds to several farmers, the global capacity to provide seeds of the donor is equally distributed among the recipients. Likewise, if a population receives seeds from several populations, then the total amount seeds received seed is equally distributed among the seed sources.

  * **friendly**: this parameter value corresponds to the situation when a farmer provides seed to another farmer even if his population don’t produces enough seed for remplir son champ.
  
**col_from_one and migr_from_one [0,1] (Default:0)**

These parameters allow to reduce the send of seed in colonisation or migration from only one population. 

  * **0**: A population can received seeds from several population in one colonisation or migration.
  * **1**: A population can received seed from only one population in one colonisation or migration.
  
**migr_carrying [0,1] (Default:0)**

This parameter allows to control if a population can received migrant seed when hasn't at its carrying capacity. 

  * **0**: The recipient population has to be at its carrying capacity to receive seeds by migration.
  * **1**: The recipient population does not need to be at its carrying capacity to receive seeds by migration. In this case, the migrant seeds complete the local seed without exceeding the carrying capacity.

  
**migr_replace [Double/vector] (Default:0)**

This parameter allows to set the replacement rate for each population. This corresponds to the percentage of local seeds that will be replaced by the migrant seeds. If *migr_replace* is a single value, then it is the same replacement rate for all the populations. If it is a vector, then a replacement rate is defined for each population.

**col_rateMinimum [Double/vector] (Default:0)** and **migr_rateMinimum [Double/vector] (Default:0.5)**

These parameters allows to set the minimum proportion that the population 

==========
Regulation
==========

This event allows to limit the temporary population size obtained after the previous steps to its *carrying capacity*. After this step, the seeds become adult individuals (plants), starting a new life cycle by reproducing during the breeding process. 

=======
Outputs
=======

At the end of each life cycle, i.e. one generation, information corresponding to the mono-locus genotypes of individuals are stored to be written in the result file after the simulation. This information is stored at every step determined by the parameter *step*.

**step [Integer] (Default:1)**

This parameter corresponds to the frequency to store individual information into output files.

::

  step:5
  
In this example, data is stored every 5 generations.

**outputs [String/vector])**

This parameter defines optional results files which will be created after the simulation.

  * **Genotype**: creates a file with the number of each unique multi-locus genotype per population, for all the requested generations and all the replicates. The time to write this file is proportional to the number of markers used in the simulation.
  * **Haplotype**: creates a file with the number of each haplotype per population for all the requested generations and all the replicates.
  * **Seed_transfer**: creates two files summarizing the different seed transfer events and seed quantity at each generation for all the populations and replicate. One file corresponds to the colonization events and the second to the migration events.

.. csv-table:: genotype mono-locus result
   :header: "Replicate", "Population", "Marker", "Genotype", "Gen 0","Gen 1","Gen 2", "..."
   :widths: 40, 40 ,40, 40, 40, 40, 40, 40

   "0", "0", "0", "0/0", "25","28","31", "..."
   "0", "0", "0", "0/1", "49","20","19", "..."
   "0", "1", "1", "1/1", "26","28","31", "..."
  
.. csv-table:: genotype multi-locus result
   :header: "Replicate", "Population", "Marker 0", "Marker 1", "Gen 0","Gen 1","Gen 2", "..."
   :widths: 40, 40 ,40, 40, 40, 40, 40, 40

   "0", "0", "0/0", "0/0", "25","28","31", "..."
   "0", "0", "0/0", "1/0", "22","20","19", "..."
   "0", "1", "0/0", "0/0", "26","28","31", "..."
   
.. csv-table:: haplotype result
   :header: "Replicate", "Population", "Marker 0", "Marker 1", "Gen 0","Gen 1","Gen 2", "..."
   :widths: 40, 40 ,40, 40, 40, 40, 40, 40

   "0", "0", "0", "0", "25","28","31", "..."
   "0", "0", "0", "1", "22","20","19", "..."
   "0", "1", "0", "0", "26","28","31", "..."

   
.. csv-table:: colonization result (or migration result)
   :header: "Replicate", "Source", "Target", "Gen 0","Gen 1","Gen 2", "..."
   :widths: 40, 40 ,40, 40, 40, 40, 40

   "0", "0", "1", "25","0","31", "..."
   "0", "0", "2", "22","0","0", "..."
   "0", "1", "0", "0","28","31", "..." 

::

  outputs:{Haplotype,Seed_transfer}

  
**separate_replicate [0,1] (Default:0)**

This parameter allows to separate the results by replicates.

  * **0**: All results are in a same file
  * **1**: A different file is created for each replicate.
  
