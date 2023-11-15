# Notes on single-cell Hi-C technologies, tools, and data

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![PR's Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com) 

Single-cell 3D genomics notes. Please, [contribute and get in touch](CONTRIBUTING.md)! See [MDmisc notes](https://github.com/mdozmorov/MDmisc_notes) for other programming and genomics-related notes.

# Table of content

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Tools](#tools)
  - [Normalization](#normalization)
  - [Clustering](#clustering)
  - [3D modeling](#3d-modeling)
  - [Simulation](#simulation)
- [Papers](#papers)
  - [Clustering, embedding](#clustering-embedding)
  - [Technologies, data](#technologies-data)
    - [scHi-C multi-omics](#schi-c-multi-omics)
    - [Imaging](#imaging)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Tools

- [SnapHiC](https://github.com/HuMingLab/SnapHiC) - scHi-C analysis pipeline. Identifies chromatin loops at 10kb resolution. Imputes contact probability with the random walk with restart algorithm (scHiCluster method, considering the effective fragment size, GC content, mappability, details in Methods), distance-normalizes, applies the paired t-test using global and local background to identify loop candidates, groups the loop candidates using the Rodriguez and Lailo's algorithm, and identifies summits within each cluster. Considers global and local background to filter out false positives. Tested on 742 mouse embryonic stem cells, sn-methyl-3C-seq data from 2,869 human prefrontal cortical cells. Compared with HiCCUPS, discovers 4-70 times more cell-type-specific loops, achieves better F1, peak enrichment in APA analysis, CTCF convergent orientation, also detects known long-range interactions. Linking putative target genes and non-coding sequence variants associated with neuropsychiatric disorders. Ground truth for benchmarking: HiCCUPS loops plus long-range interadtions from PLAC-seq and HiChIP experiments from mESCs (MAPS pipeline). Compared with Hi-C-FastHiC, FitHiC2, HiC-ACT, on downsampled data. <details>
    <summary>Paper</summary>
    Yu, Miao, Armen Abnousi, Yanxiao Zhang, Guoqiang Li, Lindsay Lee, Ziyin Chen, Rongxin Fang, et al. “SnapHiC: A Computational Pipeline to Identify Chromatin Loops from Single-Cell Hi-C Data.” Nature Methods, August 26, 2021. https://doi.org/10.1038/s41592-021-01231-2.

    Li, Xiaoqi, Lindsay Lee, Armen Abnousi, Miao Yu, Weifang Liu, Le Huang, Yun Li, and Ming Hu. “SnapHiC2: A Computationally Efficient Loop Caller for Single Cell Hi-C Data.” Computational and Structural Biotechnology Journal 20 (2022): 2778–83. https://doi.org/10.1016/j.csbj.2022.05.046. - SnapHiC2, fast reimplementation using a sliding window approach for random walk with restart. Enables data processing at 5kb resolution.
</details>

- [scHiCExplorer](https://github.com/joachimwolff/scHiCExplorer) - Single cell Hi-C data analysis toolbox, Python, from processing to normalization, clustering, compartment identification, visualization.

- [lh3/hickit](https://github.com/lh3/hickit) - TAD calling, phase imputation, 3D modeling and more for diploid single-cell Hi-C (Dip-C) and general Hi-C. See [https://doi.org/10.1016/j.cell.2020.12.032](https://doi.org/10.1016/j.cell.2020.12.032)

- [tanlongzhi/dip-c](https://github.com/tanlongzhi/dip-c) - Tools to analyze Dip-C (or other 3C/Hi-C) data. See [https://doi.org/10.1126/science.aat5641](https://doi.org/10.1126/science.aat5641)

- [nuc_processing](https://github.com/TheLaueLab/nuc_processing) - Chromatin contact paired-read single-cell Hi-C processing module for Nuc3D and NucTools. See [https://doi.org/10.1038/nature21429](https://doi.org/10.1038/nature21429)

## Normalization

- [scHiCNorm](http://dna.cs.miami.edu/scHiCNorm/) - scHi-C normalization using regression against known biases (cutting site density, mappability, CG) using six distributions. Filter out cells with less than 50,000 uniquely mapped reads, merge cells, 1Mb resolution. Ramani 2017 data, 74 matrices. Correlations are assumed to be driven by biases, and decrease in between-dataset correlation and increase in variability is judged as good. 
    - Liu, Tong, and Zheng Wang. “[ScHiCNorm: A Software Package to Eliminate Systematic Biases in Single-Cell Hi-C Data](https://doi.org/10.1093/bioinformatics/btx747).” Bioinformatics, (March 15, 2018)

### Clustering

- [BandNorm](https://github.com/keleslab/BandNorm) and [3DVI](https://github.com/keleslab/3DVI) methods for normalizing and denoising single-cell Hi-C data. BandNorm - an R package, distance-centric band normalization approach, improves cell clustering. 3DVI - deep generative modeling framework using Poisson and Negative Binomial distributions to model scHi-C counts, accounting for library size and batch effect for each band matrix, learns low-dimensional representation of scHi-C data, denoises and enables 3D compartment identification (uses scvi-tools). Compared against library size scaling methods (global CellScale and local BandScale), and scHiCluster, scHiC Topics, Higashi (Table 1 - overview of 8 methods total). Evaluated clustering ARI/silhouette on Ramani2017, Kim2020, Lee2019, and Li2019 scHi-C datasets, 1Mb resolution (Supplementary Table 1). Differential TAD boundaries detection evaluated on TADcompare, diffHiC, CHESS using concordance with bulk data. [Tweet](https://twitter.com/yezhengSTAT/status/1370226715293708290?s=20)
    - Zheng, Ye, Siqi Shen, and Sunduz Keles. “[Normalization and De-Noising of Single-Cell Hi-C Data with BandNorm and 3DVI](https://doi.org/10.1101/2021.03.10.434870).” Preprint. Bioinformatics, March 11, 2021

- [Fast-Higashi](https://github.com/ma-compbio/Fast-Higashi) - scHi-C analysis for precise single cell clustering (rare cell type identification), trajectory inference, differential contact analysis (meta-interactions). Models scHi-C data using tensor decomposition ([PARAFAC2](https://doi.org/10.1016/j.chemolab.2020.104127) joint factorization algorithm, decomposes chromosome-specific 3-way tensors into four factors, Figure 1, Methods). Partial random walk with restart to impute the data. Applied to three 500kb scHi-C datasets ([Tan et al. 2021](https://doi.org/10.1016/j.cell.2020.12.032), [:Liu et al. 2021](https://doi.org/10.1038/s41586-020-03182-8), [Lee et al. 2019](https://doi.org/10.1038/s41592-019-0547-z), and more). Compared with 3DVI, scHiCluster, Higashi (modularity score, ARI, adjusted mutual information, F1 scores), improves detection of rare cell types, trajectories, cell type-specific connections, aggregated A/B compartment analysis. Fast. Can initialize Higashi for better performance. Python/Pytorch code on [Zenodo](https://zenodo.org/record/7023632). <details>
    <summary>Paper</summary>
    Zhang, Ruochi, Tianming Zhou, and Jian Ma. “Ultrafast and Interpretable Single-Cell 3D Genome Analysis with Fast-Higashi.” Cell Systems 13, no. 10 (October 2022): 798-807.e6. https://doi.org/10.1016/j.cels.2022.09.004.
</details>

- [Higashi](https://github.com/ma-compbio/Higashi) - hypergraph representation learning for scHi-C embedding (learning node embedding of the hypergraph) and imputation (predicting missing hyperedges within the hypergraph). Whole scHi-C dataset as a hypergraph, with cell nodes and genomic bin nodes. Uses Hyper-SAGNN architecture. Imputation by borrowing information from k-nearest neighbors in the embedding space. Detects TAD-like structures. Applied to 4D Nucleome, Ramani, Nagano scHi-C data. Outperforms HiCCRep/MDS, scHiCluster. LDA in imputation, cell clustering. Can incorporate other omics modalities, and shown improved performance on single-nucleus methyl-3C (sn-m3C-seq) scHi-C and methylation data in human prefrontal cortex cells. Robust to downsampling. A/B compartments detection improved after imputation. Improved detection of TAD-like structures using insulation scores, genes associated with variable boundaries. Methods and supplementary detail network structure, input as triplets of attributes of one cell node and two genomic bin nodes, loss function, training.
    - Zhang, Ruochi, Tianming Zhou, and Jian Ma. “[Multiscale and Integrative Single-Cell Hi-C Analysis with Higashi](https://doi.org/10.1101/2020.12.13.422537).” Preprint. Bioinformatics, December 15, 2020.

- [Hyper-SAGNN](https://github.com/ma-compbio/Hyper-SAGNN) - self-attention based graph neural network applicable to homogeneous and heterogeneous hypergraphs. Applied to scHi-C Ramani and Nagano data. Compared with DeepWalk, LINE, and HEBE. Not compared with hyper2vec and node2vec. Outperforms HiCrep+MDS and scHiCluster in measuring scHi-C similarity. Demo of other applications.
- Zhang, Ruochi, Yuesong Zou, and Jian Ma. "[Hyper-SAGNN: a self-attention based graph neural network for hypergraphs](https://arxiv.org/abs/1911.02613)." arXiv preprint (November 6, 2019).

- [scHiCTools](https://github.com/liu-bioinfo-lab/scHiCTools) - scHiCTools - a set of tools for high-level analysis (clustering) of scHi-C data. Project single cells in a lower-dimensional Euclidean space. Three methods for smoothing scHi-C data (linear convolution, random walk, network enhancing), three projection methods (fastHiCRep, Selfish, newly developed InnerProduct), three embedding methods if assuming cells come from a continuous manifold (MDS, t-SNE, PHATE), or three clustering methods if assuming cells are from different clusters (k-means, spectral clustering, HiCluster). Brief Methods of each approach. Tested on Nagano 2017 cell-cycle dataset. InnerProduct captures cell similarity well, any embedding works good, linear convolution and random walk improve projections at high dropout rates. QC plots. ACROC - area under the curve of a circular ROC calculation.  Input - text matrices. Python 3.
    - Li, Xinjun, Fan Feng, Wai Yan Leung, and Jie Liu. “[ScHiCTools: A Computational Toolbox for Analyzing Single-Cell Hi-C Data](https://doi.org/10.1101/769513).” Preprint. Bioinformatics, September 18, 2019. 

- [scHiCluster](https://github.com/zhoujt1994/scHiCluster) - single-cell Hi-C clustering algorithm based on imputation using linear convolution (neighborhood smoothing within a window of size 1 over 1Mb scHi-C matrices) and random walk with restarts. scHi-C challenges: variability, sparsity, coverage heterogeneity. Two-step imputation to resolve sparsity, top-ranked interactions after imputation to resolve heterogeneity. Tested on simulated (from bulk Hi-C controlling for sparsity, and pseudobulk) and experimental (Ramani, four human cell lines; Flyamer, mouse zygotes and oocytes; Nagano) scHi-C data. Against PCA, HiCRep+MDS, the eigenvector method, the decay profile method. Adjusted Rand Index to test clustering quality. TAD-like structures can be detected in imputed data (TopDom). At least 5k contacts per cell is sufficient. Python package. Input - sparse matrices, 1Mb resolution, or juicer-pre format for custom resolution.
    - Zhou, Jingtian, Jianzhu Ma, Yusi Chen, Chuankai Cheng, Bokan Bao, Jian Peng, Terrence J. Sejnowski, Jesse R. Dixon, and Joseph R. Ecker. “[Robust Single-Cell Hi-C Clustering by Convolution- and Random-Walk–Based Imputation](https://doi.org/10.1073/pnas.1901423116).” Proceedings of the National Academy of Sciences, (July 9, 2019)

### TAD calling

- [DeDoc2](https://github.com/zengguangjie/deDoc2) - scHi-C hierarchical TAD caller. Two variants, deDoc2.w and deDoc2.s, to predict higher and lower level TLDs. Minimize structural entropy of the whole chromosome of sliding window. Benchmarked on downsampled, simulated, and experimental scHi-C data, against Higashi, scHiCluster, deTOKI, SpectralTAD, deDoc, GRiNCH, Insulation Score. Robust to noise, no need for data imputation. <details>
    <summary>Paper</summary>
    Li, Angsheng, Guangjie Zeng, Haoyu Wang, Xiao Li, and Zhihua Zhang. “DeDoc2 Identifies and Characterizes the Hierarchy and Dynamics of Chromatin TAD-Like Domains in the Single Cells.” Advanced Science (Weinheim, Baden-Wurttemberg, Germany) 10, no. 20 (July 2023): e2300366. https://doi.org/10.1002/advs.202300366.
</details>

### 3D modeling

- [DPDchrom](https://github.com/polly-code/DPDchrom) - reconstruction of the 3D chromatin conformation from single-cell Hi-C data. Relies on dissipative particle dynamics (DPD). Incorporates expectation whether the conformation should be coil-like or globular (at the resolution of 10kb and lower). Explicitly accounts for solvent. Compared with the Stevens method, classical molecular dynamics (CMD) method. Benchmarked on artificial polymer models, DPDchrom performs better at low contact density (up to 95% accuracy). On experimental data - up to 65% accuracy. Propose the Modified Jaccard Index (Methds) to compare 3D structures irrespectively of spatial orientation and scale. Many practical aspects and parameters affecting reconstruction accuracy, data sparsity exponentially affects accuracy. [S2 Table](https://doi.org/10.1371/journal.pcbi.1009546.s010) - list of single nucleus Hi-C datasets, [S1 Appendix](https://doi.org/10.1371/journal.pcbi.1009546.s011) - Details of simulation methods and analysis, ORBITA protocol for snHi-C. [Tweet by Pavel Kos](https://twitter.com/PavelKos7/status/1461639123505135617?s=20)
    - Kos, Pavel I., Aleksandra A. Galitsyna, Sergey V. Ulianov, Mikhail S. Gelfand, Sergey V. Razin, and Alexander V. Chertovich. “[Perspectives for the Reconstruction of 3D Chromatin Conformation Using Single Cell Hi-C Data](https://doi.org/10.1371/journal.pcbi.1009546).” PLOS Computational Biology, (November 18, 2021)

### Simulation

- [scHi-CSim](https://github.com/zhanglabtools/scHi-CSim) - a single-cell Hi-C simulator (Python), estimates statistical properties from experimental data and generate simulated data closely resembling experimental (cell type information, biological functions, enhancer-promoter interactions, loops, their statistical significance). Used for clustering benchmarking. <details>
    <summary>Paper</summary>
    Fan, Shichen, Dachang Dang, Yusen Ye, Shao-Wu Zhang, Lin Gao, and Shihua Zhang. “scHi-CSim: A Flexible Simulator That Generates High-Fidelity Single-Cell Hi-C Data for Benchmarking.” Edited by Luonan Chen. Journal of Molecular Cell Biology 15, no. 1 (June 1, 2023): mjad003. https://doi.org/10.1093/jmcb/mjad003.
</details>

## Papers

- Galitsyna, Aleksandra A, and Mikhail S Gelfand. “[Single-Cell Hi-C Data Analysis: Safety in Numbers](https://doi.org/10.1093/bib/bbab316).” Briefings in Bioinformatics, August 18, 2021
    - Single-cell Hi-C review, technology overview, analysis steps, challenges, tools. Mapping (split-read alignment, iterative mapping, read clipping, ORBITA), filtering spurious contacts, cells. Analysis, from 3D structure reconstruction, imputation, embedding, to clustering, pseudobulk analysis and AB compartments/TADs calling, deconvolution.

Zhou, Tianming, Ruochi Zhang, and Jian Ma. “[The 3D Genome Structure of Single Cells](https://doi.org/10.1146/annurev-biodatasci-020121-084709).” Annual Review of Biomedical Data Science, (July 20, 2021)
    - Review of scHi-C technologies, computational methods.Table 1 - technologies (proximity ligation-based (e.g., sci-Hi-C, Dip-C), ligation-free (e.g., scSPRITE, ChIA-Drop), imaging-based (e.g., Oligopaint, OligoFISSEQ, hiFISH, HIPMap)), number of cells, depth. Data processing (demultiplexing, alignment, binning, filtering, storage, tool - scHiCExplorer), dimensionality reduction (HiCRep + MDS, scHiCluster, hypergraph-based Higashi + Hyper-SAGNN), imputation (scHiCluster, Higashi), Challenges in 3D structure modeling, sompartment annotation, domain/loop identification. Multi-way interaction analysis methods (MIA-Sig, MATCHA).

- Li, Xiao, Ziyang An, and Zhihua Zhang. “[Comparison of Computational Methods for 3D Genome Analysis at Single-Cell Hi-C Level](https://doi.org/10.1016/j.ymeth.2019.08.005).” Methods, August 2019 - Assessment of Hi-C methods applied to single-cell Hi-C data. Overview of computational analysis of Hi-C data (normalization, A/B compartment, TAD, loop calling, differential analysis), scRNA-seq data properties. Tested on systematically downsampled data and on experimental scHi-C data. HiCnorm is most performing for normalization, Insulation Score fastHiC for TAD/loop calling. A/B compartments are poorly defined in scHi-C data, TADs can be identified at single-cell level, aggregation improves TAD detection. Adjusted mutual information and weight similarity for TAD similarity assessment. Other methods, like TAD boundary prediction from epigenomic features.
    - [Table 1 - Methods for TAD identification](https://www.sciencedirect.com/science/article/pii/S1046202319300891?via%3Dihub#t0005)
    - [Table 2 - Methods for chromatin loop identification](https://www.sciencedirect.com/science/article/pii/S1046202319300891?via%3Dihub#t0005).

### Clustering, embedding

- Kim, Hyeon-Jin, Galip Gürkan Yardımcı, Giancarlo Bonora, Vijay Ramani, Jie Liu, Ruolan Qiu, Choli Lee, et al. “[Capturing Cell Type-Specific Chromatin Compartment Patterns by Applying Topic Modeling to Single-Cell Hi-C Data](https://doi.org/10.1371/journal.pcbi.1008173).” PLOS Computational Biology, (September 18, 2020) - Topic modeling (Latent Dirichlet allocation, LDA) on sciHi-C data. 4D Nucleome datasets, 500kb resolution, newly generated data from GM12878, H1ESC, HFF IMR90, HAP1 cells, >19,000 cells. Preprocessing and converting 500Mb scHi-C matrices to locus-pairs (LPs), then tSNE. Cell-topics, LP-topics representation. Topics can capture A/B compartments. LDA using the cisTopic package, procedure for selecting the number of topics. Comparison with scHiCluster, similar perfrormance.

- Liu, Jie, Dejun Lin, Galip Gürkan Yardimci, and William Stafford Noble. “[Unsupervised Embedding of Single-Cell Hi-C Data](https://doi.org/10.1093/bioinformatics/bty285).” Bioinformatics, (July 1, 2018) - Embedding of scHi-C data. HiCRep with MDS performs best. Contact Probability Function as a means to compare Hi-C matrices. Methods for evaluating reproducibility also can be used to compare matrices, details of HiCRep, GenomeDISCO, HiC-Spector methods. Description of scHi-C datasets and their arrangement by cell cycle stage. 5K total reads per scHi-C matrix is sufficient for proper embedding.


### Technologies, data

- [scSPRITE](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154353) - single-cell split-pool recognition of interactions by tag extension technology. Multi-way interactions. Applied to mESCs and detected chromosome territories, A/B compartments, TADs (heterogeneous), long-range interactions organized around various nuclear bodies. [GEO GSE154353](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154353) - processed matrices. [GitHub](https://github.com/caltech-bioinformatics-resource-center/Guttman_Ismagilov_Labs) - Code 
    - Arrastia, Mary V., Joanna W. Jachowicz, Noah Ollikainen, Matthew S. Curtis, Charlotte Lai, Sofia A. Quinodoz, David A. Selck, Rustem F. Ismagilov, and Mitchell Guttman. "[Single-cell measurement of higher-order 3D genome organization with scSPRITE](https://doi.org/10.1038/s41587-021-00998-1)." Nature Biotechnology (2021): 1-10.

- Single-nucleus Hi-C data (scHi-C) of 88 Drosophila BG3 cells. 2-5M paired-end reads per cell, 10kb resolution. ORBITA pipeline to eliminate the effect of Phi29 DNA polymerase template switching. Chromatin compartments approx. 1Mb in size, non-hierarchical conserved TADs can be detected. Lots of biology, integration with other omics data. Raw and processed data in .cool format at [GEO GSE131811](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131811)
    - Ulianov, Sergey V., Vlada V. Zakharova, Aleksandra A. Galitsyna, Pavel I. Kos, Kirill E. Polovnikov, Ilya M. Flyamer, Elena A. Mikhaleva, et al. “[Order and Stochasticity in the Folding of Individual Drosophila Genomes](https://doi.org/10.1038/s41467-020-20292-z).” Nature Communications 12, no. 1 (December 2021)

- Four scHi-C datasets (Kim2020, Li2019, Ramani2017, and Lee2019) can be downloaded using the [BandNorm](https://github.com/keleslab/BandNorm) package's function [download_schic](https://sshen82.github.io/BandNorm/reference/download_schic.html). Alternatively, [direct download](http://pages.stat.wisc.edu/~sshen82/bandnorm/) 

- Tan, Longzhi, Wenping Ma, Honggui Wu, Yinghui Zheng, Dong Xing, Ritchie Chen, Xiang Li, Nicholas Daley, Karl Deisseroth, and X. Sunney Xie. “[Changes in Genome Architecture and Transcriptional Dynamics Progress Independently of Sensory Experience during Post-Natal Brain Development](https://doi.org/10.1016/j.cell.2020.12.032).” Cell, January 2021 <span data-badge-type="4" data-doi="10.1016/j.cell.2020.12.032" data-hide-no-mentions="true" class="altmetric-embed"> </span> - Single-cell 3D genomic (3646 cells) and transcriptomic (3517 cells) data of developing (7 time points post-natal) mouse brain (cortex and hippocampus). Dip-C - diploid chromatin conformation capture, MALBAC-DT - multiple annealing and looping-based amplification cycles for digital transcriptomics. 3D genome separates cells into 13 structure types. Three main lineages: neurons, astrocytes, oligodendrocytes. Correlating gene cell type-specific expression with A/B compartmentalization helps to integrate gene expression and the 3D structure. WGCNA modules of correlated genes, dynamics of those modules across time. Integration with published 3D genome, transcriptome, methylome, and chromatin accessibility data. A major 3D genome transformation between P7 (postnatal day 7) and P28 days, genetics determines 3D structure irrespectively of sensory stimuli (visual cortex in dark-reared animals), many more in the text. Data processed with [dip-c - tools to analyze Dip-C data, with documentation](https://github.com/tanlongzhi/dip-c) and [hickit - TAD calling, phase imputation, 3D modeling and more for diploid single-cell Hi-C (Dip-C) and general Hi-C](https://github.com/lh3/hickit). [Processed MALBAC-DT and Dip-C data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE162511). References to other published data in the "Key resourses" table

- Arrastia, Mary V., Joanna W. Jachowicz, Noah Ollikainen, Matthew S. Curtis, Charlotte Lai, Sofia Quinodoz, David A. Selck, Mitchell Guttman, and Rustem F. Ismagilov. “[A Single-Cell Method to Map Higher-Order 3D Genome Organization in Thousands of Individual Cells Reveals Structural Heterogeneity in Mouse ES Cells](https://doi.org/10.1101/2020.08.11.242081).” Preprint. Molecular Biology, August 12, 2020. - **scSPRITE** - Single-cell split-pool recognition of interactions by tag extension.Two triple-sets of split-pool barcoding, nuclear and spatial, DNA phosphate modified (DMP), odd, and even tagging. Paired-end sequencing, read 1 has genomic DNA and the DMP tag, read 2 has other 5 tags. Detects chromatin structures at all scales. Applied to >1000 mESC nuclei. Highly correlate with bulk SPRITE data. Captures multi-way interactions. Captures >30-fold more contacts than scHi-C with <10-fold reads. Processed single-cell matrices, ensemble, https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE154353. [A Snakemake workflow for data processing](https://github.com/caltech-bioinformatics-resource-center/Guttman_Ismagilov_Labs)

- Tan, Longzhi, Dong Xing, Chi-Han Chang, Heng Li, and X. Sunney Xie. “[Three-Dimensional Genome Structures of Single Diploid Human Cells](https://doi.org/10.1126/science.aat5641).” Science, (31 2018) - **Dip-C** technology for single-cell Hi-C, multiplex end-tagging amplification (META). Detects ~5 times more contacts. Possible to make haplotype-separated Hi-C maps, detect CNVs, resolve X-chromosome inactivation. ~10kb-resolution Hi-C matrices, 3D genome reconstruction at 20kb resolution. PCA on chromatin compartments separates cell types. Comparison with Nagano data, bulk Gm12878 Hi-C. [Tools to analyze Dip-C (or other 3C/Hi-C) data](https://github.com/tanlongzhi/dip-c), [lh3/hickit](https://github.com/lh3/hickit). [Processed data: Gm12878, 17 cells, PBMC, 18 cells](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE117876).

- Lando, David, Tim J. Stevens, Srinjan Basu, and Ernest D. Laue. “[Calculation of 3D Genome Structures for Comparison of Chromosome Conformation Capture Experiments with Microscopy: An Evaluation of Single-Cell Hi-C Protocols](https://doi.org/10.1080/19491034.2018.1438799).” Nucleus, (January 1, 2018) - scHi-C protocol evaluation (Flyamer, Stevens, Nagano. Ramani). 100kb data. Practical observations, code for processing the data. [NucProcess](https://github.com/tjs23/nuc_processing) - Python toolkit for processing scHi-C-seq data. [NucDynamics](https://github.com/tjs23/nuc_dynamics) - calculating genome structures from scHi-C data, explanation of read alignment/filtering at restriction fragment resolution. 

- Flyamer, Ilya M., Johanna Gassler, Maxim Imakaev, Hugo B. Brandão, Sergey V. Ulianov, Nezar Abdennur, Sergey V. Razin, Leonid A. Mirny, and Kikuë Tachibana-Konwalski. “[Single-Nucleus Hi-C Reveals Unique Chromatin Reorganization at Oocyte-to-Zygote Transition](https://doi.org/10.1038/nature21711).” Nature, (06 2017) - **snHi-C** method, single-nucleus Hi-C that provides >10-fold more contacts per cell than the previous method. Omitted biotin incorporation and enrichment for ligated fragments steps. Applied to mouse oocyte-to-zygote transition, separately to maternal and paternal genomes (different patterns of chromatin reorganization, A/B compartments present in paternal nuclei only). Single cells have variable chromatin structure, but global patterns emerge when averaging. Changes in slope of the distance-dependent decay. TADs and loops may be generated by different mechanisms than compartments. Decrease in loop, TAD, and compartment strength during maturation. hiclib data processing, lavaburst for TAD identification. [Data, pooled sparse contact matrices (individual samples available under their own GSM accessions)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE80006)

- Nagano, Takashi, Yaniv Lubling, Tim J. Stevens, Stefan Schoenfelder, Eitan Yaffe, Wendy Dean, Ernest D. Laue, Amos Tanay, and Peter Fraser. “[Single-Cell Hi-C Reveals Cell-to-Cell Variability in Chromosome Structure](https://doi.org/10.1038/nature12593).” Nature, (October 3, 2013) - Single-cell Hi-C, protocol. 10 cells analyzed individually and as an ensemble. Large domains are stable, within-domain interactions are stochastic. Active marks correlate with enrichment of trans-chromosomal contacts. [Data: Mouse Th1 cells, 11 samples](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE48262). 

- Takashi Nagano et al., “[Cell-Cycle Dynamics of Chromosomal Organization at Single-Cell Resolution](https://doi.org/10.1038/nature23001),” Nature, (July 5, 2017) - Single-cell Hi-C, mouse embryonic stem cells, diploid and haploid, over cell cycle, 45 samples. [Data on GEO](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE94489) and [data on the pipeline page](https://bitbucket.org/tanaylab/schic2/overview)
    - Haploid single cell Hi-C processed counts from Nagano et al. were obtained from the Tanay lab, [schic_hap_2i_adj_files.tar.gz](http://compgenomics.weizmann.ac.il/files/archives/schic_hap_2i_adj_files.tar.gz) and [schic_hap_serum_adj_files.tar.gz](http://compgenomics.weizmann.ac.il/files/archives/schic_hap_serum_adj_files.tar.gz). Only cells with a total of 100,000 reads or more were used. Data were further filtered using HiFive single cell Hi-C filters. This involved removing fragment ends (fends) with no interactions, fends smaller than 21 bp or larger than 10 Kb, and all fends not originating from chromosomes 1 through 19 or X. Next, because only haploid cell data were used any fend with more than two interactions was removed and fends with exactly two interactions were removed if the interactions occurred with partner fends more than 40 fends apart; otherwise, the longer of the two interactions was kept. Finally, fends were partitioned into 1 Mb bins. (from Luperchio et al., “The Repressive Genome Compartment Is Established Early in the Cell Cycle before Forming the Lamina Associated Domains.”)
    - [scHiC 2.0: Sequence and analysis pipeline of single-cell Hi-C datasets](https://bitbucket.org/tanaylab/schic2/src/default/)

- Ramani, Vijay, Xinxian Deng, Ruolan Qiu, Kevin L. Gunderson, Frank J. Steemers, Christine M. Disteche, William S. Noble, Zhijun Duan, and Jay Shendure. “[Massively Multiplex Single-Cell Hi-C](https://doi.org/10.1038/nmeth.4155).” Nature Methods, (2017) - single-cell combinatorial indexed Hi-C protocol, exploratory data analysis, PCA, QC and filtering steps. Some experiments contained mixture of human and mouse cells. Four human cell lines, HeLa S3, HAP1, K562, and GM12878. These cell lines are distributed over five sequencing libraries labeled as ml1, ml2, ml3, pl1, pl2, where pairs ml1 and ml2, and pl1 and pl2 are sequencing experiments with the same library preparations, respectively, and hence present different batches. >10,000 cells. [GEO GSE84920](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE84920)

- Stevens, Tim J., David Lando, Srinjan Basu, Liam P. Atkinson, Yang Cao, Steven F. Lee, Martin Leeb, et al. “[3D Structures of Individual Mammalian Genomes Studied by Single-Cell Hi-C](https://doi.org/10.1038/nature21429).” Nature, March 13, 2017 - 100kb five single-cell HiC. TADs are dynamic, A/B compartments, LADs, enhancers/promoters are consistent. 3D clustering of active histone marks, highly expressed genes. Co-expression of genes within TAD boundaries. Supplementary material has processing pipeline description, [TheLaueLab/nuc_processing](https://github.com/TheLaueLab/nuc_processing). [Videos](http://www.nature.com/nature/journal/v544/n7648/full/nature21429.html#supplementary-information)

- Ulianov, Sergey V., Kikue Tachibana-Konwalski, and Sergey V. Razin. “[Single-Cell Hi-C Bridges Microscopy and Genome-Wide Sequencing Approaches to Study 3D Chromatin Organization](https://doi.org/10.1002/bies.201700104).” BioEssays, (2017) - scRNA-seq, review of the technology and six papers that generated scHi-C data.

#### scHi-C multi-omics

- HiRES technology, Hi-C and RNA-seq employed simultaneously. Single-cell Hi-C and RNA-seq profiling from the same cells. Single-cell 3D structures depend on cell cycle but also diverge in cell type-specific manner. Interactions between B compartments increase during development. 3D changes occur before transcriptional changes. Brain cells and developing mouse embryos, between day 7 (E7.0) and E11.5. 20kb resolution, agrees with Dip-C. SimpleDiff pipeline for differential chromatin interaction analysis (Wilcoxon on distance-specific Z-score-transformed contacts between groups of cells), excitatory vs. inhibitory adult mouse brain neuron analysis. [GSE223917](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE223917) - processed data, [description](GSE223917/README.md). Processing [Scripts](https://zenodo.org/records/7691903), Python, R, command line. <details>
    <summary>Paper</summary>
    Liu, Zhiyuan, Yujie Chen, Qimin Xia, Menghan Liu, Heming Xu, Yi Chi, Yujing Deng, and Dong Xing. “Linking Genome Structures to Functions by Simultaneous Single-Cell Hi-C and RNA-Seq.” Science 380, no. 6649 (June 9, 2023): 1070–76. https://doi.org/10.1126/science.adg3797.
</details>

- [Single-cell DNA methylation (snmC-seq3) and 3D genome architecture (snm3C-seq) in the human brain](http://neomorph.salk.edu/hba/). Additional snRNA-seq and snATAC-seq. 517K cells from 46 regions of three adult male brains. Epigenome-based classification of brain cell types, comparison of neurons and non-neurons in terms of loops, domains, compartments, differentially expressed genes, methylation patterns, association among modalities. Data at [GEO GSE215353](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM2153253), at [Brain initiative](https://cellxgene.cziscience.com/collections/fdebfda9-bb9a-4b4b-97e5-651097ea07b0) (download and visualize). Tools: [scHiCcluster](https://zhoujt1994.github.io/scHiCluster/hba/intro.html), [GitHub1](https://github.com/lhqing/cemba_data), [GitHub2](https://github.com/lhqing/ALLCools), [GitHub3](https://github.com/jksr/human-brain-atlas-code). [Supplementary tables](http://neomorph.salk.edu/hba/tables/) include differential loops between major cell types, candidate cis-regulatory elements. <details>
    <summary>Paper</summary>
    Tian, Wei, Jingtian Zhou, Anna Bartlett, Qiurui Zeng, Hanqing Liu, Rosa G. Castanon, Mia Kenworthy, et al. “Single-Cell DNA Methylation and 3D Genome Architecture in the Human Brain.” Science 382, no. 6667 (October 13, 2023): eadf5357. https://doi.org/10.1126/science.adf5357.
</details>

- Lee, Dong-Sung, Chongyuan Luo, Jingtian Zhou, Sahaana Chandran, Angeline Rivkin, Anna Bartlett, Joseph R. Nery, et al. “[Simultaneous Profiling of 3D Genome Structure and DNA Methylation in Single Human Cells](https://doi.org/10.1038/s41592-019-0547-z).” Nature Methods, September 9, 2019 - **sn-m3C-seq** - single-nucleus methyl-3C sequencing, extension of snmC-seq2 method, DpnII digestion Fluorescence-Activated Nuclei sorting and the following bisulfite conversion. Cell types can be distinguished by hierarchical clustering (mouse cell types, 4238 human prefrontal cortex cells separated into 14 populations - Astro, Endo, L2/3, L4, L5, L6, MG, MP, Ndnf, ODC, OPC, Pvalb, Sst, Vip, originating from two donors with ages of 21 and 29 years and in a total of five sequencing libraries). TAURUS-MH pipeline, outperforms BWA-METH. sn-m3C-seq methylation correlates well with bulk and single-cell methylation measures. More Hi-C contacts than published datasets. Comparing brain cell subpopulations, chromatin interactions overlap, methylation differ, hypomethylation is associated with increased interactions, differential domain boundaries are associated with differential methylation. [mESC data (raw FASTQ, >600 samples, >60Gb)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE124391), [human brain data (raw FASTQ, >4K samples, >700Gb)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE130711), [.cool 10Mb resolution files](https://salkinstitute.app.box.com/s/fp63a4j36m5k255dhje3zcj5kfuzkyj1/folder/82405563291). [Protocol](https://protocolexchange.researchsquare.com/article/fbc07d40-d794-4cfc-9e7c-294aafbefc10/v1). [Interactive methylation data](http://neomorph.salk.edu/snm3Cseq_human_FC.php), [Hi-C data](https://dixon.salk.edu/projects/snm3Cseq/). [Code scripts](https://github.com/dixonlab/scm3C-seq), [TAURUS-MH pipeline](https://github.com/dixonlab/Taurus-MH), [Twitter](https://twitter.com/Jesse_R_Dixon/status/1171099343236919296?s=03)

- Li, Guoqiang, Yaping Liu, Yanxiao Zhang, Naoki Kubo, Miao Yu, Rongxin Fang, Manolis Kellis, and Bing Ren. “[Joint Profiling of DNA Methylation and Chromatin Architecture in Single Cells](https://doi.org/10.1038/s41592-019-0502-z).” Nature Methods, August 5, 2019 - **Methyl-HiC** - in situ Hi-C and WGBS. mESC cells cultured in serum and leukemia inhibitory factor (LIF) condition (serum mESCs: serum 1 and serum 2) and mESCs cultured in LIF with GSK3 and MEK inhibitors (2i) condition. Comparable Hi-C matrices, TADs. 20% fewer CpGs overall, more CpGs in open chromatin. Proximal CpGs correlate irrespectively of loop anchors, weaker for inter-chromosomal interactions. Application to single-cell, mouse ESCs under different conditions. Relevant clustering, cluster-specific genes. Methods for wet-lab and computational processing. [Bulk (replicates) and single-cell Methyl-HiC data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE119171). [Scripts](https://bitbucket.org/dnaase/bisulfitehic/src/master/), Bhmem pipeline to map bisulfite-converted reads, Juicer pipeline for processing, VC normalization, HiCRep at 1Mb matrix similarity.

#### Imaging

- [MERFISH](https://github.com/BogdanBintu/ChromatinImaging) - Super-resolution imaging technology, reconstruction 3D structure in single cells at 30kb resolution, 1.2Mb region of Chr21 in IMR90 cells. Distance maps obtained by microscopy show small distance for loci within, and larger between, TADs. TAD-like structures exist in single cells. 2.5Mb region of Chr21 in HCT116 cells, cohesin depletion does not abolish TADs, only alter their preferential positioning. Multi-point (triplet) interactions are prevalent. TAD boundaries are highly heterogeneous in single cells. , diffraction-limited and STORM (stochastic optical reconstruction microscopy) imaging. [GitHub](https://github.com/BogdanBintu/ChromatinImaging)
    - Bintu, Bogdan, Leslie J. Mateo, Jun-Han Su, Nicholas A. Sinnott-Armstrong, Mirae Parker, Seon Kinrot, Kei Yamaya, Alistair N. Boettiger, and Xiaowei Zhuang. “[Super-Resolution Chromatin Tracing Reveals Domains and Cooperative Interactions in Single Cells](https://doi.org/10.1126/science.aau1783).” Science, (October 26, 2018)

- Single-cell level massively multiplexed FISH (MERFISH, sequential genome imaging) to measure 3D genome structure in context of gene expression and nuclear structures. Approx. 650 loci, 50kb resolution, on chr21 10.4-46.7Mb from the hg38 genome assembly, IMR90 cells, population average from approx. 12K chr21 copies, multiple rounds of hybridization. Investigation of TADs, A/B compartments, 87% agreement with bulk Hi-C. Association with cell type markers, transcription. Genome-scale imaging using barcodes, 1041 30kb loci covering autosomes and chrX of IMR90, over 5K cells, 5 replicates. [Processed multiplexed FISH data and more, TXT format](https://zenodo.org/record/3928890), [GitHub](https://github.com/ZhuangLab/Chromatin_Analysis_2020_cell)
    - Su, Jun-Han, Pu Zheng, Seon S. Kinrot, Bogdan Bintu, and Xiaowei Zhuang. “[Genome-Scale Imaging of the 3D Organization and Transcriptional Activity of Chromatin](https://doi.org/10.1016/j.cell.2020.07.032).” Cell, August 2020

- [Parser of multiplexed single-cell imaging data from Bintu et al. 2018 and Su et al. 2020](https://github.com/agalitsyna/DPDchrom_input_parser) - Take 3D coordinates of the regions as input and write the distance and contact matrices for these datasets.
