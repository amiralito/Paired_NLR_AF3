
##### Script to generate the phylogenetic tree:

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