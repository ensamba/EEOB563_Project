## Codes for replicating reconstruction of alpha tubulin phylogeny using Maximum likelihood as infered by Raxml
```
cp alphatub_nt.fasta Replicate.alpha/
```
#### Multiple sequence alignment and converting to phylip
```
mafft --auto --phylipout alphatub_nt.fasta > alpha_nt.phylip 
```
### Install RAxML
Fetch the latest RAxML codebase from github and make the executable. There are many parallel versions of RAxML
```
git clone https://github.com/stamatak/standard-RAxML.git
cd standard-RAxML
make -f Makefile.SSE3.PTHREADS.gcc
ls -ltr
...
-rwxr-xr-x. 1 ensamba domain users 1247032 Mar 31 10:19 raxmlHPC-PTHREADS-SSE3
```
### Likelihood analysis using RAxML
RAxML versions installed on HPC but with long and ugly names (raxmlHPC-MPI-AVX, raxmlHPC-PTHREADS-AVX, raxmlHPC-PTHREADS-SSE3). To make our life easier, I will create an alias for one of them:
```
$ alias raxmlHPC='raxmlHPC-PTHREADS-SSE3 -T2'
$ raxmlHPC -h
```
#### Likelihood analysis using GTRGAMMA model of nucleotide evolution using C.elegans as an outgroup
```
raxmlHPC-PTHREADS-SSE3 -T2 -m GTRGAMMA -o CAEEL -p 12345 -s alphatub_nt.phy -n alphatub
```
#### To view the tree, I used newick tree viewer
```
module load newick_utils
nw_display RAxML_bestTree.alphatub 
```

### Bootsrap analysis on hpc class using C.elegans as an outgroup
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
raxmlHPC-PTHREADS-SSE3 -f a -m GTRGAMMA -o CAEEL  -p 12345 -x 12345 -# 1000 -s alphatub_nt.phy -n alphatub.bootstrap1
```
This took only 04 minutes to run.
### Adding and Commiting files to Github repository
```
 git add  Replicate.alpha
 git commit -a
 git push origin master
 ```
 

