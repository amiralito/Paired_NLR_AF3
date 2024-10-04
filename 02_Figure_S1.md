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
```r
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


Process the data:
```r
# extract the full-length NLRs
RefSeq_NLR <- filter(RefSeq_NLRtracker, RefSeq_NLRtracker$type == "CHAIN" & RefSeq_NLRtracker$Status == "NLR")

# only keep the desired domain architectures and deduplicate NLRs
RefSeq_NLR_filtered <- RefSeq_NLR[RefSeq_NLR$Simple %in% domains,]

RefSeq_NLR_filtered_deduplicated <- remove_duplicates_by_sequence(RefSeq_NLR_filtered)



# extract the NBARC domains
RefSeq_NBARC <- RefSeq_NLRtracker[RefSeq_NLRtracker$description == "NBARC",]

RefSeq_NBARC_filtered_deduplicated <- RefSeq_NBARC[RefSeq_NBARC$ID %in% RefSeq_NLR_filtered_deduplicated$ID,]


# filter out NLRs with truncated NBARC domain

RefSeq_NBARC_filtered_deduplicated_len <- filter(RefSeq_NBARC_filtered_deduplicated, 
                                                              RefSeq_NBARC_filtered_deduplicated$end - RefSeq_NBARC_filtered_deduplicated$start > 150 &
                                                              RefSeq_NBARC_filtered_deduplicated$end - RefSeq_NBARC_filtered_deduplicated$start < 500)


RefSeq_NLR_filtered_deduplicated_len <- RefSeq_NLR_filtered_deduplicated[RefSeq_NLR_filtered_deduplicated$seqname %in% RefSeq_NBARC_filtered_deduplicated_len$seqname,]

# convert the final data to biostring objects

RefSeq_NLR_filtered_deduplicated_len_seq <- AAStringSet(RefSeq_NLR_filtered_deduplicated_len$sequence)
RefSeq_NLR_filtered_deduplicated_len_seq@ranges@NAMES <- RefSeq_NLR_filtered_deduplicated_len$ID


## add a suffix to NLRs with two NBARCs
RefSeq_NBARC_filtered_deduplicated_len <- RefSeq_NBARC_filtered_deduplicated_len %>%
  group_by(ID) %>%
  mutate(`deduplicated ID` = paste0(ID, ifelse(row_number() == 1, "", paste0("_", row_number())))) %>%
  ungroup()

RefSeq_NBARC_filtered_deduplicated_len_seq <- AAStringSet(RefSeq_NBARC_filtered_deduplicated_len$sequence)
RefSeq_NBARC_filtered_deduplicated_len_seq@ranges@NAMES <- RefSeq_NBARC_filtered_deduplicated_len$`deduplicated ID`

```

Import RefPlantNLR and references, and add them to the datasets and export:
```r
RefPlantNLR <- readAAStringSet("/path/to/data/references/RefPlantNLR.fasta")
RefPlantNLR_NBARC <- readAAStringSet("/path/to/data/references/RefPlantNLR_NBARC.fasta")

reference_pairs <- readAAStringSet("/path/to/data/references/nlrtracker_characterized_paired_NLRs_NLR.fasta")
reference_pairs_NBARC <- readAAStringSet("/path/to/data/references/nlrtracker_characterized_paired_NLRs_NBARC.fasta")

new_refs <- reference_pairs[!reference_pairs@ranges@NAMES %in% RefPlantNLR_NBARC@ranges@NAMES]
new_refs_NBARC <- reference_pairs_NBARC[!reference_pairs_NBARC@ranges@NAMES %in% RefPlantNLR_NBARC@ranges@NAMES]

all_ref <- c(RefSeq_NLR_filtered_deduplicated_len_seq, RefPlantNLR, new_refs)
all_ref_NBARC <- c(RefSeq_NBARC_filtered_deduplicated_len_seq, RefPlantNLR_NBARC, new_refs_NBARC)

writeXStringSet(all_ref_NBARC,"/path/to/all_ref_nbarc.fasta") # This will be used for alignment and generating the phylogenetic tree
writeXStringSet(all_ref,"/path/to/all_ref.fasta")
```

-------------

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
