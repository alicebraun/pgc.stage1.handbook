# Installing RICOPILI
**Source documents**: [Ricopili @ Snellius](https://docs.google.com/document/d/1VL7j-gA7wW8VCvj3YmfRvR8Ny9651WE9EcbUYE1Xg7A/edit?tab=t.0#heading=h.i7fbl6xjsyub) | [RICOPILI custom installation](https://docs.google.com/document/d/14aa-oeT5hF541I8hHsDAL_42oyvlHRC5FWR7gir4xco/edit?tab=t.0#heading=h.clyzm24wfoeu) <br>
**Author**: Alice Braun, M.Sc., B.A. [braun@broadinstitute.org](mailto:braun@broadinstitute.org)<br> 
***

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
`vim ricopili.conf` and paste the following contents, replacing:<br>

```
home YOUR_HOME
init YOUR_INITIALS
email YOUR_EMAIL
loloc YOUR_HOME
```

!!! warning
    To fix eloc ln -s /usr/lib64/libgsl.so.25/ libgsl.so.23 <br>
    `ln -s /usr/lib64/libgsl.so.25/ libgsl.so.23` <br>
    `export LD_LIBRARY_PATH=`
    <br>
    <br>
    Alternatively you can install EIGENSOFT through conda: <br>
    `conda install bioconda::eigensoft`

```

!!! warning
Currently, you need to manually load texlive and GCC in order for several modules to run (e.g. pcaer):
```
module load 2022 <br>
module add texlive/20230313-GCC-11.3.0 

```

#eloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/eigensoft/mar_2023/EIG-master/bin/ 
eloc /home/pgca1scz/.conda/envs/ricopili/bin/
i2loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/impute_v2
i4loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/impute_v4
hmloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/hapmap_ref/
minimac3loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/Minimac3/
minimac4loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/Minimac4/minimac4-4.1.2-Linux-x86_64/bin/ 
bcloc_plugins /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bcftools/bcftools-1.18/plugins/  
sh5loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/shapeit5 
plink2loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/plink2 
rloc module_SPACE_load_SPACE_2023;module_SPACE_load_SPACE_R/4.3.2-gfbf-2023a;_SPACE_R
ldsc_start NA
sh3loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/shapeit3
tabixloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/tabix/
bcloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bcftools/bcftools-1.9_bin/
ealoc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/eagle
bgziploc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bgzip/
ldsc_ref /gpfs/work5/0/pgcdac/ricopili_download/dependencies/ldsc
liloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/liftover/
rpac NA
p2loc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/plink
shloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/shapeit
meloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/metal/
bcrloc /gpfs/work5/0/pgcdac/ricopili_download/dependencies/bcftools/resources/
sloc /scratch-local
home YOUR_HOME
init YOUR_INITIALS
email YOUR_EMAIL
loloc LOG_DIRECTORY # usually identical to YOUR_HOME
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
