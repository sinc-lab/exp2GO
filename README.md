# exp2GO
This repository contains the data and source code for the original methods proposed in:

L. Di Persia, T. Lopez, A. Arce, D.H. Milone, and G. Stegmayer, "*exp2GO: improving prediction of functions in the Gene Ontology with expression data*", 2021.

exp2GO is a novel method for the computational prediction of gene function annotations based on the inference of GO similarities from expression similarities.

![exp2go](exp2go.png)

**Pipeline of exp2GO for inferring GO labels**. A) Expression data. B) Expression pairwise Euclidean distance matrix dE among all genes in the study. C) Semantic data: GO annotations for well-known genes (gene 1, gene 2, ...); some genes in the study are completely unknown (gene A, gene B, ...). D) Semantic distance matrix dGO among all genes: with missing rows and columns because many genes are not semantically annotated. E) and F)With exp2GO it is possible to complete the missing distances in dGO, using the information available in B) and D). G) Once the dGO matrix is completed, GO annotations are assigned according to the reconstructed semantic space. From the closest set of genes with known GO terms, the potential terms
(according to a Bayesian model) are sorted in descending order by their posterior probability. Finally, the candidate GO terms with the highest accumulated probability (in yellow) are assigned to each un-annotated gene (gene A, gene B, ...).




This repository provides the data and several notebooks to use exp2GO. It is open sourced and free to use. If you use any part of this, please cite our work. 

## Quick start 

Reproducing paper results: the next notebooks reproduce the results presented in the paper. 

1. [Expression data](https://colab.research.google.com/github/sinc-lab/exp2GO/blob/master/notebooks/01_expression_ara_espinoza.ipynb): 
  - conversion of the original expression files to an unified format
  - remove the empty genes
  - calculate several distance matrices (euclidean, correlation, cosine...)

- `02_annotations`:
  - extract [annotations](https://colab.research.google.com/drive/1K0fEeDMnyHTfTJhoHO3TKhLXbfaz1952) from GAF
  - search for gene names in DB_Synonym
  - only for expressed genes
  - for each species, for T-1, T0 and T1
  - with experimental annotations
  - Biological process
  - `bkp/exp2go_Ancestors_oboT-1_gafT0.ipynb`: alternative version downloading obo and gaf.

- `41loo/`:  Leave-one-out (LOO) experiment

  - `03_ancestors41`:
    - propagate [ancestors](https://colab.research.google.com/drive/1h2pAKVhHA3TgQ5PMs18duu156tBoywuQ) for the annotations in 02
    - using CAFA3 reference OBO (2016-06-01) 
  - `04_semantic_dist_genes`:
    - semantic [distance](https://colab.research.google.com/drive/1-5cbXyF2y5PF-vlRItutEsY7cve68Dbu#scrollTo=Ue9sA8t1hUj2) between genes
    - for annotations extracted in 02
    - provides Resnik, Lin, Relevance
    - integrated by gene with min, max, average or BMA

- `42delta/`:  Delta-T experiment (CAFA3)
  - `03_filter_terms`:
    - [filter terms](https://colab.research.google.com/drive/1_S56rMVPt5Iyx5SU5dn_vxPLePmULG0V) from the annotations file in step 02
      - with less than 3 occurrences
      - not in the intersection of terms in T-1 and T0

  - `04_ancestors42`:
    - the same as `03_ancestors41` but [using terms filtered](https://colab.research.google.com/drive/11VbEyJFw7cXylfu-chbHLiwCvrtrfsth) in previous step
    - using CAFA3 referece OBO (2016-06-01) 
    - `04b_convert_to_onehot`: [convert](https://colab.research.google.com/drive/19-OlNx4c7siHWLDpQ_91RKjOt3GpKski#scrollTo=Ue9sA8t1hUj2) output file format for matlab compatibility

  - `051_semantic_dist_genes`:
    - semantic [distance](https://colab.research.google.com/drive/1M9p3K4MkrjJXKoEbI0rGymM1C-zyLqZZ#scrollTo=Ue9sA8t1hUj2) between genes
    - from the output of step `03_filter_terms`

  - `052_semantic_dist_terms`:
    - semantic [distance](https://colab.research.google.com/drive/1IhhhU2CgJBZTPdXBFpZ6bAt1NfLBLq_H#scrollTo=Ue9sA8t1hUj2) between isolated terms
    - from the output of step `04_ancestors42`
    - provides Resnik, Lin, Relevance
