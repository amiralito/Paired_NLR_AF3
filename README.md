# Phylogenetic analysis of paired NLRs in rice.

[![DOI](https://img.shields.io/badge/bioRxiv-doi.org/10.1101/2024.11.24.625045-BE2634.svg)](https://doi.org/10.1101/2024.11.24.625045)
[![DOI](https://img.shields.io/badge/new_phytologist-doi.org/10.1111/nph.70391-29AB87)](https://doi.org/10.1111/nph.70391)


# Supporting scripts and material for "Can AI predictions of protein structures distinguish between sensor and helper NLR immune receptors?"

AmirAli Toghani, Raoul Frijters, Tolga O. Bozkurt, Ryohei Terauchi, Sophien Kamoun, Yu Sugihara


Resources:
Software                            | Source
------------------------------------| ------------------------------------
*FAMSA v2.2.2*                      | ([https://github.com/refresh-bio/FAMSA](https://github.com/refresh-bio/FAMSA))
*MAFFT v7.525*                      | (https://github.com/GSLBiotech/mafft)
*FastTree v2.1.10*                  | ([http://www.microbesonline.org/fasttree/](http://www.microbesonline.org/fasttree/))
*R v4.3.3*                          | ([https://cran.r-project.org/](https://cran.r-project.org/))
*NLRtracker*                        | ([https://github.com/slt666666/NLRtracker](https://github.com/slt666666/NLRtracker))



R packages:
```R
install.packages("tidyverse")

if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("Biostrings")
```


* [Part one - Materials and Methods](/01_materials_and_methods.md)
* [Part two - Scripts for Figure S1](/02_Figure_S1.md)
