# Conducting liftovers
Once in a while you may need to lift your genotypes over to another genome build (e.g. hg19 to hg38). <br>
Thankfully, there is a RICOPILI module available that does this job for you, given the data is in PLINK .BED format. <br>
The RICOPILI module runs the UCSC LiftOver tool in the background. <br>
If you need to liftover a VCF file or even a reference VCF file a useful tool is Picard Liftover, which works without transforming the VCF to BED first. <br>
Code snippets to run Picardtools Liftover on SURFsnellius can be found in this directory. <br>
In the example code we use Ensembl reference and chain files for hg19 to hg38 liftover.
