# Notes on single-cell Hi-C technologies, tools, and data

For a comprehensive overview of the subject, consider [other bioinformatics resources](https://github.com/mdozmorov/Bioinformatics_notes) and [collections of links to various resources](https://github.com/mdozmorov/MDmisc_notes). Issues with suggestions and pull requests are welcome!

# Table of content

* [Tools](#tools)
* [Papers and data](#papers-and-data)

- Li, Xiao, Ziyang An, and Zhihua Zhang. “Comparison of Computational Methods for 3D Genome Analysis at Single-Cell Hi-C Level.” Methods, August 2019, S1046202319300891. https://doi.org/10.1016/j.ymeth.2019.08.005. - Assessment of Hi-C methods applied to single-cell Hi-C data. Overview of computational analysis of Hi-C data (normalization, A/B compartment, TAD, loop calling, differential analysis), scRNA-seq data properties. Tested on systematically downsampled data and on experimental scHi-C data. HiCnorm is most performing for normalization, Insulation Score fastHiC for TAD/loop calling. A/B compartments are poorly defined in scHi-C data, TADs can be identified at single-cell level, aggregation improves TAD detection. Adjusted mutual information and weight similarity for TAD similarity assessment. Other methods, like TAD boundary prediction from epigenomic features.
    - [Table 1 - Methods for TAD identification](https://www.sciencedirect.com/science/article/pii/S1046202319300891?via%3Dihub#t0005)
    - [Table 2 - Methods for chromatin loop identification](https://www.sciencedirect.com/science/article/pii/S1046202319300891?via%3Dihub#t0005).


## Tools

- `scHiCExplorer` - Single cell Hi-C data analysis toolbox, Python, from processing to normalization, clustering, compartment identification, visualization. https://github.com/joachimwolff/scHiCExplorer

- `hickit` - TAD calling, phase imputation, 3D modeling and more for diploid single-cell Hi-C (Dip-C) and general Hi-C. https://github.com/lh3/hickit

- `dip-c` - Tools to analyze Dip-C (or other 3C/Hi-C) data - https://github.com/tanlongzhi/dip-c

- `HiCluster` - scHi-C clustering based on imputation using linear convolution and random walk. scHi-C challenges. Outperforms PCA, HiCrep. TAD-like structures can be detected in imputed data. Simulations, introducing noise, sparsity. https://github.com/zhoujt1994/scHiCluster
    - Zhou, Jingtian, Jianzhu Ma, Yusi Chen, Chuankai Cheng, Bokan Bao, Jian Peng, Terrence Sejnowski, Jesse Dixon, and Joseph Ecker. “HiCluster: A Robust Single-Cell Hi-C Clustering Method Based on Convolution and Random Walk.” Preprint. Bioinformatics, December 27, 2018. https://doi.org/10.1101/506717.

- `nuc_processing` - Chromatin contact paired-read single-cell Hi-C processing module for Nuc3D and NucTools. https://github.com/TheLaueLab/nuc_processing.
    - Stevens, Tim J., David Lando, Srinjan Basu, Liam P. Atkinson, Yang Cao, Steven F. Lee, Martin Leeb, et al. “3D Structures of Individual Mammalian Genomes Studied by Single-Cell Hi-C.” Nature, March 13, 2017. https://doi.org/10.1038/nature21429.

## Papers and data

- Tan, Longzhi, Dong Xing, Chi-Han Chang, Heng Li, and X. Sunney Xie. “Three-Dimensional Genome Structures of Single Diploid Human Cells.” Science (New York, N.Y.) 361, no. 6405 (31 2018): 924–28. https://doi.org/10.1126/science.aat5641. - Dip-C technology for single-cell Hi-C, multiplex end-tagging amplification (META). Detects ~5 times more contacts. Possible to make haplotype-separated Hi-C maps, detect CNVs, resolve X-chromosome inactivation. ~10kb-resolution Hi-C matrices, 3D genome reconstruction at 20kb resolution. PCA on chromatin compartments separates cell types. Comparison with Nagano data, bulk Gm12878 Hi-C. Tools to analyze Dip-C (or other 3C/Hi-C) data - https://github.com/tanlongzhi/dip-c, https://github.com/lh3/hickit. Processed data: Gm12878, 17 cells, PBMC, 18 cellshttps://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE117876.

- Lando, David, Tim J. Stevens, Srinjan Basu, and Ernest D. Laue. “Calculation of 3D Genome Structures for Comparison of Chromosome Conformation Capture Experiments with Microscopy: An Evaluation of Single-Cell Hi-C Protocols.” Nucleus (Austin, Tex.) 9, no. 1 (January 1, 2018): 190–201. https://doi.org/10.1080/19491034.2018.1438799. - scHi-C protocol evaluation (Flyamer, Stevens, Nagano. Ramani). 100kb data. Practical observations, code for processing the data. NucProcess - Python toolkit for processing scHi-C-seq data, https://github.com/tjs23/nuc_processing. NucDynamics - calculating genome structures from scHi-C data, explanation of read alignment/filtering at restriction fragment resolution, https://github.com/tjs23/nuc_dynamics. 

- Flyamer, Ilya M., Johanna Gassler, Maxim Imakaev, Hugo B. Brandão, Sergey V. Ulianov, Nezar Abdennur, Sergey V. Razin, Leonid A. Mirny, and Kikuë Tachibana-Konwalski. “Single-Nucleus Hi-C Reveals Unique Chromatin Reorganization at Oocyte-to-Zygote Transition.” Nature 544, no. 7648 (06 2017): 110–14. https://doi.org/10.1038/nature21711. - snHi-C method, single-nucleus Hi-C that provides >10-fold more contacts per cell than the previous method. Omitted biotin incorporation and enrichment for ligated fragments steps. Applied to mouse oocyte-to-zygote transition, separately to maternal and paternal genomes (different patterns of chromatin reorganization, A/B compartments present in paternal nuclei only). Single cells have variable chromatin structure, but global patterns emerge when averaging. Changes in slope of the distance-dependent decay. TADs and loops may be generated by different mechanisms than compartments. Decrease in loop, TAD, and compartment strength during maturation. hiclib data processing, lavaburst for TAD identification. Data, pooled sparse contact matrices (individual samples available under their own GSM accessions): https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE80006

- Nagano, Takashi, Yaniv Lubling, Tim J. Stevens, Stefan Schoenfelder, Eitan Yaffe, Wendy Dean, Ernest D. Laue, Amos Tanay, and Peter Fraser. “Single-Cell Hi-C Reveals Cell-to-Cell Variability in Chromosome Structure.” Nature 502, no. 7469 (October 3, 2013): 59–64. https://doi.org/10.1038/nature12593. - Single-cell Hi-C, protocol. 10 cells analyzed individually and as an ensemble. Large domains are stable, within-domain interactions are stochastic. Active marks correlate with enrichment of trans-chromosomal contacts. Mouse Th1 cells, 11 samples. https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE48262. 

- Takashi Nagano et al., “Cell-Cycle Dynamics of Chromosomal Organization at Single-Cell Resolution,” Nature 547, no. 7661 (July 5, 2017): 61–67, https://doi.org/10.1038/nature23001. Single-cell Hi-C, mouse embryonic stem cells, diploid and haploid, over cell cycle, 45 samples. GEO link https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE94489 and data on the pipeline page https://bitbucket.org/tanaylab/schic2/overview
    - Haploid single cell Hi-C processed counts from Nagano et al. were obtained from the Tanay lab (http://compgenomics.weizmann.ac.il/files/archives/schic_hap_2i_adj_files.tar.gz and http://compgenomics.weizmann.ac.il/files/archives/schic_hap_serum_adj_files.tar.gz). Only cells with a total of 100,000 reads or more were used. Data were further filtered using HiFive single cell Hi-C filters. This involved removing fragment ends (fends) with no interactions, fends smaller than 21 bp or larger than 10 Kb, and all fends not originating from chromosomes 1 through 19 or X. Next, because only haploid cell data were used any fend with more than two interactions was removed and fends with exactly two interactions were removed if the interactions occurred with partner fends more than 40 fends apart; otherwise, the longer of the two interactions was kept. Finally, fends were partitioned into 1 Mb bins. (from Luperchio et al., “The Repressive Genome Compartment Is Established Early in the Cell Cycle before Forming the Lamina Associated Domains.”)
    - scHiC 2.0: Sequence and analysis pipeline of single-cell Hi-C datasets. https://bitbucket.org/tanaylab/schic2/src/default/

- Ramani, Vijay, Xinxian Deng, Ruolan Qiu, Kevin L. Gunderson, Frank J. Steemers, Christine M. Disteche, William S. Noble, Zhijun Duan, and Jay Shendure. “Massively Multiplex Single-Cell Hi-C.” Nature Methods 14, no. 3 (2017): 263–66. https://doi.org/10.1038/nmeth.4155. - single-cell combinatorial indexed Hi-C protocol, exploratory data analysis, PCA, QC and filtering steps. Some experiments contained mixture of human and mouse cells. >10,000 cells. Six data libraries, described in the Methods. https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE84920

- Stevens, Tim J., David Lando, Srinjan Basu, Liam P. Atkinson, Yang Cao, Steven F. Lee, Martin Leeb, et al. “3D Structures of Individual Mammalian Genomes Studied by Single-Cell Hi-C.” Nature, March 13, 2017. https://doi.org/10.1038/nature21429. - 100kb five single-cell HiC. TADs are dynamic, A/B compartments, LADs, enhancers/promoters are consistent. 3D clustering of active histone marks, highly expressed genes. Co-expression of genes within TAD boundaries. Supplementary material has processing pipeline description, https://github.com/TheLaueLab/nuc_processing. Videos at http://www.nature.com/nature/journal/v544/n7648/full/nature21429.html#supplementary-information

- Ulianov, Sergey V., Kikue Tachibana-Konwalski, and Sergey V. Razin. “Single-Cell Hi-C Bridges Microscopy and Genome-Wide Sequencing Approaches to Study 3D Chromatin Organization.” BioEssays: News and Reviews in Molecular, Cellular and Developmental Biology 39, no. 10 (2017). https://doi.org/10.1002/bies.201700104. - scRNA-seq, review of the technology and six papers that generated scHi-C data.


