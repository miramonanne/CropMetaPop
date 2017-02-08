************
Introduction
************

=====
Scope
=====

CropMetaPop is a forward-time, individual-based, genetically explicit stochastic simulation program. This program aims to take into account farmer's practices that shape the evolution of crop genetic diversity by following a metapopulation framework. It accounts for demagraphic processes like population growth, extinction, colonization, and evolutionary forces like migration, mutation, genetic drift and selection. 
It allows the simulation of diploid individuals with different and mix mating systems from selfing to open-pollinated system.

============
Availability
============

The source code of CropMetaPop, the user's manual and the developer's manual are available at www.cropmetapop.org

=========
Technical
=========

CropMetaPop is based on simuPOP (http://simupop.sourceforge.net), a python library for the simulation of populations.
CropMetaPop is thus a console program writen in python2.7 using an object oriented approach. 
CropMetaPop can be used on any computer platform. 
Limits of CropMetaPop are the same than simuPOP. (http://simupop.sourceforge.net/Main/FAQ#toc4)

=======
License
=======

CropMetaPop is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foun- dation, either version 3 of the License, or (at your option) any later version. 

CropMetaPop is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for moredetails. You should have received a copy of the GNU General Public License along with CropMetaPop.

If not, see http://www.gnu.org/licenses/.

===============
Acknowledgments
===============

The programme has been developped within the framework of the EU H2020 project Diversifood under the supervision of Isabelle Goldringer, Mathieu Thomas and Frederic Hospital. We are grateful to the research team DEAP and the group MIRES-MADRES and in particular Jérôme Enjalbert and Francois Massol for their valuable assistance. These persons helped us to build and improve CropMetaPop through discussions.

=============
Main features
=============

**Meta-population**: A meta-population in CropMetaPop is a set of cultivated populations connected to each other through migration or colonization. A population is grown within a patch that corresponds to a farmer's field. A population is composed of individuals from the same species. Pollen flow between populations (fields) is not considered in this version of the model.

**Seed circulation**: In this program, migration and colonisation of individuals correspond to seed lot circulation among connected patches (fields), i.e. seed lot circulation among fields owned by farmers who belong to the same social network. The rate of migration/colonization corresponds to the probability of seed transfer between two fields that are socially connected. 
The number of moving seeds is calculated as a function of the parameters of the seed transfert model that checks for seed transfer feasability.

**Selection**: The selection in CropMetaPop is applied on a quantitative trait. This quantitative trait is defined by a set of gene markers with an additive determinism among markers. The allelic effects at each marker has to be set explicitly and can be different according to populations.

================
Input and output
================

-----
Input
-----

CropMetaPop is launched using a settings file. The settings file is a text file with flexible and easy to understand structure. The information is specified in a parameter:argument scheme, where the order of the parameters does not matter. The file can be edited with any text editor, and annoted for helping the reader using '#'. 

------
Output
------

CropMetaPop produces the raw results of the simulation. These results summarize genotypes information for every replicate, generation and population like number of individuals for every mono-locus genotype or multi-locus genotype or haplotype. Output also proposes history of every seed lot transfer event (colonization and migration).
