## Codes for constructing phylogenetic relationships between beta and alpha tubulins and events that lead to sequence evolutions

#### Multiple sequence alignment
```
mafft --auto --phylipout alpha_beta_aa.fasta > alpha_beta_aa.phy
```
### Likelihood analysis using RAxML
RAxML versions installed on HPC but with long and ugly names (raxmlHPC-MPI-AVX, raxmlHPC-PTHREADS-AVX, raxmlHPC-PTHREADS-SSE3). To make our life easier, I will create an alias for one of them:
```
$ alias raxmlHPC='raxmlHPC-PTHREADS-SSE3 -T2'
```
#### Maximum likelihood analysis of alpha and beta sequences using PROTGAMMALG model of amino acid substitution (no outgroup)
```
raxmlHPC-PTHREADS-SSE3 -T2 -m PROTGAMMALG -p 12345 -s alpha_beta_aa.phy -n alpha_beta.lg
```
#### To view the tree, I used newick tree viewer
```
module load newick_utils

nw_display RAxML_bestTree.alpha_beta.lg
```
### Bootsrap analysis using 1000 replicates on hpc class
Used a Slurm job script generator via hpc_class using the following script
```
#!/bin/bash

#copy/paste this job script into a text file and submit with the command:
#    sbatch thefilename

#SBATCH --time=4:00:00   # walltime limit (HH:MM:SS)
#SBATCH --nodes=1   # number of nodes
#SBATCH --ntasks-per-node=16   # 32 processor core(s) per node 
#SBATCH --mem=62G   # maximum memory per node
#SBATCH --job-name="my_raxml1"
#SBATCH --mail-user=ensamba@iastate.edu   # email address
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL
#SBATCH --output="raxml.out1" # job standard output file (%j replaced by job id)

# LOAD MODULES, INSERT CODE, AND RUN YOUR PROGRAMS HERE
module load raxml
raxmlHPC-PTHREADS-SSE3 -f a -m PROTGAMMALG -p 12345 -x 12345 -# 1000 -s alpha_beta_aa.phy -n alpha_beta.bootstrap
```
### Distance analysis
#### Multiple sequence alignment 
```
mafft --auto alpha_beta_aa.fasta > alpha_beta_aa.aln

mafft --phylipout alpha_beta_aa.aln > alpha_beta_aa.phy

```

#### Distance models
I CalculateD 4 different matrices using 4 models available in protdist and perform a NJ analysis with each of them. I went ahead and calculated a strict consensus tree rooted using an appropriate outgroup.
```
Protein distance algorithm, version 3.696

Settings for this run:
  P  Use JTT, PMB, PAM, Kimura, categories model?  Henikoff/Tillier PMB matrix
  G  Gamma distribution of rates among positions?  Yes
  C           One category of substitution rates?  Yes
  W                    Use weights for positions?  No
  M                   Analyze multiple data sets?  No
  I                  Input sequences interleaved?  Yes
  0                 Terminal type (IBM PC, ANSI)?  ANSI
  1            Print out the data at start of run  No
  2          Print indications of progress of run  Yes

Are these settings correct? (type Y or the letter for one to change)
Y
```
#### Neighbor
```
 N       Neighbor-joining or UPGMA tree?  Neighbor-joining
  O                        Outgroup root?  Yes, at species number  1
  L         Lower-triangular data matrix?  No
  R         Upper-triangular data matrix?  No
  S                        Subreplicates?  No
  J     Randomize input order of species?  No. Use input order
  M           Analyze multiple data sets?  No
  0   Terminal type (IBM PC, ANSI, none)?  ANSI
  1    Print out the data at start of run  No
  2  Print indications of progress of run  Yes
  3                        Print out tree  Yes
  4       Write out trees onto tree file?  Yes
```
#### Strict consensus tree rooted with an appropriate outgroup
Returned an error and couldn't correct it
