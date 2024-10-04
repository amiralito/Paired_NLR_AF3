### Methods

We extracted 8,377 NLR proteins from the NLRtracker output of 13 RefSeq proteomes, including two dicot species (Arabidopsis thaliana and Solanum lycopersicum) and 11 monocot species (Zea mays, Triticum aestivum, Solanum lycopersicum, Setaria viridis, Phragmites australis, Phoenix dactylifera, Oryza sativa Japonica Group, Musa acuminata AAA Group, Lolium perenne, Hordeum vulgare subsp. vulgare, Brachypodium distachyon, and Asparagus officinalis) (Data S1 and S2) (Toghani and Kamoun, 2024)[https://doi.org/10.5281/zenodo.13627395] We retained only NLRs with specific domain architectures defined by NLRtracker: "CNL", "CNLO", "CN", "OCNL", "CONL", "CNNL", "CNLOLO", "NL", "NLO", "ONL", "BCNL", "BNL", "BCN", "BCCNL", "BNLO", "BOCNL", "RNL", "TN", "TNL", "TNLO", and "TNLJ." This filtering step resulted in 7,592 sequences.

After removing redundant sequences, 5,097 sequences remained. We further filtered sequences based on NB-ARC domain length, keeping only those between 150 and 500 amino acids. This left 4,936 sequences (Data S2). We added a suffix to the second NB-ARC domain to avoid duplicated IDs in cases where NLRs contained two NB-ARCs. Next, we incorporated NB-ARC domain sequences for NLR pairs from Stein et al. and RefPlantNLR, adding them to our extracted set for phylogenetic analysis (Table S1 and S5) (Kourelis et al., 2021).

The NB-ARC sequences were aligned using FAMSA v2.2.2 (Deorowicz et al., 2016), and the alignment was used to construct a phylogenetic tree with FastTree v2.1.10 [-lg] (Price et al., 2010) (Data S3). We rooted the resulting tree at the CCR-NLR clade. Finally, the phylogenetic tree, along with reference sequences and genetic linkage data, was visualized using iTOL (Letunic and Bork, 2021).


References:

- Toghani, A. and S. Kamoun. Functional Annotation of 180 Refseq Reference Plant Proteomes Reveals a Dataset of 113,684 NLR Proteins. Zenodo, Sept. 2024, doi:10.5281/zenodo.13627395.

- Kourelis, Jiorgos et al. “RefPlantNLR is a comprehensive collection of experimentally validated plant disease resistance proteins from the NLR family.” PLoS biology vol. 19,10 e3001124. 20 Oct. 2021, doi:10.1371/journal.pbio.3001124

- Deorowicz, Sebastian et al. “FAMSA: Fast and accurate multiple sequence alignment of huge protein families.” Scientific reports vol. 6 33964. 27 Sep. 2016, doi:10.1038/srep33964

- Price, Morgan N et al. “FastTree 2--approximately maximum-likelihood trees for large alignments.” PloS one vol. 5,3 e9490. 10 Mar. 2010, doi:10.1371/journal.pone.0009490

- Letunic, Ivica, and Peer Bork. “Interactive Tree Of Life (iTOL) v5: an online tool for phylogenetic tree display and annotation.” Nucleic acids research vol. 49,W1 (2021): W293-W296. doi:10.1093/nar/gkab301




