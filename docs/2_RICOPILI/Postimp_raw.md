# Postimp in a new directory
***
**Source document**: <br> [How to run the post-imputation module on raw genotypes or dosages in a new directory](https://docs.google.com/document/d/1uD0VBFr-9UeALZ4rVssJcJG9855nQ7tgX6B-zoamosg/edit?tab=t.0) <br>
**Authors**: <br>
Gio Panagiotaropoulou, Ph.D. student, [gpanagio@broadinstitute.org](mailto:gpanagio@broadinstitute.org) <br>
***

When you want to run some post-imputation analysis in new directory, different than the original one that imputation was performed (i.e. when you want to run the post-imputation module individually), you have to link in (or copy) a number of files that are implicitly required by the postimp command: <br>
## RICOPILI module my.joinimp2
There is a script that does this job while combining multiple source directories: `my.joinimp2`. <br> 
You should be able to find it in your ricopili distribution. <br>

## Manually
In case you want to do this manually: <br>

- The `dasuqc1_*` directories that contain the imputed genomic chunks for every study of interest;
- The (pre-QC) bed/bim/fam files of every study of interest.
- The “reference_info” file, which is required to combine the genomic chunk files across the entire genome;
- The command will also need a “datasets_info” file with the list of filenames of all studies that are to be included.  If the pipeline doesn’t find a datasets_info file in its working directory, it will attempt to create one using the filenames of every *.bim file that it will find in the working directory. You can also create this by echoing into a text file the name of every study (the rest of the filename that follows after “dasuqc1_”).
- If case-controls studies are present, you will also need to link in the `*.mds_cov` file with the population covariates. You need to explicitly reference this file with the --mds option of the postimp_navi command.
- For some cases (and also to keep everything together) it might make sense to link in the best guess genotypes from the “cobg_dir_genome_wide/” subdirectory.
