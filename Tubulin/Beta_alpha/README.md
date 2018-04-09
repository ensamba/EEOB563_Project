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

