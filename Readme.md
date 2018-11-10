---
title: "ssGSEA2.0 / PTM-SEA"
output:
  html_document:
    toc: true
    toc_depth: 3
    number_sections: false
    toc_float:
      smooth_scroll: true
---

***

Resources for gene-centric **single sample Gene Set Enrichment Analysis (ssGSEA)** of gene expression data (e.g. mRNAs, proteins) and site-centric **PTM Signature Enrichment Analysis (PTM-SEA)**[1] of phosphoproteomics data sets using the **PTM signatures database (PTMsigDB)** [1].

## ssGSEA 2.0
This is an updated version of the original ssGSEA [2,3] R-implementation. Depending on the input dataset and chosen database (gene sets or PTM signatures), the software performs either ssGSEA or PTM-SEA, respectively. The Molecular Signatures Database ([MSigDB](http://software.broadinstitute.org/gsea/msigdb/)) [4] provides a large collection of curated gene sets.  Gene sets are stored as plain text in  [GMT](https://software.broadinstitute.org/cancer/software/gsea/wiki/index.php/Data_formats#GMT:_Gene_Matrix_Transposed_file_format_.28.2A.gmt.29) format. A current version of MSigDB gen set collections can be found in the ```db``` subfolder.

File formats supported by ssGSEA2.0/PTM-SEA are Gene Cluster Text [GCT v1.2](https://software.broadinstitute.org/cancer/software/gsea/wiki/index.php/Data_formats#GCT:_Gene_Cluster_Text_file_format_.28.2A.gct.29) or [GCT v1.3](https://clue.io/connectopedia/gct_format) files. [Morpheus](https://software.broadinstitute.org/morpheus/) provides a convenient way to convert your data tables into GCT format.

For information about the GSEA method and MSigDB please visit http://software.broadinstitute.org/gsea/.


## PTMsigDB
The PTM signatures database (PTMsigDB) is a database comprised of modification site-specific signatures of perturbations, kinase activities and signaling pathways curated from more than 2,500 publications which provides the foundation to perform PTM-SEA. A unique advantage of PTMsigDB over other pathway databases is the annotation of each PTM site with its reported direction of change upon a specific perturbation or signaling event which is incorporated into the scoring scheme of PTM-SEA. The foundation of PTMsigDB is PhosphoSitePlus (PSP) [5], a comprehensive systems biology resource for PTMs, which provides high-quality curation and annotation of PTMs at the individual residue level. A collection of PTM sites, whose levels are collectively regulated in a curated pathway or upon a perturbation, are defined as a signature set. Signature sets in PTMsigDB can be separated into three categories: 1) Perturbation signatures derived from treatment of cells with perturbagens such as small molecules or growth factors; 2) Signature sets of molecular signaling pathways; and 3) Kinase-substrate signatures.

To ensure a high degree of compatibility to phosphorylation datasets generated by different software packages and searched against different protein sequence databases, PTMsigDB represents signatures using three different identifiers to represent phosphorylation sites: 1) PSP site group ID; 2) UniProt-centric ID; 3) Flanking sequence (Table 1). While the PSP site group ID provides an unambiguous representation of PTM sites within protein families and across species [5], using this type of identifier restricts the analysis to PTM sites present in PSP. We generally recommend to usinge the flanking sequence as site identifier, since these are more invariant to updates made to protein sequence databases.

| Database format   | Site accession | Example | Download
| ----------------- | -------------- | ------------ | -------------------
| UniProt-centric   | Uniprot_acc;site-type;direction | Q06609;Y315-p;u | [human](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.uniprot.human.v1.8.1.gmt)<br>[mouse](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.uniprot.mouse.v1.8.1.gmt)<br>[rat](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.uniprot.rat.v1.8.1.gmt)
| Flanking sequence | +/-7aa flanking seq-type;direction | ETRICKIYDSPCLPE-p;u | [human](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.flanking.human.v1.8.1.gmt)<br>[mouse](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.flanking.mouse.v1.8.1.gmt)<br>[rat](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.flanking.rat.v1.8.1.gmt)
| PSP site group id | site_grp_id-type;direction | 448324-p;u | [human](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.sitegrpid.human.v1.8.1.gmt)<br>[mouse](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.sitegrpid.mouse.v1.8.1.gmt)<br>[rat](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/db/ptm.sig.db.all.sitegrpid.rat.v1.8.1.gmt)

Table: Table 1: PTM site representation in PTMsigDB


## PTM-SEA
PTM-Signature Enrichment Analysis (PMT-SEA) is a modified version of ssGSEA to perform site-specific signature analysis by scoring PTMsigDB's bi-directional signature-sets. The input to PTM-SEA is a single site-centric data matrix, *m*, stored in [GCT v1.2](https://software.broadinstitute.org/cancer/software/gsea/wiki/index.php/Data_formats#GCT:_Gene_Cluster_Text_file_format_.28.2A.gct.29) or [GCT v1.3](https://clue.io/connectopedia/gct_format)  format and PTM signatures database (PTMsigDB). Each row in *m* represents a single phosphorylation site confidently localized to a specific amino acid residue, with measured abundances across samples specified in columns in *m*. Multiple phosphorylation sites detected on the same peptide have to be converted into separate site-specific entities for every site. While some proteomics software packages, such as  MaxQuant [6], readily produce single site-centric PTM reports, the use of other software packages might require additional preprocessing steps.


## How can I use these tools?

ssGSEA2.0/PTM-SEA can be run on a local PC/MAC in R or RStudio. In addition, ssGSEA2.0/PTM-SEA can be access on Broad's public GenePattern [7] server. Below we provide instructions how to run ssSGEA2.0/PTM-SEA.

### Example dataset

We provide an example data set that can be used to test PTM-SEA. The dataset is based on Supplemental Table 6 in [1]..

[Single site-centric phosphoproteome dataset](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/example/PI3K_pert_logP_n2x23936.gct)

[PTMsigDB](https://raw.githubusercontent.com/broadinstitute/ssGSEA2.0/master/example/ptm.sig.db.all.flanking.human.v1.8.1.gmt)


### GenePattern
[GenePattern](http://software.broadinstitute.org/cancer/software/genepattern) is a powerful platform to deploy and run software or entire analysis pipelines in a web browser [4]. We have implemented ssGSEA2.0/PTM-SEA as GenePattern module which can be accessed at the link below. Please note that access to the public GenePattern server requires a free registration.

PTM-SEA in GenePattern: https://tinyurl.com/GP-ssGSEA-PTM-SEA

### R-GUI / RStudio
The script ```ssgsea-gui.R``` requires little or no knowledge of R or how to use the command line. Input files and databases can be specified via Windows file dialogs that will be automatically invoked. The first dialog lets you choose a folder containing input files in [GCT v1.2](https://software.broadinstitute.org/cancer/software/gsea/wiki/index.php/Data_formats#GCT:_Gene_Cluster_Text_file_format_.28.2A.gct.29) or [GCT v1.3](https://clue.io/connectopedia/gct_format) format. The script loops over all GCT files in this directory and runs ssGSEA on each file separately. The second dialog window lets the user choose one or multiple gene set databases in [GMT](https://software.broadinstitute.org/cancer/software/gsea/wiki/index.php/Data_formats#GMT:_Gene_Matrix_Transposed_file_format_.28.2A.gmt.29) format such as [MSigDB](http://software.broadinstitute.org/gsea/msigdb/). A current version of MSigDB databases can be found in the ```db``` subfolder. 


##### **Windows OS**
To run the script source it into a running R-session.

- [RStudio](https://www.rstudio.com/products/rstudio/download/): open the file and press 'Source' in the upper right part of the editor window 
- [R-GUI](https://cran.r-project.org/bin/windows/base/): drag and drop this file into an R-GUI window

##### **iOS/MAC** 
In order to invoke file dialogs as decribed above, the [XQuartz](https://www.xquartz.org) X Window System is required. Once installed ```ssgsea-gui.R``` can be sourced into an R session.


### Command line
For integration of ssGSEA2.0/PTM-SEA into your own analysis pipelines we recommend to use the ```ssgsea-cli.R``` script which has been successfully tested on Windows, Mac and Linux OS. Please see ```ssgsea-cli.R --help``` for instructions.



## Misc

### ssGSEA2.0/PTM-SEA parameters

Other paramaters for ssGSEA/PTM-SEA can be altered inside the parameters section in ```ssgsea-gui.R``` or as arguments on the command line. The default parameters have been choosen carefully and should provide reliable results for most use-case scenerios. 


### Changes to the original ssGSEA R-implementation
Original code written by Pablo Tamayo. Adapted with additional modifications by D. R. Mani and Karsten Krug. Adaptions include:

- support of multiple CPU cores (```doParallel``` R-package)
- support of GCT v1.3 format using functions from [cmapR](https://github.com/cmap/cmapR)
- improved handling of missing values
- scoring of directional gene sets (PTMsigDB)
- basic error handling
- general performance improvements
- additional output files like rank plots and parameter files


## References
1.  Krug, K., Mertins, P., Zhang, B., Hornbeck, P., Raju, R., Ahmad, R., . Szucs, M., Mundt, F., Forestier, D., Jane-Valbuena, J., Keshishian, H., Gillette, M. A., Tamayo, P., Mesirov, J. P., Jaffe, J. D., Carr, S. A., Mani, D. R. A curated resource for phosphosite-specific signature analysis, submitted

1.  Barbie, D. A., Tamayo, P., Boehm, J. S., Kim, S. Y., Susan, E., Dunn, I. F., . Hahn, W. C. (2010). Systematic RNA interference reveals that oncogenic KRAS- driven cancers require TBK1, 462(7269), 108-112. https://doi.org/10.1038/nature08460

1. Abazeed, M. E., Adams, D. J., Hurov, K. E., Tamayo, P., Creighton, C. J., Sonkin, D., et al. (2013).
       Integrative Radiogenomic Profiling of Squamous Cell Lung Cancer. Cancer Research, 73(20), 6289-6298.
       http://doi.org/10.1158/0008-5472.CAN-13-1616

1. Subramanian, A., Tamayo, P., Mootha, V. K., Mukherjee, S., Ebert, B. L., Gillette, M. A., et al. (2005).
   Gene set enrichment analysis: a knowledge-based approach for interpreting genome-wide expression profiles.
  Proceedings of the National Academy of Sciences of the United States of America, 102(43), 15545-15550. http://doi.org/10.1073/pnas.0506580102

1. Hornbeck, P. V., Zhang, B., Murray, B., Kornhauser, J. M., Latham, V., & Skrzypek, E. (2015). PhosphoSitePlus, 2014: mutations, PTMs and recalibrations. Nucleic Acids Research, 43(D1), D512-D520. https://doi.org/10.1093/nar/gku1267
 

1. Cox, J., & Mann, M. (2008). MaxQuant enables high peptide identification rates, individualized p.p.b.-range mass accuracies and proteome-wide protein quantification. Nature Biotechnology, 26(12), 1367-1372. https://doi.org/10.1038/nbt.1511


1. Reich, M., Liefeld, T., Gould, J., Lerner, J., Tamayo, P., & Mesirov, J. P. (2006). GenePattern 2.0. Nature Genetics, 38(5), 500-501. https://doi.org/10.1038/ng0506-500

      

***
