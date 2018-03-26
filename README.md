## EEOB563_Project

### Gene family evolution of mammalian beta-tubulin
#### Introduction 
Microtubules (MTs) are filaments of the eukaryotic cytoskeleton assembled from the protein tubulin. Among their many roles, MTs represent the principal structural components of the mitotic spindle, are essential in maintaining cell shape, motility and to facilitate intracellular transport and intracellular communication. Tubulin is a heterodimer of alpha- and beta-subunits; the sequence, and structure of tubulin subunits is highly conserved among species. Many eukaryotic organisms carry multiple genomic copies of functional α or β tubulin, commonly referred to as isoforms (or isotypes if they are confined to a single organism) whose protein sequences are very well conserved. In humans for example, 9 genes encode for the α-subunit whereas 10 genes encode the β-subunit which are assembled into functional microtubule polymers. Mutations in different mammalian tubulin proteins have been linked to a wide range of disorders many of which affect brain development (Mark I. Rees et al, 2014; Beta tubulin is of critical function because most of the available data in the literature deals with this protein as a target for drug action and protein-protein interactions (Huzil et al, 2007). For instance, all drugs targeting microtubules for cancer chemotherapy are designed to bind to beta tubulin. The antitumor drugs stabilize microtubules and reduces their dynamicity, promoting mitotic arrest and eventually apoptosis. Mutations in specific beta-tubulin isotypes cause severe neuropathies that disrupt axonal transport and cancer (Huzil et al, 2007). Genetic variations affecting all beta-tubulin genes expressed at high levels in the brain (TUBB2B, TUBB3, TUBB, TUBB4A, and TUBB2A) have been linked with malformations of cortical development and disease phenotypes that arise from their disruption, include microcephaly, lissencephaly and polymicrogyria (Thomas D. Cushion, et al. 2014)
To date, mammalian beta tubulin has been grouped into 9 classes, class I, IIa, IIb, III, IVa, IVb, V, VI, VIII (HUGO Gene Nomenclature Commitee). However, searching through literature, no phylogenic study has been done/found categorizing these genes in their respective classes like it was done with α-tubulin partners (Varsha K. Khodiyar et al, 2007).  Thus, the basis of beta tubulin classification is not yet clear. Despite lack of the consensus phylogeny, it has been shown that class I β-tubulin is the most commonly expressed isotype in humans and as such is also the most common isotype found in cancer cells. Alternatively, both classes II and III β-tubulin have been observed at increased levels in human tumors (Ferguson et al. 2005; Mozzetti et al. 2005). 
 There is a degree of tissue specificity in the expression of some β- tubulins as described and some degree of gene redundancy where the loss of one gene can be compensated by over expression of another as has been shown in yeast (Nsamba et.al unpublished)
#### This proposal is set to;
 (a) Validate the nomenclature of human beta tubulins by phylogenetic construction of molecular data using maximum likelihood (nucleotide sequences)
I hypothesize beta isotypes to be orthologous to each other and expect them to form 08 clades of evolutionary relationships as reported in the literature.
(b). Replicate phylogenetic analysis of mammalian alpha-tubulin as presented by V.K. Khodiyar et al. 2007, in their paper entitled " A revised nomenclature for the human and rodent α-tubulin gene family) which categorized the 09 alpha tubulin genes into 4 major groups using neighbor joining.  I would like to use maximum likelihood instead of NJ to construct phylogeny of alpha tubulins and compare it with their results. (nucleotide)
(c) Determine molecular evolutionary relationship between the two major classes of mammalian tubulin using Neighbor joining. By using the NJ algorithm, I expect the two tubulin classes to be paralogous to each other and orthologous with in beta and alpha isotypes. (protein)

#### Methods
Sources of data: UniProtKB. and NCBI databases for protein sequences and nucleotide sequences respectively.
Protein sequences
The mammalian alpha tubulin protein sequences will be those used in V.K. Khodiyar et al. 2007, (A revised nomenclature for the human and rodent α-tubulin gene family) whereas the beta tubulin sequences will be extracted from UniProtKB database (http://www.uniprot.org/). Accession numbers will be provided. In total, I will work with 19 protein sequences (9-α and 10-β tubulin)
#### Nucleotide sequence;
Both α or β-tubulin gene sequences will be extracted from NCBI database using their corresponding gene names as published in literature.  Only the protein-coding portion of each cDNA will be used, to prevent differences in length of the UTRs biasing the alignments.  These were extracted from (https://www.ncbi.nlm.nih.gov/CCDS/CcdsBrowse.cgi) a sub-project of NCBI for annotating coding regions. For neighbor joining analysis, I will use C. elegans α-2 tubulin as an outgroup thus in total, I will have 20 nucleotide sequences from which I will build the phylogeny. I couldn't find the coding sequence of TUBB7P (Homo sapiens tubulin beta 7 pseudogene) and therefore excluded it from the analysis leaving only 9 human -tubulin genes.

#### Performing multiple sequence alignment 
I will employ a MAFFT module available on HPC class to align the sequences and save them in a fasta format. Depending on the type of analysis, I will convert the fasta file in different formats that suit a specific phylogenetic analysis method as described below. For example, to perform a maximum likelihood of protein sequences, I will save them to nexus format; whereas for distance analysis using NJ, I will convert them to phylip format. 
Phylogenetic analyses 
Maximum likelihood
Maximum likelihood phylogenetic tree will be inferred using RAxML77 v.8.0.22 with the GAMMA-LG model of evolution for protein data and GTRGAMMA for nucleotide sequences. 

#### Distance analyses (PHYLIP)
Pairwise distances among taxa will be used as input for phylogenetic reconstruction with the neighbor-joining algorithm.  For protein sequences, distance matrices of the input data will be estimated under five different models of amino acid substitutions available in protdist; the Dayhoff PAM matrix, the JTT (Jones-Taylor-Thornton) model, the PMB (Probability Matrix from Blocks) model, Kimura's distance, and Categories distances) after perform a neighbor joining for each one of them using the C. elegans α-2 tubulin as an outgroup. 
Whereas for nucleotide sequences, distance matrices will be estimated under the 4 models of nucleotide substitutions available in dnadist; the Jukes-Cantor, Kimura, and F84 models of sequence evolution and perform a neighbor joining analysis for each one of them C. elegans α-2 tubulin as an outgroup.
I will build a strict and majority consensus tree from the protein and nucleotide data.

#### Bootstrap tree analysis
One thousand bootstrap replicates will be performed for each analysis. In all the above methods of phylogenetic construction of my taxa, I will quantify the branching confidence in the inferred evolutionary tree by bootstrapping with 1000 iterations. The bootstrap origin classification confidence is the fraction of bootstrapped trees that fall into the same category as the real tree, times 1000.

#### Tree visualization
I will employ the following packages to visualize trees especially those with bootstrap support values. 
Newick tree viewer
Dendroscope (http://www-ab.informatik.uni-tuebingen.de/software/dendroscope)
Figtree


#### References
Huzil et al, 2007, The Roles of β-Tubulin Mutations and Isotype Expression
in Acquired Drug Resistance. Cancer Informatics 2007: 3 159-181
V.K. Khodiyar et al. 2007. A revised nomenclature for the human and rodent α-tubulin gene family. Genomics 90 (2007) 285-289
Thomas D. Cushion et a., 2014, De Novo Mutations in the Beta-Tubulin Gene TUBB2A Cause Simplified Gyral Patterning and Infantile-Onset Epilepsy. The American Journal of Human Genetics 94, 634-641, April 3, 2014
Takeshi Kawauchi, 2017. Tubulin isotype speci city in neuronal migration: Tuba8 can't ll in for Tuba1a 





