## Codes for phylogenetic construction of beta tubulin genes using maximum likelihood
#### Changing Directory to Beta tubulin
```
cd Beta_only/
```
#### Multiple sequence alignment and converting to phylip for Maximum likelihood analysis
```
mafft --auto --phylipout betatub_nt.fasta > beta_nt.phy
```
#### Likelihood analysis using RAxML setting C.elegans beta tubulin as an outgroup
To construct maximum Likelihood tree, I will use GTRGAMMA model of nucleotide evolution using C.elegans beta tubulin as an outgroup. I am using GTRGAMMA model because it assumes different rates of substitution for each pair of nucleotides, in addition to assuming different frequencies of occurrence of nucleotides. It also models rates of heterogeneity over different sites.

```
raxmlHPC-PTHREADS-SSE3 -T2 -m GTRGAMMA -o CAEEL_tbb-1 -p 12345 -s beta_nt.phy -n betatub
```
#### To view the tree, I used newick tree viewer
module load newick_utils

nw_display RAxML_bestTree.alphatub 

#### Bootsrap analysis (1000 replicates) on hpc class using C.elegans as an outgroup
Used a Slurm job script generator via hpc_class I generated the following bash script to run 1000 iterations across trees
```
#!/bin/bash

#copy/paste this job script into a text file and submit with the command:
#    sbatch thefilename
#SBATCH --time=4:00:00   # walltime limit (HH:MM:SS)
#SBATCH --nodes=1   # number of nodes
#SBATCH --ntasks-per-node=16   # 32 processor core(s) per node 
#SBATCH --mem=62G   # maximum memory per node
#SBATCH --job-name="beta_raxml"
#SBATCH --mail-user=ensamba@iastate.edu   # email address
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL
#SBATCH --output="raxml.out1" # job standard output file (%j replaced by job id)
# LOAD MODULES, INSERT CODE, AND RUN YOUR PROGRAMS HERE
module load raxml
raxmlHPC-PTHREADS-SSE3 -f a -m GTRGAMMA -o CAEEL_tbb-1 -p 12345 -x 12345 -# 1000 -s beta_nt.phy -n betatub.bootrap
```
### Using Bootsrap replicates to build consensus trees, RAxML supports strict, majority rule, and extended majority rule consenus trees:
#### Strict consensus
```
raxmlHPC -m GTRCAT -J STRICT -z RAxML_bootstrap.betatub.bootrap -o CAEEL_tbb-1 -n strict_beta
```
#### Majority rule
```
raxmlHPC -m GTRCAT -J MR -O CAEEL_tbb-1 -z RAxML_bootstrap.betatub.bootrap -n majority_beta
```
#### Extended majority rule
```
raxmlHPC -m GTRCAT -J MRE -o CAEEL_tbb-1  -z RAxML_bootstrap.betatub.bootrap -n extmaj_beta 
```
raxmlHPC -m GTRCAT -J MRE -o CAEEL_tbb-1  -z RAxML_bootstrap.betatub.bootrap -n extmaj_beta 
```




