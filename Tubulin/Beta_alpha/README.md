## Codes for constructing phylogenetic relationships between beta and alpha tubulins and events that lead to sequence evolutions

#### Multiple sequence alignment
```
mafft --auto --phylipout alpha_beta_aa.fasta > alpha_beta_aa.phy
```
#### Likelihood analysis using RAxML
RAxML versions installed on HPC but with long and ugly names (raxmlHPC-MPI-AVX, raxmlHPC-PTHREADS-AVX, raxmlHPC-PTHREADS-SSE3). To make our life easier, I will create an alias for one of them:
```
$ alias raxmlHPC='raxmlHPC-PTHREADS-SSE3 -T2'
```
### Maximum likelihood analysis of alpha and beta sequences using PROTGAMMALG model of amino acid substitution (no outgroup)
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

