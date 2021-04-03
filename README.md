# Notes on single-cell Hi-C technologies, tools, and data

[![MIT License](https://img.shields.io/apm/l/atomic-design-ui.svg?)](https://github.com/tterb/atomic-design-ui/blob/master/LICENSEs) [![PR's Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com) 

Single-cell 3D genomics notes. Please, [contribute and get in touch](CONTRIBUTING.md)! See [MDmisc notes](https://github.com/mdozmorov/MDmisc_notes) for other programming and genomics-related notes.

# Table of content

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Tools](#tools)
  - [Clustering](#clustering)
- [Papers](#papers)
  - [Clustering, embedding](#clustering-embedding)
  - [Technologies, data](#technologies-data)
    - [scHi-C multi-omics](#schi-c-multi-omics)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Tools

- `scHiCExplorer` - Single cell Hi-C data analysis toolbox, Python, from processing to normalization, clustering, compartment identification, visualization. https://github.com/joachimwolff/scHiCExplorer

- `hickit` - TAD calling, phase imputation, 3D modeling and more for diploid single-cell Hi-C (Dip-C) and general Hi-C. https://github.com/lh3/hickit

- `dip-c` - Tools to analyze Dip-C (or other 3C/Hi-C) data - https://github.com/tanlongzhi/dip-c

- `nuc_processing` - Chromatin contact paired-read single-cell Hi-C processing module for Nuc3D and NucTools. https://github.com/TheLaueLab/nuc_processing.
    - Stevens, Tim J., David Lando, Srinjan Basu, Liam P. Atkinson, Yang Cao, Steven F. Lee, Martin Leeb, et al. “3D Structures of Individual Mammalian Genomes Studied by Single-Cell Hi-C.” Nature, March 13, 2017. https://doi.org/10.1038/nature21429.

### Clustering

- [scHiCTools](https://github.com/liu-bioinfo-lab/scHiCTools) - scHiCTools - a set of tools for high-level analysis (clustering) of scHi-C data. Project single cells in a lower-dimensional Euclidean space. Three methods for smoothing scHi-C data (linear convolution, random walk, network enhancing), three projection methods (fastHiCRep, Selfish, newly developed InnerProduct), three embedding methods if assuming cells come from a continuous manifold (MDS, t-SNE, PHATE), or three clustering methods if assuming cells are from different clusters (k-means, spectral clustering, HiCluster). Brief Methods of each approach. Tested on Nagano 2017 cell-cycle dataset. InnerProduct captures cell similarity well, any embedding works good, linear convolution and random walk improve projections at high dropout rates. QC plots. ACROC - area under the curve of a circular ROC calculation.  Input - text matrices. Python 3.
    - Li, Xinjun, Fan Feng, Wai Yan Leung, and Jie Liu. “[ScHiCTools: A Computational Toolbox for Analyzing Single-Cell Hi-C Data](https://doi.org/10.1101/769513).” Preprint. Bioinformatics, September 18, 2019. 

- [scHiCluster](https://github.com/zhoujt1994/scHiCluster) - s single-cell Hi-C clustering algorithm based on imputation using linear convolution (averaging within a window of size 1 over 1Mb scHi-C matrices) and random walk with restarts. scHi-C challenges: variability, sparsity, coverage heterogeneity. Two-step imputation to resolve sparsity, top-ranked interactions after imputation to resolve heterogeneity. Tested on simulated (from bulk Hi-C controlling for sparsity, and pseudobulk) and experimental (Ramani, four human cell lines; Flyamer, mouse zygotes and oocytes; Nagano) scHi-C data. Against PCA, HiCRep+MDS, the eigenvector method, the decay profile method. Adjusted Rand Index to test clustering quality. TAD-like structures can be detected in imputed data (TopDom). At least 5k contacts per cell is sufficient. Python package. Input - sparse matrices, 1Mb resolution, or juicer-pre format for custom resolution.
    - Zhou, Jingtian, Jianzhu Ma, Yusi Chen, Chuankai Cheng, Bokan Bao, Jian Peng, Terrence J. Sejnowski, Jesse R. Dixon, and Joseph R. Ecker. “[Robust Single-Cell Hi-C Clustering by Convolution- and Random-Walk–Based Imputation](https://doi.org/10.1073/pnas.1901423116).” Proceedings of the National Academy of Sciences, (July 9, 2019)

## Papers

- Li, Xiao, Ziyang An, and Zhihua Zhang. “[Comparison of Computational Methods for 3D Genome Analysis at Single-Cell Hi-C Level](https://doi.org/10.1016/j.ymeth.2019.08.005).” Methods, August 2019 - Assessment of Hi-C methods applied to single-cell Hi-C data. Overview of computational analysis of Hi-C data (normalization, A/B compartment, TAD, loop calling, differential analysis), scRNA-seq data properties. Tested on systematically downsampled data and on experimental scHi-C data. HiCnorm is most performing for normalization, Insulation Score fastHiC for TAD/loop calling. A/B compartments are poorly defined in scHi-C data, TADs can be identified at single-cell level, aggregation improves TAD detection. Adjusted mutual information and weight similarity for TAD similarity assessment. Other methods, like TAD boundary prediction from epigenomic features.
    - [Table 1 - Methods for TAD identification](https://www.sciencedirect.com/science/article/pii/S1046202319300891?via%3Dihub#t0005)
    - [Table 2 - Methods for chromatin loop identification](https://www.sciencedirect.com/science/article/pii/S1046202319300891?via%3Dihub#t0005).

### Clustering, embedding

- Liu, Jie, Dejun Lin, Galip Gürkan Yardimci, and William Stafford Noble. “[Unsupervised Embedding of Single-Cell Hi-C Data](https://doi.org/10.1093/bioinformatics/bty285).” Bioinformatics, (July 1, 2018) - Embedding of scHi-C data. HiCRep with MDS performs best. Contact Probability Function as a means to compare Hi-C matrices. Methods for evaluating reproducibility also can be used to compare matrices, details of HiCRep, GenomeDISCO, HiC-Spector methods. Description of scHi-C datasets and their arrangement by cell cycle stage. 5K total reads per scHi-C matrix is sufficient for proper embedding.

### Technologies, data

- Tan, Longzhi, Wenping Ma, Honggui Wu, Yinghui Zheng, Dong Xing, Ritchie Chen, Xiang Li, Nicholas Daley, Karl Deisseroth, and X. Sunney Xie. “[Changes in Genome Architecture and Transcriptional Dynamics Progress Independently of Sensory Experience during Post-Natal Brain Development](https://doi.org/10.1016/j.cell.2020.12.032).” Cell, January 2021 <span data-badge-type="4" data-doi="10.1016/j.cell.2020.12.032" data-hide-no-mentions="true" class="altmetric-embed"> </span> - Single-cell 3D genomic (3646 cells) and transcriptomic (3517 cells) data of developing (7 time points post-natal) mouse brain (cortex and hippocampus). Dip-C - diploid chromatin conformation capture, MALBAC-DT - multiple annealing and looping-based amplification cycles for digital transcriptomics. 3D genome separates cells into 13 structure types. Three main lineages: neurons, astrocytes, oligodendrocytes. Correlating gene cell type-specific expression with A/B compartmentalization helps to integrate gene expression and the 3D structure. WGCNA modules of correlated genes, dynamics of those modules across time. Integration with published 3D genome, transcriptome, methylome, and chromatin accessibility data. A major 3D genome transformation between P7 (postnatal day 7) and P28 days, genetics determines 3D structure irrespectively of sensory stimuli (visual cortex in dark-reared animals), many more in the text. Data processed with [dip-c - tools to analyze Dip-C data, with documentation](https://github.com/tanlongzhi/dip-c) and [hickit - TAD calling, phase imputation, 3D modeling and more for diploid single-cell Hi-C (Dip-C) and general Hi-C](https://github.com/lh3/hickit). [Processed MALBAC-DT and Dip-C data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE162511). References to other published data in the "Key resourses" table

- Arrastia, Mary V., Joanna W. Jachowicz, Noah Ollikainen, Matthew S. Curtis, Charlotte Lai, Sofia Quinodoz, David A. Selck, Mitchell Guttman, and Rustem F. Ismagilov. “[A Single-Cell Method to Map Higher-Order 3D Genome Organization in Thousands of Individual Cells Reveals Structural Heterogeneity in Mouse ES Cells](https://doi.org/10.1101/2020.08.11.242081).” Preprint. Molecular Biology, August 12, 2020. - **scSPRITE** - Single-cell split-pool recognition of interactions by tag extension.Two triple-sets of split-pool barcoding, nuclear and spatial, DNA phosphate modified (DMP), odd, and even tagging. Paired-end sequencing, read 1 has genomic DNA and the DMP tag, read 2 has other 5 tags. Detects chromatin structures at all scales. Applied to >1000 mESC nuclei. Highly correlate with bulk SPRITE data. Captures multi-way interactions. Captures >30-fold more contacts than scHi-C with <10-fold reads. Processed single-cell matrices, ensemble, https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154353. A Snakemake workflow for data processing, https://github.com/caltech-bioinformatics-resource-center/Guttman_Ismagilov_Labs

- Lee, Dong-Sung, Chongyuan Luo, Jingtian Zhou, Sahaana Chandran, Angeline Rivkin, Anna Bartlett, Joseph R. Nery, et al. “[Simultaneous Profiling of 3D Genome Structure and DNA Methylation in Single Human Cells](https://doi.org/10.1038/s41592-019-0547-z).” Nature Methods, September 9, 2019 - **sn-m3C-seq** - single-nucleus methyl-3C sequencing, extension of snmC-seq2 method, DpnII digestion Fluorescence-Activated Nuclei sorting and the following bisulfite conversion. Cell types can be distinguished by hierarchical clustering (mouse cell types, 4238 human prefrontal cortex cells separated into 14 populations). TAURUS-MH pipeline, outperforms BWA-METH. sn-m3C-seq methylation correlates well with bulk and single-cell methylation measures. More Hi-C contacts than published datasets. Comparing brain cell subpopulations, chromatin interactions overlap, methylation differ, hypomethylation is associated with increased interactions, differential domain boundaries are associated with differential methylation. mESC data (raw FASTQ, >600 samples, >60Gb): https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE124391, human brain data (raw FASTQ, >4K samples, >700Gb): https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE130711. Protocol: https://protocolexchange.researchsquare.com/article/fbc07d40-d794-4cfc-9e7c-294aafbefc10/v1. Interactive methylation data: http://neomorph.salk.edu/snm3Cseq_human_FC.php, Hi-C data: https://dixon.salk.edu/projects/snm3Cseq/. Code scripts https://github.com/dixonlab/scm3C-seq, TAURUS-MH pipeline https://github.com/dixonlab/Taurus-MH, [Twitter](https://twitter.com/Jesse_R_Dixon/status/1171099343236919296?s=03)

- Tan, Longzhi, Dong Xing, Chi-Han Chang, Heng Li, and X. Sunney Xie. “[Three-Dimensional Genome Structures of Single Diploid Human Cells](https://doi.org/10.1126/science.aat5641).” Science, (31 2018) - **Dip-C** technology for single-cell Hi-C, multiplex end-tagging amplification (META). Detects ~5 times more contacts. Possible to make haplotype-separated Hi-C maps, detect CNVs, resolve X-chromosome inactivation. ~10kb-resolution Hi-C matrices, 3D genome reconstruction at 20kb resolution. PCA on chromatin compartments separates cell types. Comparison with Nagano data, bulk Gm12878 Hi-C. [Tools to analyze Dip-C (or other 3C/Hi-C) data](https://github.com/tanlongzhi/dip-c), [lh3/hickit](https://github.com/lh3/hickit). [Processed data: Gm12878, 17 cells, PBMC, 18 cells](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE117876).

- Lando, David, Tim J. Stevens, Srinjan Basu, and Ernest D. Laue. “[Calculation of 3D Genome Structures for Comparison of Chromosome Conformation Capture Experiments with Microscopy: An Evaluation of Single-Cell Hi-C Protocols](https://doi.org/10.1080/19491034.2018.1438799).” Nucleus, (January 1, 2018) - scHi-C protocol evaluation (Flyamer, Stevens, Nagano. Ramani). 100kb data. Practical observations, code for processing the data. [NucProcess](https://github.com/tjs23/nuc_processing) - Python toolkit for processing scHi-C-seq data. [NucDynamics](https://github.com/tjs23/nuc_dynamics) - calculating genome structures from scHi-C data, explanation of read alignment/filtering at restriction fragment resolution. 

- Flyamer, Ilya M., Johanna Gassler, Maxim Imakaev, Hugo B. Brandão, Sergey V. Ulianov, Nezar Abdennur, Sergey V. Razin, Leonid A. Mirny, and Kikuë Tachibana-Konwalski. “[Single-Nucleus Hi-C Reveals Unique Chromatin Reorganization at Oocyte-to-Zygote Transition](https://doi.org/10.1038/nature21711).” Nature, (06 2017) - **snHi-C** method, single-nucleus Hi-C that provides >10-fold more contacts per cell than the previous method. Omitted biotin incorporation and enrichment for ligated fragments steps. Applied to mouse oocyte-to-zygote transition, separately to maternal and paternal genomes (different patterns of chromatin reorganization, A/B compartments present in paternal nuclei only). Single cells have variable chromatin structure, but global patterns emerge when averaging. Changes in slope of the distance-dependent decay. TADs and loops may be generated by different mechanisms than compartments. Decrease in loop, TAD, and compartment strength during maturation. hiclib data processing, lavaburst for TAD identification. [Data, pooled sparse contact matrices (individual samples available under their own GSM accessions)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE80006)

- Nagano, Takashi, Yaniv Lubling, Tim J. Stevens, Stefan Schoenfelder, Eitan Yaffe, Wendy Dean, Ernest D. Laue, Amos Tanay, and Peter Fraser. “[Single-Cell Hi-C Reveals Cell-to-Cell Variability in Chromosome Structure](https://doi.org/10.1038/nature12593).” Nature, (October 3, 2013) - Single-cell Hi-C, protocol. 10 cells analyzed individually and as an ensemble. Large domains are stable, within-domain interactions are stochastic. Active marks correlate with enrichment of trans-chromosomal contacts. [Data: Mouse Th1 cells, 11 samples](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE48262). 

- Takashi Nagano et al., “[Cell-Cycle Dynamics of Chromosomal Organization at Single-Cell Resolution](https://doi.org/10.1038/nature23001),” Nature, (July 5, 2017) - Single-cell Hi-C, mouse embryonic stem cells, diploid and haploid, over cell cycle, 45 samples. [Data on GEO](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE94489) and [data on the pipeline page](https://bitbucket.org/tanaylab/schic2/overview)
    - Haploid single cell Hi-C processed counts from Nagano et al. were obtained from the Tanay lab, [schic_hap_2i_adj_files.tar.gz](http://compgenomics.weizmann.ac.il/files/archives/schic_hap_2i_adj_files.tar.gz) and [schic_hap_serum_adj_files.tar.gz](http://compgenomics.weizmann.ac.il/files/archives/schic_hap_serum_adj_files.tar.gz). Only cells with a total of 100,000 reads or more were used. Data were further filtered using HiFive single cell Hi-C filters. This involved removing fragment ends (fends) with no interactions, fends smaller than 21 bp or larger than 10 Kb, and all fends not originating from chromosomes 1 through 19 or X. Next, because only haploid cell data were used any fend with more than two interactions was removed and fends with exactly two interactions were removed if the interactions occurred with partner fends more than 40 fends apart; otherwise, the longer of the two interactions was kept. Finally, fends were partitioned into 1 Mb bins. (from Luperchio et al., “The Repressive Genome Compartment Is Established Early in the Cell Cycle before Forming the Lamina Associated Domains.”)
    - [scHiC 2.0: Sequence and analysis pipeline of single-cell Hi-C datasets](https://bitbucket.org/tanaylab/schic2/src/default/)

- Ramani, Vijay, Xinxian Deng, Ruolan Qiu, Kevin L. Gunderson, Frank J. Steemers, Christine M. Disteche, William S. Noble, Zhijun Duan, and Jay Shendure. “[Massively Multiplex Single-Cell Hi-C](https://doi.org/10.1038/nmeth.4155).” Nature Methods, (2017) - single-cell combinatorial indexed Hi-C protocol, exploratory data analysis, PCA, QC and filtering steps. Some experiments contained mixture of human and mouse cells. >10,000 cells. [Six data libraries, described in the Methods](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE84920)

- Stevens, Tim J., David Lando, Srinjan Basu, Liam P. Atkinson, Yang Cao, Steven F. Lee, Martin Leeb, et al. “[3D Structures of Individual Mammalian Genomes Studied by Single-Cell Hi-C](https://doi.org/10.1038/nature21429).” Nature, March 13, 2017 - 100kb five single-cell HiC. TADs are dynamic, A/B compartments, LADs, enhancers/promoters are consistent. 3D clustering of active histone marks, highly expressed genes. Co-expression of genes within TAD boundaries. Supplementary material has processing pipeline description, [TheLaueLab/nuc_processing](https://github.com/TheLaueLab/nuc_processing). [Videos](http://www.nature.com/nature/journal/v544/n7648/full/nature21429.html#supplementary-information)

- Ulianov, Sergey V., Kikue Tachibana-Konwalski, and Sergey V. Razin. “[Single-Cell Hi-C Bridges Microscopy and Genome-Wide Sequencing Approaches to Study 3D Chromatin Organization](https://doi.org/10.1002/bies.201700104).” BioEssays, (2017) - scRNA-seq, review of the technology and six papers that generated scHi-C data.

#### scHi-C multi-omics

- Li, Guoqiang, Yaping Liu, Yanxiao Zhang, Naoki Kubo, Miao Yu, Rongxin Fang, Manolis Kellis, and Bing Ren. “[Joint Profiling of DNA Methylation and Chromatin Architecture in Single Cells](https://doi.org/10.1038/s41592-019-0502-z).” Nature Methods, August 5, 2019 - **Methyl-HiC** - in situ Hi-C and WGBS. Comparable Hi-C matrices, TADs. 20% fewer CpGs overall, more CpGs in open chromatin. Proximal CpGs correlate irrespectively of loop anchors, weaker for inter-chromosomal interactions. Application to single-cell, mouse ESCs under different conditions. Relevant clustering, cluster-specific genes. Methods for wet-lab and computational processing. [Bulk (replicates) and single-cell Methyl-HiC data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119171). [Scripts](https://bitbucket.org/dnaase/bisulfitehic/src/master/), Bhmem pipeline to map bisulfite-converted reads, Juicer pipeline for processing, VC normalization, HiCRep at 1Mb matrix similarity.


