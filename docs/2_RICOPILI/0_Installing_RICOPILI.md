# Installing RICOPILI
**Source documents**: <br> [Ricopili @ Snellius](https://docs.google.com/document/d/1VL7j-gA7wW8VCvj3YmfRvR8Ny9651WE9EcbUYE1Xg7A/edit?tab=t.0#heading=h.i7fbl6xjsyub) | [RICOPILI custom installation](https://docs.google.com/document/d/14aa-oeT5hF541I8hHsDAL_42oyvlHRC5FWR7gir4xco/edit?tab=t.0#heading=h.clyzm24wfoeu) <br>
**Author**: <br> Alice Braun, M.Sc., B.A. [braun@broadinstitute.org](mailto:braun@broadinstitute.org)<br> 
***

## Downloading dependencies
If RICOPILI isn't centrally installed on your cluster (e.g. on SURFsnellius or Broad UGER) you'll need to download a dependency package via
```wget https://personal.broadinstitute.org/braun/sharing/Ricopili_Dependencies.1118b.tar.gz
wget https://personal.broadinstitute.org/braun/sharing/Ricopili_Dependencies.1118b.md5.cksum
# verify checksum
md5sum Ricopili_Dependencies.1118b.tar.gz
tar -xvzf Ricopili_Dependencies.1118b.tar.gz```

## On SURFsnellius
***
Download from GitHub via ssh from https://github.com/Ripkelab/ricopili

```
git clone git@github.com:Ripkelab/ricopili.git
mv ricopili/rp_bin/ ~
```
!!! Info "Running Git on SURFsnellius"
    If you you are unable download from Github you need to copy your ssh key to Github first. <br>
    `ssh-keygen -t ed25519`<br>
    When prompted, e.g. save in `/home/pgca1scz/.ssh/id_ed25519_github`<br>
    <br>
    Then run to add your SSH key to the agent:<br>
    `ssh-add .ssh/id_ed25519_github`<br>
    `eval "$(ssh-agent -s)`<br>
    <br>
    Now log into GitHub in the browser and navigate to Settings > SSH and GPG keys.<br>
    Click New SSH key and paste the contents of your public key file: `cat ~/.ssh/id_ed25519.pub`<br>
    <br>
    **Finally, verify the SSH Config file:<br>**
    `vim ~/.ssh/config` and paste the following:<br>
    *Host github.com <br>
    HostName github.com <br>
    User git <br>
    IdentityFile ~/.ssh/id_ed25519_github <br>*
    <br>
    Test the connection: `ssh -T git@github.com`


### RICOPILI configuration
***
On SURFsnellius you may use the centrally installed dependencies on PGC DAC: <br>
`/gpfs/work5/0/pgcdac/ricopili_download/dependencies/` <br>
To swiftly install RICOPILI you need to create a file called ricopili.conf e.g. in your home directory.
`vim ricopili.conf` and paste the following contents, replacing: `home, init, email, loloc` with your personal information. <br>

## SURFsnellius config file 
```bash
eloc /home/pgca1scz/.conda/envs/ricopili/bin/ # conda environment installation
i2loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/impute_v2
i4loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/impute_v4
hmloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/hapmap_ref/
minimac3loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/Minimac3/
minimac4loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/Minimac4/minimac4-4.1.2-Linux-x86_64/bin/ 
gmloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/genetic_map_files 
sh5loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/shapeit5 
plink2loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/plink2 
rloc module_SPACE_load_SPACE_2023;module_SPACE_load_SPACE_R/4.3.2-gfbf-2023a;_SPACE_R
ldsc_start /home/pgca1scz/.conda/envs/ricopili/bin/  # conda environment installation
sh3loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/shapeit3
tabixloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/tabix/
bcloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bcftools/bcftools-1.18
bcloc_plugins /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bcftools/bcftools-1.18/plugins/
ealoc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/eagle
bgziploc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bgzip/
ldsc_ref /gpfs/work5/0/pgcdac/ricopili_download/dependencies/ldsc
liloc /home/pgca1scz/rp_dependencies/liftover #local installation
rpac /home/pgca1scz/R/x86_64-pc-linux-gnu-library/4.3/  #local installation
p2loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/plink
shloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/shapeit
meloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/metal/
bcrloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bcftools/resources/
home /home/pgca1scz/
sloc /scratch-local
init ab
email braun@broadinstitute.org
loloc /home/pgca1scz/
batch_jobcommand sbatch
batch_name -J_SPACE_XXX
batch_jobfile XXX
batch_memory_request NONE
batch_walltime --time=0-HH:MM:SS
batch_array --array=1-XXX%YYY
batch_stdout -o_SPACE_XXX/%x-%j.out
batch_stderr -e_SPACE_XXX/%x-%j.out
batch_job_dependency --dependency=afterany:XXX
batch_array_task_id $SLURM_ARRAY_TASK_ID
batch_max_parallel_jobs_per_one_array added_to_array
batch_other_job_flags --partition=genoa_SPACE_--cpus-per-task=32 # changed to genoa from rome (thin) partition as default
batch_job_output_jid Submitted_SPACE_batch_SPACE_job_SPACE_XXX
batch_ncores_per_node 32
batch_mem_per_node 56
queue custom


```
Start the script rp_config. <br>
This is an interactive script that will take care of the installation in your computer cluster environment. The standard procedure is meanwhile custom configuration. 
If Ricopili is already installed in the system under your account, it will ask you if you wish to unset the Ricopili PATH settings first. For first time custom installation it is highly recommended to do so. The configuration script will give you the two commands you have to issue. You just need to copy/paste them into the command line. 

If the configuration script cannot find a configuration file (by default the script is looking for a file named rp_config.custom.txt) an empty file is created, that needs to be filled by you and/or a system-administrator with the knowledge gained in the previous chapters.

This file follows a two column structure, where variable-names are found in the first column and variable-values in the second. “###” means comments, everything after that is discarded.
Whitespace can be as long as necessary
Spaces are not allowed. Please use term `_SPACE_` if needed. <br>

To run the configuration on **SURFsnellius** you can copy paste the following into the `rp_config.custom.txt`, replacing `rp_user_initials, rp_user_email, rp_logfiles`:<br>
```bash

### for details please refer to https://docs.google.com/document/d/14aa-oeT5hF541I8hHsDAL_42oyvlHRC5FWR7gir4xco/edit?usp=sharing
###          and https://docs.google.com/spreadsheets/d/1LhNYIXhFi7yXBC17UkjI1KMzHhKYz0j2hwnJECBGZk4/edit?usp=sharing
variable_name                  variable_value
----------------------------------------------
rp_dependencies_dir  /gpfs/work5/0/pgcdac/ricopili_download/dependencies
R_packages_dir    /gpfs/work5/0/pgcdac/ricopili_download/dependencies/R_packages
starting_R        module_SPACE_load_SPACE_2023_SPACE_R/4.2.1-foss-2022a;_SPACE_R
path_to_Perlmodules  /gpfs/work5/0/pgcdac/ricopili_download/dependencies/perl_modules
path_to_scratchdir   /scratch-local
starting_ldsc  NA
ldsc_reference   /gpfs/work5/0/pgcdac/ricopili_download/dependencies/ldsc
rp_user_initials   <YOUR_INITIALS>
rp_user_email   <YOUR_MAIL>
rp_logfiles     <YOUR_HOME>
----------------------------------------
----------------------------------------
---- jobarray and queueing parameters:
----------------------------------------
----------------------------------------
batch_jobcommand sbatch
batch_memory_request NONE
batch_walltime --time=0-HH:MM:SS
batch_array --array=1-XXX%YYY
batch_max_parallel_jobs_per_one_array added_to_array
batch_jobfile XXX
batch_name -J_SPACE_XXX
batch_stdout -o_SPACE_XXX/%x-%j.out
batch_stderr -e_SPACE_XXX/%x-%j.out
batch_job_dependency --dependency=afterany:XXX
batch_array_task_id $SLURM_ARRAY_TASK_ID
batch_other_job_flags  --partition=thin_SPACE_--cpus-per-task=32
batch_job_output_jid Submitted_SPACE_batch_SPACE_job_SPACE_XXX
batch_ncores_per_node 32
batch_mem_per_node 56

```


```bash
./rp_config
```
Follow the instructions but do not replace the config file you have just pasted.

## Bug fixes on SURFsnellius
***
!!! warning
    Currently, the libgsl.so.23 dependency for EIGENSOFT is not available on SURFsnellius. <br>
    To fix eloc without conda try the following workaround trick: <br> 
    `ln -s /usr/lib64/libgsl.so.25/ libgsl.so.23` <br>
    `export LD_LIBRARY_PATH=/usr/lib64/`
    <br>
    <br>
    Alternatively you can install EIGENSOFT through conda: <br>
    `conda install bioconda::eigensoft`
    and add the following to your ricopili.conf  
    `eloc /home/$USER/.conda/envs/ricopili/bin/`


!!! warning

    Currently, you need to manually load texlive and GCC in order for several modules to run (**e.g. pcaer**):
    ```
    module load 2022
    module load texlive/20230313-GCC-11.3.0
    ``` 
