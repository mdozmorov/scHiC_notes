# Data from https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE223917

Liu, Zhiyuan, Yujie Chen, Qimin Xia, Menghan Liu, Heming Xu, Yi Chi, Yujing Deng, and Dong Xing. “Linking Genome Structures to Functions by Simultaneous Single-Cell Hi-C and RNA-Seq.” Science 380, no. 6649 (June 9, 2023): 1070–76. https://doi.org/10.1126/science.adg3797.

```{bash}
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/soft/GSE223917_family.soft.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/miniml/GSE223917_family.xml.tgz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/matrix/GSE223917_series_matrix.txt.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_RAW.tar
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_HiRES_brain.rna.umicount.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_HiRES_brain_metadata.xlsx
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_HiRES_emb.rna.umicount.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_HiRES_emb_GADIs.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_HiRES_emb_metadata.xlsx
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_HiRES_emb_residuals-updated-02-22-2023.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_HiRES_emb_significant_DIs.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_brain_genome1.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_brain_genome2.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_embryo_genome1.tsv.gz
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE223nnn/GSE223917/suppl/GSE223917_embryo_genome2.tsv.gz
```

- gene expression, allele-specific
    - `GSE223917_embryo_genome1.tsv.gz`, `GSE223917_embryo_genome2.tsv.gz`
    - `GSE223917_brain_genome1.tsv.gz`, `GSE223917_brain_genome2.tsv.gz`

- scHi-C, 20kb resolution
    - `GSE223917_RAW` - lots of files like `GSM6998595_GasaE751001.pairs.gz` (pairs format) and `GSM6998595_GasaE751001.20k.0.clean.3dg.txt.gz` (not sure)

- Cell IDs vs. metadata. Metadata includes "Cell type"
    - `GSE223917_HiRES_brain_metadata.xlsx` 
      20 Ast - astrocytes
     204 Ex1 - excitatory neuron cluster 1
      43 Ex2
      13 Ex3
      52 In1 - inhibitory neuron cluster 1
      47 In2
      20 Oli - oligodendrocytes
    - `GSE223917_HiRES_emb_metadata.xlsx`
     387 ExE ectoderm
     485 ExE endoderm
     304 ExE mesoderm
     128 NMP
     701 blood
     447 early mesenchyme
     413 early mesoderm
     476 early neurons
     139 endoderm
     221 epiblast and PS
     304 epithelial cells
     340 intermediate mesoderm
     563 mitosis
     953 mix late mesenchyme
      67 myocytes
     454 neural ectoderm
     227 neural tube
     137 notochord
     180 oligodendrocytes and progenitors
     473 radial glias
      70 schwann cell precursors

- Differential interactions
    - `GSE223917_HiRES_emb_significant_DIs.tsv.gz`
     151513 early_mesenchyme
     444692 early_mesoderm
    1416102 early_neurons
      51898 endoderm
     360132 epiblast_and_PS
      98065 epithelial_cells
     217127 ExE_mesoderm
     176388 intermediate_mesoderm
    1335808 mix_late_mesenchyme
      13926 myocytes
     748130 neural_ectoderm
      66632 neural_tube
      24201 NMP
      33558 notochord
      96461 oligodendrocytes_and_progenitors
     454679 radial_glias
        481 schwann_cell_precursors

- Using the embryonic top DIs and gene expression profiles, we identified 390,252 significant DI-gene associations (FDR <1%), involving 1955 genes and 223,607 DIs, which we refer to as gene-associated DIs (GADIs)
    - `GSE223917_HiRES_emb_GADIs.tsv.gz`
    - `GSE223917_HiRES_emb_residuals-updated-02-22-2023.tsv.gz` - We then calculated a “residual” for each GADI-gene pair, defined as the difference of the GADI interaction strength and the expression level from the same metacell, similarly as in the “chromatin potential” approach (44), and performed a one- sample t test using all metacells along the trajectory to determine whether the residual was significantly deviated from zero
