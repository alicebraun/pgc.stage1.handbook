# Modules

**For data intake on SURFsnellius** <br>
**Source document**: <br> [RICOPILI: Sub-Steps of each module](https://docs.google.com/document/d/13ljVIhKYek_LqmvywZ94f4Mh7hxF5jOlXEGPLYQSjnc/edit?tab=t.0#heading=h.1e6uecnclnyb) <br>
**Authors**: <br>
Swapnil Awasthi, Ph.D. student, [swapnil.awasthi@charite.de](mailto:swapnil.awasthi@charite.de) <br>
Prof. Stephan Ripke, M.D., Ph.D., Group Leader, [stephan.ripke@charite.de](mailto:stephan.ripke@charite.de)
***

# Substeps of each module 
## Preimputation (preimp_dir)
***
| Short Name | Step                               | i=datasets | Default Hardware Request         |
|------------|------------------------------------|------------|----------------------------------|
| plague     | Genotyping platform guess from the PLINK .bim files  | i          | h_vmem=2g h_rt=1:00:00          |
| qc         | Quality control/ data management                 | i          | h_vmem=2g h_rt=1:00:00          |


##Principal Component Analysis (pcaer)
***

| Short Name  | Step                                                    | i=datasets | Default Hardware Request         |
|-------------|---------------------------------------------------------|------------|----------------------------------|
| me300       | Merging all plink files together                        | 1          | h_vmem=3g -l h_rt=1:00:00       |
| pruneprep   | Prune1                                                  | 1          | h_vmem=1g h_rt=1:00:00          |
| prunebed    | prune2                                                  | 1          | h_vmem=1g h_rt=1:00:00          |
| mepr        | Pruning (2 step approach), filtering                    | 1          | h_vmem=3g h_rt=2:00:00          |
| genome      | Overlap and Relatedness testing, including reporting as PDF | ~n/200+1; n=sample size | h_vmem=4g h_rt=2:00:00 |
| repov       | (cont.)                                                 | 1          | h_vmem=1g h_rt=1:00:00          |
| epca        | PCA with eigenstrat                                     | 1          | h_vmem=6g h_rt=2:00:00          |
| asso        | Genomewide association of PCAs with genotypes           | 20         | h_vmem=2g h_rt=1:00:00          |
| gwapl       | (Plots in various flavours)                             | 20         | h_vmem=2g h_rt=1:00:00          |
| qqpl        | (cont.)                                                 | 40         | h_vmem=2g h_rt=1:00:00          |
| lahu        | (cont.)                                                 | 1          | h_vmem=1g h_rt=1:00:00          |
| pcaplot     | (cont.)                                                 | (1-3)      | h_vmem=2g h_rt=1:00:00          |
|             |                                                         | 4          | h_vmem=1g h_rt=1:00:00          |

## Imputation (impute_dir)
***

| Short Name      | Step                                                               | i=datasets, j = number of genomic chunks(j=131) | Default Hardware Request         |
|-----------------|--------------------------------------------------------------------|--------------------------------------------------|----------------------------------|
| buigue          | Guess genome build, liftover to hg19 if necessary                 | i                                                | h_vmem=2g h_rt=2:00:00          |
| readref         | Creating subsets of the reference information for higher efficiency in next steps | 22*i                                              | h_vmem=2g h_rt=2:00:00          |
| reresum         | (cont.)                                                           | i                                                | h_vmem=1g h_rt=2:00:00          |
| predep          | Preparation of datasets for deployment to imputation server       | i                                                | h_vmem=2g h_rt=2:00:00          |
| chepos          | Align SNP names based on positions and alleles, standardize indel coding | i                                                | h_vmem=8g h_rt=2:00:00          |
| chefli          | Align alleles, resolve strand flips                                | i                                                | h_vmem=8g h_rt=2:00:00          |
| preph           | Prephasing in genomic chunks                                      | i*j                                              | h_vmem=2g h_rt=2:00:00          |
| pseudo          | (create pseudo-case and control for trios)                        | i*5                                              | h_vmem=13g h_rt=1:00:00         |
| imp             | Imputation in genomic chunks                                      | i*j                                              | h_vmem=4g h_rt=2:00:00          |
| dos             | Data formatting in genomic chunks                                  | i*j                                              | h_vmem=2g h_rt=2:00:00          |
| dabg            | Postimputation QC/Best guess in genomic chunks                    | i*j                                              | h_vmem=2g h_rt=24:00:00         |
| cobg_gw         | Genome wide best guess                                             | i*3                                              | h_vmem=12g h_rt=2:00:00         |
| clean           | Clean temporary files                                             | (i*7)+1                                           | h_vmem=1g h_rt=2:00:00          |
| du              | Evaluate hard disk usage and performance of single steps          | 1                                                | h_vmem=1g h_rt=2:00:00          |
| vcf2dos_deploy  | Integrating imputation-server data for postimp module             |                                                  | h_vmem=4g h_rt=2:00:00          |

## Postimputation (postimp_dir)
***

| Short Name    | Step                                                             | i=datasets, j=number of genomic chunks, k=Number of GWS hits | Default Hardware Request         |
|---------------|------------------------------------------------------------------|------------------------------------------------------------|----------------------------------|
| daner**       | Association analysis in genomic chunks                           | i*j                                                        | h_vmem=1g h_rt=2:00:00          |
| chunk*        | Chunking whole genome meta analyses into j’ chunks               | i                                                          | h_vmem=1g h_rt=1:00:00          |
| dameta**      | Meta analysis in genomic chunks                                  | j                                                          | h_vmem=1g h_rt=2:00:00          |
| resmet*       | Meta analysis from chunked summary statistics                    | j’                                                         | h_vmem=1g h_rt=2:00:00          |
| damecat       | Collection of genomewide results                                 | i+1                                                        | h_vmem=1g h_rt=1:00:00          |
| daner_het2p   | Create a separate set for heterogeneity P (only for meta analyses) | 1                                                          | h_vmem=1g h_rt=1:00:00          |
| lth           | Count SNPs for various thresholds, create top list               | i+1                                                        | h_vmem=1g h_rt=1:00:00          |
| areator       | Clumping of top results                                          | i+1                                                        | h_vmem=3g h_rt=1:00:00          |
| areaplot      | Region plots                                                     | k (minus overlapping regions)                               | h_vmem=1g h_rt=1:00:00          |
| forestplot    | Forest plots                                                     | k (plus additional index SNPs)                              | h_vmem=1g h_rt=1:00:00          |
| manhplot      | Manhattan plots                                                  | i+1                                                        | h_vmem=1g h_rt=1:00:00          |
| qqplot        | QQ-plot                                                          | i+1                                                        | h_vmem=3g h_rt=1:00:00          |
| ldsc          | LD score                                                         | 1                                                          | h_vmem=4g h_rt=1:00:00          |
| lahu          | Lambda plot                                                      | (i*4)+6                                                     | h_vmem=2g h_rt=1:00:00          |


## Clumper (clump_nav)
***

| Short Name | Step                                                               | i=datasets | Default Hardware Request         |
|------------|--------------------------------------------------------------------|------------|----------------------------------|
| clump      | Chunking the meta-analysis by chromosome & Clumping                | 22         | h_vmem=2g h_rt=2:00:00          |
