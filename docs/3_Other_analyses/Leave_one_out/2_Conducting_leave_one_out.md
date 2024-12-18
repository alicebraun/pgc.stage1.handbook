# Conducting leave-one-out analyses
***
**Author**: <br> Alice Braun, M.Sc., B.A. [braun@broadinstitute.org](mailto:braun@broadinstitute.org)<br> 
***
In order to conduct LNO analyses on SURFsnellius we need to navigate to the following working directories: <br>
`/gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/pgcdrc/scz/working/wave3/summary_stats/`
It contains several sub-directories: <br>


``` 
.../summary_stats/ 
├─  chrX  
├─  PRS  
├─  README.txt 
└─── autosomes 
    ├─  female_all 
    ├─  female_eu 
    ├─  female_eu 
    ├─  male_all
    ├─  male_eu
    ├─  meta_gwas 
    └─── single 
        └─── assoc_single
```

The automsomes directory contains several filtered subsets of the individual 90 cohorts <br>
In order to run a LNO analyses we take the table generated by the requesting researcher in 1_Leave-one-out_requests and take the <mark>PRS_results_file_name</mark> column to generate a results file.<br>
To run the LNO analyses using RICOPILI you run the follwing command from within the directory with the relevant subset (all, male_all etc.):<br>

```
cd /gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/pgcdrc/scz/working/wave3/summary_stats/autosomes/single/assoc_single 
postimp_navi --out NAME --result results_FILE --addout SUFFIX --noldsc 
```
After the result danerfile with the LNO meta-analysis results is ready it needs to be moved to PGC DAC at:<br>
`/gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/scz/wave3/summary_stats/autosomes/lno/`


## Custom LNO
***
Sometimes people may request custom LNO analyses, requiring to run a new GWAS on wave3 genotypes on the Broad cluster. <br>
The imputed genotypes (using 1KG) are located on the broad at `/psych/ripke/scz/wave3/scz_phase3_imputation/batch_3`. The mds_cov files with the covariates are located at `/psych/ripke/scz/wave3/scz_phase3_imputation/batch123`.
