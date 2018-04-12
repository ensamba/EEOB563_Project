## Codes for constructing phylogenetic relationships between beta and alpha tubulins and comparison of sequence evolution rates between the two sequences

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
I CalculateD 4 different matrices using 4 models available in protdist and perform a NJ analysis with each of them. 
I went ahead and calculated a strict consensus tree rooted using an appropriate outgroup.
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
I went a head and appended all the tree files from different models and run both a strict and majority consensus tree for all of them
#### Strict consensus tree
```
Consensus tree program, version 3.696

Settings for this run:
 C         Consensus type (MRe, strict, MR, Ml):  strict
 O                                Outgroup root:  No, use as outgroup species  1
 R                Trees to be treated as Rooted:  No
 T           Terminal type (IBM PC, ANSI, none):  ANSI
 1                Print out the sets of species:  Yes
 2         Print indications of progress of run:  Yes
 3                               Print out tree:  Yes
 4               Write out trees onto tree file:  Yes

Are these settings correct? (type Y or the letter for one to change)
Y
```
#### Majority rule tree
```
Settings for this run:
 C         Consensus type (MRe, strict, MR, Ml):  Majority rule (extended)
 O                                Outgroup root:  No, use as outgroup species  1
 R                Trees to be treated as Rooted:  No
 T           Terminal type (IBM PC, ANSI, none):  ANSI
 1                Print out the sets of species:  Yes
 2         Print indications of progress of run:  Yes
 3                               Print out tree:  Yes
 4               Write out trees onto tree file:  Yes

Are these settings correct? (type Y or the letter for one to change)
Y
```

### Hypothesis Testing and Detecting Selection with codeml
codeml is a part of the PAML package, which is a suite of programs for phylogenetic analyses of DNA or protein sequences using maximum likelihood (ML). 
My goal is to determine the rates of evolution in the beta tubulin genes and ascertain the underlying selection pressure. 
For this particular analysis since I was dealing with  protein-coding DNA sequences, I set was seqtype set to 1 and carried out ML analysis using codon substitution models (e.g., Goldman and Yang 1994).

I run the alpha and beta tubulin gene sequences separately and compared the omegas that is the dN/dS ratios to see the underlying selection pressure.


#### codeml for beta tubulin gene sequences
I set kappa and omega to be estimated and run codeml under the following settings
```
seqfile = beta_nt_no_out.aln  * sequence data filename
      outfile = results1.txt   * main result file name

        noisy = 9      * 0,1,2,3,9: how much rubbish on the screen
      verbose = 1      * 1:detailed output
      runmode = -2     * -2:pairwise

      seqtype = 1      * 1:codons
    CodonFreq = 2      * 0:equal, 1:F1X4, 2:F3X4, 3:F61
        model = 0      *
      NSsites = 0      *
        icode = 0      * 0:universal code

    fix_kappa = 0      * 1:kappa fixed, 0:kappa to be estimated
        kappa = 2      * initial or fixed kappa

    fix_omega = 0      * 1:omega fixed, 0:omega to be estimated
        omega = 0.2  * 1st fixed omega value [change this]
```
To extract the omega values
```
vi results1.txt
```
#### codeml for alpha tubulin gene sequences
```
 seqfile = alpha_no_out.aln  * sequence data filename
      outfile = results1.txt   * main result file name

        noisy = 9      * 0,1,2,3,9: how much rubbish on the screen
      verbose = 1      * 1:detailed output
      runmode = -2     * -2:pairwise

      seqtype = 1      * 1:codons
    CodonFreq = 2      * 0:equal, 1:F1X4, 2:F3X4, 3:F61
        model = 0      *
      NSsites = 0      *
        icode = 0      * 0:universal code

    fix_kappa = 0      * 1:kappa fixed, 0:kappa to be estimated
        kappa = 2      * initial or fixed kappa

    fix_omega = 0      * 1:omega fixed, 0:omega to be estimated
        omega = 0.2  * 1st fixed omega value [change this]
```

