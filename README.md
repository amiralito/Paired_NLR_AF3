# Phylogenetic analysis of paired NLRs in rice.

# Supporting scripts and material for "Can AI predictions of protein structures distinguish between sensor and helper NLR immune receptors?"

AmirAli Toghani, Ryohei Terauchi, Sophien Kamoun, Yu Sugihara


Resources:
Software                            | Source
------------------------------------| ------------------------------------
*FAMSA v2.2.2*                      | ([https://github.com/refresh-bio/FAMSA](https://github.com/refresh-bio/FAMSA))
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
