# Phylogenetic analysis of paired NLRs in rice.

Supporting scripts and material for:
# Can AI predictions of protein structures distinguish between sensor and helper NLR immune receptors?

AmirAli Toghani, Sophien Kamoun, Yu Sugihara


DOI: 



Resources:
Software                            | Source
------------------------------------| ------------------------------------
*FAMSA v2.2.2*                      | (https://github.com/GSLBiotech/mafft)
*FastTree v2.1.10*                  | (http://www.microbesonline.org/fasttree/)
*R v4.3.1*                          | (https://cran.r-project.org/)
*NLRtracker*                        | (https://github.com/slt666666/NLRtracker)


R packages:
```R
install.packages("tidyverse")

if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("Biostrings")
BiocManager::install("ggtree")
```


* [Part one - Materials and Methods](/01_materials_and_methods)
