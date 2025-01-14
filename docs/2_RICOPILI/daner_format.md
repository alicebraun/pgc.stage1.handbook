# Ricopili daner format

### Vassily Trubetskoy, **[vassily@broadinstitute.org](mailto:vassily@broadinstitute.org)** Prof. Stephan Ripke, M.D., Ph.D., Group Leader, [stephan.ripke@charite.de](mailto:stephan.ripke@charite.de)

[GResU](https://www.bihealth.org/de/forschung/projekte/gwas-research-unit/%20) (GWAS Research Unit, BIH, Berlin)

# **(Last modified on: 18.09.2019)**

# 

The daner file reports GWAS summary statistics for a set of variants. It is a tab delimited format with a header line and one row per variant tested. The name “daner” comes from “Dosage ANalyzER” and reflects the fact that these summary statistic originally came from analysis of imputed genotype dosages. The format has grown and has been adapted for use across the Ricopili code base.

Please also refer to [this document about meta analyses over sumstat files](https://docs.google.com/document/d/1o4bN_uLK4IEItXCSdeQkXfZwEpLuSCWlveJevRogi08/edit#heading=h.4008addvumol) since the topics are highly overlapping.

There are two situations in which you typically encounter daner files. The format and interpretation of columns changes slightly in each.

# 

# 1 \- Single study GWAS

The single study daner contains the following 12 columns, in order:

| Column  Position | Column Header | Description |
| :---: | :---: | ----- |
| 1 | *CHR* | Chromosome identifier; Ricopili expects numeric identifiers. |
| 2 | *SNP* | Unique SNP identifier |
| 3 | *BP* | Base pair position |
| 4 | *A1* | Allele 1; odds ratios are reported with respect to this allele. |
| 5 | *A2* | Allele 2 |
| 6 | *FRQ\_A\_{\#Cases}* | Allele frequency with respect to A1 in cases. The number of cases is appended to the header field. In the case of continuous phenotypes, the number of cases will be set to the sample size. In meta-analyses, this number shows as the maximum achievable Case-Number (see below) |
| 7 | *FRQ\_U\_{\#Controls}* | Allele frequency with respect to A1 in controls. The number of controls is appended to the header field. In the case of continuous phenotypes, the number of controls will be set to the sample size. In meta-analyses, this number shows as the maximum achievable Control-Number |
| 8 | *INFO* | Imputation quality metric as calculated from the ratio of expected and observed variance (provided from plink). |
| 9 | *OR/BETA* | This field will either be “*OR*” or “*BETA*”. The “*OR*” refers to odds ratio and will be present when the phenotype is binary. The “*BETA*” refers to effect size and will be present when the phenotype is continuous. In either case, both are reported using the A1 allele as the risk/effect allele. |
| 10 | *SE* | The standard error of the effect size estimate. For case-control results, this is the standard error of the “beta” from the logistic regression, and not the standard error of the odds ratio. |
| 11 | *P* | The corresponding p-value of the test. |
| 12 | *ngt* | Number of datasets where the SNP was genotyped directly (i.e. not imputed). This will be a maximum of 1 for the single study case. This column is for display only and does not influence any of the other parameters. |

# 2 \- Multi-study GWAS meta-analysis

This daner file will contain the 12 columns above, and additionally the following columns are appended:

| Column  Position | Column Header | Description |
| :---: | :---: | ----- |
| 13 | *Direction* | This is a string of characters “+”, “-” or “?”. The number of characters is the number of studies in the meta-analysis. Each character represents direction of the estimated effect: “+” is one direction, “-” is the other direction, and “?” means that this variant was missing in the original study.  |
| 14 | *HetISqt* | The I² statistic for estimating heterogeneity among study effects. It summarizes the amount of variation across studies attributable to heterogeneity between studies. |
| 15 | *HetDf* | The degrees of freedom for the heterogeneity test. |
| 16 | *HetPVa* | The p-value of the test of heterogeneity among estimated effects in the studies. |
| 17 | *Nca* | The number of cases used in the test. This Number is used as a weight on the calculation of resulting FRQ\_A in meta-analyses. |
| 18 | *Nco* | The number of controls in the overall meta-analysis. This Number is used as a weight on the calculation of resulting FRQ\_U in meta-analyses. |
| 19 | *Neff\_half* | The effective sample size. For single cohorts this is defined as 4\*Nca\*Nco/(2\*(Nca+Nco)) . In meta analyses these numbers are summed over cohorts.  |

