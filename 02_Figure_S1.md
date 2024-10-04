### Data preparation and processing:

![R](https://img.shields.io/badge/r-4.3.3-blue.svg)
##### Load required libraries:
```r
library(tidyverse)
library(readr)
library(Biostrings)
library(readxl)
```

Process the taxonomy and metadata:
```r
# Import the taxonomy metadata
RefSeq_taxonomy_metadata <- read_xlsx("/path/to/Data_S1.csv")

species_meta <- RefSeq_taxonomy_metadata[,c(1,3,40,46,47)]
```

Import metadata and sequences:
```r
# directory to the NLRtracker outputs for the selected species from Toghani, A. and S. Kamoun., 2024., https://doi.org/10.5281/zenodo.13627395
setwd("/path/to/NLRtacker_outputs/")

RefSeq_files <- list.files(pattern = "_Domains.tsv", recursive = T)

RefSeq_list <- list()

# Loop through each file
for (i in seq_along(RefSeq_files)) {
  # Read the Excel file
  RefSeq_list[[i]] <- read_delim(RefSeq_files[i])
  
  # Extract the specific part of the file name and add it as a new column
  RefSeq_list[[i]] <- RefSeq_list[[i]] %>%
    mutate('Assembly Accession' = gsub(".*_(GCA|GCF)_([0-9]+\\.[0-9]+)_.*", "\\1_\\2", basename(RefSeq_files[i])))

  # Print a message for each file (optional)
  cat("Read file:", RefSeq_files[i], "and added formatted filename column\n")
}


# merge all and add the metadata
RefSeq_NLRtracker <- do.call(rbind,RefSeq_list) %>% inner_join(species_meta, by = "Assembly Accession")


RefSeq_NLRtracker <- RefSeq_NLRtracker %>% 
  mutate(ID = paste(`Order name`,`Organism Name`, seqname, Simple, sep = "_")) %>%
  # Replace spaces in the "Species" column with underscores for the "ID" column
  mutate(ID = gsub(" ", "_", ID))

```

Prepare the functions and prerequisites:
```{r}
# Function to remove duplicates based on "sequence" column
remove_duplicates_by_sequence <- function(df) {
  # Filter out rows with duplicate "sequence" values
  df %>% distinct(sequence, .keep_all = TRUE)
}

domains <- c("CNL","CNLO","CN","OCNL","CONL","CNNL","CNLOLO",
             "NL","NLO","ONL",
             "BCNL","BNL","BCN","BCCNL","BNLO","BOCNL",
             "RNL",
             "TN","TNL","TNLO","TNLJ")

```



### Script to generate the phylogenetic tree:

```bash
#!/bin/bash

#SBATCH -c 16
#SBATCH --mem=30G

source famsa-2.2.2
source fasttree-2.1.10


# Run famsa to generate the .afa file
srun famsa ${1} `basename ${1} .fasta`_famsa.afa

# Use the generated .afa file as input for FastTree
srun FastTree -lg `basename ${1} .fasta`_famsa.afa > `basename ${1} .fasta`_famsa.newick
```
