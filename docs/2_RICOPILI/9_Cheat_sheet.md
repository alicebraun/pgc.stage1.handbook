# Cheat Sheet
***
**Source document**: [RICOPILI Cheat Sheet](https://docs.google.com/document/d/1X9SamlZi-Ze4nBRQkHwk8pClwqVeTFlk-GDyUltk9gg/edit?tab=t.0#heading=h.lejnguklkite) <br>
**Authors**: <br> Alice Braun, M.Sc., B.A. [braun@broadinstitute.org](mailto:braun@broadinstitute.org)<br> 
Prof. Stephan Ripke, M.D., Ph.D., Group Leader, [stephan.ripke@charite.de](mailto:stephan.ripke@charite.de)


## Overview of RICOPILI Quality Control (QC) Steps
The default QC parameters used by Ricopili, are in order below. It is possible to change the QC thresholds using optional commands.

| **QC Category**       | **Criteria**                                                                                     |
|------------------------|-------------------------------------------------------------------------------------------------|
| SNP QC                | Call rate ≥ 0.95 (useful when merging case and control datasets from different studies)         |
| Sample QC             | Call rate in cases or controls ≥ 0.98                                                          |
| Sample QC             | FHET within ±0.20 in cases or controls                                                         |
| Sample QC             | Sex violations (excluded) - genetic sex does not match pedigree sex                            |
| Sample QC             | Sex warnings (not excluded) - undefined phenotype / ambiguous genotypes                        |
| SNP QC                | Call rate ≥ 0.98                                                                               |
| SNP QC                | Missing difference ≤ 0.02                                                                      |
| SNP QC                | SNPs with no valid association p-value are excluded (i.e., invariant SNP)                      |
| SNP QC                | Minor Allele Frequency (MAF) ≥ 0.01                                                            |
| SNP QC                | Hardy-Weinberg equilibrium (HWE) in controls p-value ≥ 1e-06 (≥ log₁₀(p) of -6, in Ricopili)   |
| SNP QC                | Hardy-Weinberg equilibrium (HWE) in cases p-value ≥ 1e-10 (≥ log₁₀(p) of -10, in Ricopili)     |


## General Tips 
We strongly recommend going through detailed UNIX tutorials like this one here: ​​http://www.ee.surrey.ac.uk/Teaching/Unix/ 
**VERY IMPORTANT FOR BEGINNERS**: <br>
The Tab key     `↹`    will autocomplete filenames and commands <br>
Also if you press the “up” key   ⇧   (also repeatedly), you can jump to your former commands, edit them and restart <br>
Pressing   `control   +  A`  (on Mac) will jump to the beginning of the row,  `control  +  E`  to the end.  <br>
An asterisk   `*`   will automatically select multiple files, e.g. “S*.txt” with lead to “S1.txt Steyeye.txt and S7.txt” if existing <br>
A dot   `.`  stands for the current working directory. <br>
A tilde  `~`  stands for the home directory. <br>
Capitalization is enforced. E.g. “S*txt” will not find the file “s1-myfile1.txt” <br>
There are many more hacks like these, easy to find in WWW! <br>

## General UNIX commands for every day
| **Description**                                                     | **Command**                                                                                  |
|----------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| **Show the content of the current working directory**               | `ls`                                                                                        |
| More detailed contents, showing user info, file size, and modification date/time | `ls -la`                                                                                   |
| List all files in the directory in long list format                 | `ls -l`                                                                                     |
|                                                                      |                                                                                             |
| **Changing the current working directory**                          |                                                                                             |
| Determine the path to the current directory                         | `pwd`                                                                                       |
| Change to a specific directory                                      | `cd /home/my_directory/go_here`                                                            |
| Go to Root Directory                                                | `cd`                                                                                       |
| Move one directory level up                                         | `cd ..`                                                                                     |
| Move to the former directory                                        | `cd -`                                                                                      |
|                                                                      |                                                                                             |
| **Copying and linking files**                                       |                                                                                             |
| Copy a file                                                         | `cp path_to_source path_to_destination/`                                                   |
| Copy a directory                                                    | `cp -r path_to_source path_to_destination/`                                                |
| Create a symbolic link to a file                                    | `ln -s source_file symbolic_link`                                                          |
| Copy files from a remote server to your local computer              | `scp username@remote.host.com:~/path_to_file /path_to_destination/`                        |
|                                                                      |                                                                                             |
| **Creating and removing directories**                               |                                                                                             |
| Create a directory                                                  | `mkdir my_new_directory/`                                                                  |
| Remove a directory (including its subdirectories)                   | `rm -r my_new_directory/`                                                                  |


!!! warning 
    Be careful when using rm! Recovering directories and files in the event of accidental deletion may be impossible.

Show content of a file
Use the cat command to show the content of a file (use with caution when opening a large file – usually in combination with other commands, e.g. head):
cat my_data


Use the zcat command to show the content of a gzipped file: 
zcat my_data.gz 

Use the echo command:
echo "Hello World"
echo "Hello World" > my.output # print to file instead of screen
echo “Hello World\n” # print and change line (“\n” character)
echo "Hello\tWorld" # print with tab separator (“\t”) instead of space


Redirect and manipulate output
To transfer text stored in stdout to another command as stdin, use a pipe "|":
cat my_data | wc -l

To save text stored in stdout to a file, use a carrot ">":
cat my_data > my.output

To append text stored in stdout to the end of an existing (or new) file, use a double carrot ">>":
cat my_data2 >> my.output

Count the number of lines in a file
wc -l my.output

Print the first N (here 5) lines of a file
head -n 5 my.input
zcat my.input.gz | head -n 5 # see later for “pipe” usage

Print the last N (here 5) lines of a file
tail -n 5 my.input




Print file excluding first line (e.g. header)
tail +n 2 my.input > my.output

Sort the file on the Nth (here 5th) column (see further down for usage in context)
sort -k5,5 my.input
sort -k5,5g my.input # for numeric sorting (when multi-type data)

Print only unique rows and show their number of occurrences in a file (see further down for usage in context)
uniq -c my.output
cat my.output | sort | uniq -c # safer (see later for “pipe” usage)

Print all lines of a file matching a string
To extract all lines from an uncompressed file that match a string:
grep "my.search.string" my_data
grep -w "my.search.string" my_data # only match whole words
grep -f my.search.txt my_data # match all strings in *.txt file (per line)

*** grep is a very powerful tool, it's probably highly useful to get deeper insights into this classic unix program.
Stringing Multiple Commands Together (using the “pipe” character: | )
Take an input text file, search for all lines that match "cas_", and then count the number of lines that matched "cas_"
grep "cas_" my_data | wc -l

Isolate file basename, without path and extension (e.g. “.txt”)
echo $(basename -- "/path/to/my_file.ext") # get filename without path
# to assign to variable:
fullfile="/path/to/my_file.ext"
filename=$(basename -- "$fullfile")
echo ${filename##*.} # get filename without extension
echo ${filename%.*} # get extension only


Join two files by a matching column 
The “3” indicates the matching column, here is the third column in both files. “-1” and “-2” for the respective files containing this matching column  
join -1 3 -2 3 file1 file2 > combined_file


Loading Modules 
If you get a prompt like “command not found” this could be due to a module (e.g. PLINK) not being loaded
See https://userinfo.surfsara.nl/systems/shared/modules for a detailed overview of modules available on SURFsara’s LISA cluster
module load PLINK/1.9b_6.17-x86_64-GCC-8.3.0

Sometimes you may want to load a module which is only available in an older environment (e.g. LISA 2019 environment)
module load 2019





General AWK 

Simple awk command; print first, second, tenth and last column (“field”).
cat my_data | awk ‘{print $1,$2,$10,$NF}’ > my.output

Print conditionally: print line (all columns), only if…
#... 1st column is positive:
cat my_data | awk ‘if($1>0) {print $0}’ > my.output
#... 1st column is positive AND 2nd column is negative:
cat my_data | awk ‘if($1>0 && $2<0) {print $0}’ > my.output
#... 1st column OR 2nd column is positive:
cat my_data | awk ‘if($1>0 || $2>0) {print $0}’ > my.output
#... 1st column matches (equals) a string:
cat my_data | awk ‘if($1==”my.string”) {print $0}’ > my.output

Handy command for isolating and sorting / finding unique values of a specific column from a file:
cat my_data | awk ‘{print $1}’ | sort | uniq -c | wc -l

e.g. when working with (unique) identifiers, compare result with “cat my_data | wc -l”. Are the two line counts the same? (or are there duplicate IDs)

Remove string from a column/variable
awk '{ gsub(/any.string/,"", $2); print } ' my_data  > my.output


For sophisticated real-world usage also refers to aligning sum stats based on position and SNP names:
https://docs.google.com/document/d/1o4bN_uLK4IEItXCSdeQkXfZwEpLuSCWlveJevRogi08/edit#heading=h.4008addvumol
Set ACL Permissions (on SURFsaras LISA Cluster, specifically for the home directory)
nfs4_setfacl -a A::username@surf.nl:xrw ~

PLINK commands
e.g. https://www.cog-genomics.org/plink/1.9/filter 
Preimputation (QC)
Count number of cases and controls via FID column
grep -o 'con' my_file  | wc -l
grep -o 'cas' my_file  | wc -l

Align PLINK files e.g. before merging two datasets together 
Lift PLINK files to Human Genome Build 19 (HG19)
buigue --lift19 your_file_.bim

Align positions
checkpos6 --dbcol 1,2,3,4,5 --dbsnp path_to_reference/reference your_file.hg19.bim

Align alleles 
checkflip4 --dbcol 0,3,4,5 --fth 0.15 --sfh 0.2 --info path_to_reference/reference your_file.hg19.ch.bim

PCA
Imputation
HLA imputation 
Postimputation
Create custom forest plots 
forest_plot8 --meta daner_file.meta.gz --chr 1 --pos 31358836 --snp rs0000000 --out outname --danerdirs danerdirs


Create custom region/area plots 
area_plot_16_speed --prekno /home/pgcdac/DWFV2CJb8Piv_0116_pgc_data/HRC_reference.r1-1//pop_EUR/gwascatalog.Sep_2018.rp.txt --dandir dameta_directory --refdir /home/pgcdac/DWFV2CJb8Piv_0116_pgc_data/HRC_reference.r1-1//pop_EUR  --ngt-max -1 --title my_title --out my_outname --snp rs00000000 --area 6,32620900,32620936 --cols 2,11,1,3,-1,12 daner_file.gz


Check for duplicated SNPs in danerfile
zcat FILE | awk '{print $2}'| sort | uniq -D > mult_snp


Polygenic Risk Scoring
Clumping

clump_nav3 --noindel --pfile daner_file.gz --hq_f .05  --hq_i .9 --out my_outname --clu_p1 1.0 --clu_p2 1.0 --clu_window 500 --clu_r2 0.1 --refdir /home/pgcdac/DWFV2CJb8Piv_0116_pgc_data/HRC_reference.r1-1//pop_EUR


