# Conducting liftovers using RICOPILI
***
**Author**: <br> Alice Braun, M.Sc., B.A. [braun@broadinstitute.org](mailto:braun@broadinstitute.org)<br> 
***
Occasionally, you may need to lift your genotypes over to another genome build (e.g. to hg38). <br>
Thankfully, there is a RICOPILI module available that does this job for you, given the data is in PLINK .BED format. <br>
The RICOPILI module runs the UCSC LiftOver tool in the background. <br>
If you need to liftover a VCF file or even a reference VCF file a useful tool is Picard Liftover, which works without transforming the VCF to BED first. <br>
Code snippets to run Picardtools Liftover on SURFsnellius can be found in this directory. <br>
In the example code we use Ensembl reference and chain files for hg19 to hg38 liftover. <br>

Align PLINK .BED files e.g. before merging two datasets together Lift PLINK files to Human Genome Build 19
```
buigue --help

Usage : buigue bim-file

version: 1.0.0

  -help            print this message and exit

  --lift19         lift dataset to hg19
  --lift38         lift dataset to hg38

  --noclean        do not remove temporary files

  --debug          extended ouput



 guesses the build of a bim file out of ucsc snp file
```

!!! note
    You may need additional sequence reference files or chain files for specific liftovers. <br>
    There can be downloaded either from `https://personal.broadinstitute.org/sawasthi/share_links/XSEebTMRjD8gWfKwMovLRR67ImAv0t_chanifile_snellius.tar.gz/chanifile_snellius.tar.gz`<br> or from **the USCS repository, that maintains the liftOver tool**: <br>
    e.g. [https://hgdownload.soe.ucsc.edu/goldenpath/hg19/liftOver/](https://hgdownload.soe.ucsc.edu/goldenpath/hg19/liftOver/) <br>
    or [https://hgdownload.soe.ucsc.edu/goldenpath/hg38/liftOver/](https://hgdownload.soe.ucsc.edu/goldenpath/hg38/liftOver/)
