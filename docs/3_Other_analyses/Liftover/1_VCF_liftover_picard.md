# Conducting liftover using Picardtools
***
**Author**: <br> Alice Braun, M.Sc., B.A. [braun@broadinstitute.org](mailto:braun@broadinstitute.org)<br> 
***
## Download the reference sequence per chromosome and write a reference dictionary

```bash
#!/bin/bash
#SBATCH --job-name=ensembl_ref         # Name of the job <br>
#SBATCH --output=logs/ensembl_ref_%A_%a.log  # Output log file in logs directory 
#SBATCH --error=logs/ensembl_ref_%A_%a.err   # Error log file in logs directory
#SBATCH --time=6:00:00                       # Time limit (format: HH:MM:SS)
#SBATCH --cpus-per-task=16                   # Number of CPUs per task
#SBATCH --mem=16G                            # Memory per task
#SBATCH --array=1-22                         # Array job for chromosomes 1 to 21 

# Navigate to the working directory on pgcdrc
cd /gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/pgcdrc/scz/working/HRC1.1_hg38/ensembl || exit 1

# Load the necessary modules 
module load 2023
module load Java/17.0.6

# Get the chromosome number from the SLURM_ARRAY_TASK_ID
CHR=${SLURM_ARRAY_TASK_ID}

# Download the chromosome-specific reference file from Ensembl
wget -q https://ftp.ensembl.org/pub/release-113/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.chromosome.${CHR}.fa.gz

# Decompress the downloaded file
gunzip -f Homo_sapiens.GRCh38.dna.chromosome.${CHR}.fa.gz

# Define the reference file name 
REFERENCE_FASTA="Homo_sapiens.GRCh38.dna.chromosome.${CHR}.fa"

# Create a sequence dictionary using Picard
java -jar ../software/picard.jar CreateSequenceDictionary \
      R=${REFERENCE_FASTA} \
      O=${REFERENCE_FASTA}.dict

```
## Recode chromosome names
Newer reference files have a "chr" prefix before the chromosome number (e.g. chr22 instead of 22 in the CHR column). <br>
If liftover is failing due to this issue, you may need to use BCF tools to change the naming scheme using the follwing jobscript:<br>
```bash

#!/bin/bash
#SBATCH --job-name=bcf_rename         # Name of the job
#SBATCH --output=bcf_rename_%A_%a.log # Output log file, includes array and task IDs
#SBATCH --error=bcf_rename_%A_%a.err  # Error log file, includes array and task IDs
#SBATCH --time=12:00:00               # Time limit (format: HH:MM:SS)
#SBATCH --cpus-per-task=16            # Number of CPUs per task
#SBATCH --mem=10G                     # Memory per task
#SBATCH --array=1-22                  # Array job for chromosomes 1 to 22

# Load the necessary modules 
module load 2023
module load BCFtools/1.18-GCC-12.3.0

# Navigate to the working directory on pgcdrc
cd /gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/pgcdrc/scz/working/HRC1.1_hg38/all_chromosomes_input || exit 1

# Get the chromosome number from the SLURM_ARRAY_TASK_ID
CHR=${SLURM_ARRAY_TASK_ID}

# Create a symbolic link to the chromosome VCF file
ln -s /gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/HRC_reference.r1-1/HRC.r1-1.EGA.GRCh37.chr${CHR}.haplotypes.vcf.gz || exit 1

# Index the VCF file if it does not already exist
if [ ! -f HRC.r1-1.EGA.GRCh37.chr${CHR}.haplotypes.vcf.gz.csi ]; then
    bcftools index --threads 16 HRC.r1-1.EGA.GRCh37.chr${CHR}.haplotypes.vcf.gz
fi

# Prepare the chromosome name conversion file
echo -e "${CHR}\tchr${CHR}" > chr_name_conv.txt

# Run BCFtools to add "chr" prefix to chromosome names
bcftools annotate --threads 16 --rename-chrs chr_name_conv.txt \
    HRC.r1-1.EGA.GRCh37.chr${CHR}.haplotypes.vcf.gz \
    -Oz -o HRC.r1-1.EGA.GRCh37.chr${CHR}.haplotypes.prefix.vcf.gz || exit 1

# Index the newly created VCF file
bcftools index --threads 16 HRC.r1-1.EGA.GRCh37.chr${CHR}.haplotypes.prefix.vcf.gz

# Clean up the chromosome name conversion file
rm -f chr_name_conv.txt
```


## Conduct the liftover
```bash
#!/bin/bash
#SBATCH --job-name=picard_liftover         # Name of the job
#SBATCH --output=logs/picard_liftover_%A_%a.log  # Output log file in logs directory
#SBATCH --error=logs/picard_liftover_%A_%a.err   # Error log file in logs directory
#SBATCH --time=12:00:00               # Time limit (format: HH:MM:SS)
#SBATCH --cpus-per-task=16            # Number of CPUs per task
#SBATCH --mem=120G                     # Memory per task
#SBATCH --array=1-21                  # Array job for chromosomes 1 to 21 (22 was done as a test)

# Create logs directory if it doesn't exist
mkdir -p logs

# Navigate to the working directory on pgcdrc
cd /gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/pgcdrc/scz/working/HRC1.1_hg38/ || exit 1

# Load the necessary modules 
module load 2023
module load Java/17.0.6

# Get the chromosome number from the SLURM_ARRAY_TASK_ID
CHR=${SLURM_ARRAY_TASK_ID}

# Verify SLURM_ARRAY_TASK_ID is set
if [[ -z "${CHR}" ]]; then
    echo "Error: SLURM_ARRAY_TASK_ID is not set." >&2
    exit 1
fi

# Define the reference genome and reject file dynamically
REFERENCE_FASTA=ensembl/Homo_sapiens.GRCh38.dna.chromosome.${CHR}.fa
REJECT=HRC.GRCh38.chr${CHR}.ensembl.rejected.vcf.gz

# Run the Picard LiftoverVcf command
java -Xmx100g -jar software/picard.jar LiftoverVcf \
     I=all_chromosomes_input/HRC.r1-1.EGA.GRCh37.chr${CHR}.haplotypes.prefix.vcf.gz \
     O=all_chromosomes_output/HRC.r1-1.EGA.GRCh38.ensembl.chr${CHR}.haplotypes.vcf.gz \
     CHAIN=ensembl/GRCh37_to_GRCh38.chain \
     REJECT=${REJECT} \
     R=${REFERENCE_FASTA} \
     RECOVER_SWAPPED_REF_ALT=true \
     MAX_RECORDS_IN_RAM=5000 \
     WARN_ON_MISSING_CONTIG=true \
     WRITE_ORIGINAL_POSITION=true || {
         echo "Error: LiftoverVcf failed for chromosome ${CHR}" >&2
         exit 1
     }
```