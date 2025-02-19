# Installing RICOPILI
**Source documents**: <br> [Ricopili @ Snellius](https://docs.google.com/document/d/1VL7j-gA7wW8VCvj3YmfRvR8Ny9651WE9EcbUYE1Xg7A/edit?tab=t.0#heading=h.i7fbl6xjsyub) | [RICOPILI custom installation](https://docs.google.com/document/d/14aa-oeT5hF541I8hHsDAL_42oyvlHRC5FWR7gir4xco/edit?tab=t.0#heading=h.clyzm24wfoeu) <br>
**Author**: <br> Alice Braun, M.Sc., B.A. [braun@broadinstitute.org](mailto:braun@broadinstitute.org)<br> 
***

## Downloading dependencies
If RICOPILI isn't centrally installed on your cluster (e.g. on SURFsnellius or Broad UGER) you'll need to download a dependency package via
```
wget https://personal.broadinstitute.org/braun/sharing/Ricopili_Dependencies.1118b.tar.gz
wget https://personal.broadinstitute.org/braun/sharing/Ricopili_Dependencies.1118b.md5.cksum
# verify checksum
md5sum Ricopili_Dependencies.1118b.tar.gz
tar -xvzf Ricopili_Dependencies.1118b.tar.gz
```

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

## Conda environment .yaml file
```bash
name: rp_env
channels:
  - bioconda
  - conda-forge
dependencies:
  - _libgcc_mutex=0.1=conda_forge
  - _openmp_mutex=4.5=2_gnu
  - _r-mutex=1.0.1=anacondar_1
  - binutils_impl_linux-64=2.43=h4bf12b8_2
  - bwidget=1.10.1=ha770c72_0
  - bzip2=1.0.8=h4bc722e_7
  - c-ares=1.34.4=hb9d3cd8_0
  - ca-certificates=2025.1.31=hbcca054_0
  - cairo=1.18.2=h3394656_1
  - curl=8.11.1=h332b0f4_0
  - eigensoft=8.0.0=h75d7a4a_6
  - font-ttf-dejavu-sans-mono=2.37=hab24e00_0
  - font-ttf-inconsolata=3.000=h77eed37_0
  - font-ttf-source-code-pro=2.038=h77eed37_0
  - font-ttf-ubuntu=0.83=h77eed37_3
  - fontconfig=2.15.0=h7e30c49_1
  - fonts-conda-ecosystem=1=0
  - fonts-conda-forge=1=0
  - freetype=2.12.1=h267a509_2
  - fribidi=1.0.10=h36c2ea0_0
  - gcc_impl_linux-64=14.2.0=h6b349bd_1
  - gfortran_impl_linux-64=14.2.0=hc73f493_1
  - gmp=6.3.0=hac33072_2
  - graphite2=1.3.13=h59595ed_1003
  - gsl=2.7=he838d99_0
  - gxx_impl_linux-64=14.2.0=h2c03514_1
  - harfbuzz=9.0.0=hda332d3_1
  - icu=75.1=he02047a_0
  - kernel-headers_linux-64=3.10.0=he073ed8_18
  - keyutils=1.6.1=h166bdaf_0
  - krb5=1.21.3=h659f571_0
  - ld_impl_linux-64=2.43=h712a8e2_2
  - lerc=4.0.0=h27087fc_0
  - libblas=3.9.0=26_linux64_openblas
  - libcblas=3.9.0=26_linux64_openblas
  - libcurl=8.11.1=h332b0f4_0
  - libdeflate=1.23=h4ddbbb0_0
  - libedit=3.1.20240808=pl5321h7949ede_0
  - libev=4.33=hd590300_2
  - libexpat=2.6.4=h5888daf_0
  - libffi=3.4.2=h7f98852_5
  - libgcc=14.2.0=h77fa898_1
  - libgcc-devel_linux-64=14.2.0=h41c2201_101
  - libgcc-ng=14.2.0=h69a702a_1
  - libgfortran=14.2.0=h69a702a_1
  - libgfortran-ng=14.2.0=h69a702a_1
  - libgfortran5=14.2.0=hd5240d6_1
  - libglib=2.82.2=h2ff4ddf_1
  - libgomp=14.2.0=h77fa898_1
  - libiconv=1.17=hd590300_2
  - libjpeg-turbo=3.0.0=hd590300_1
  - liblapack=3.9.0=26_linux64_openblas
  - liblzma=5.6.3=hb9d3cd8_1
  - libnghttp2=1.64.0=h161d5f1_0
  - libopenblas=0.3.28=pthreads_h94d23a6_1
  - libpng=1.6.45=h943b412_0
  - libsanitizer=14.2.0=h2a3dede_1
  - libssh2=1.11.1=hf672d98_0
  - libstdcxx=14.2.0=hc0a3c3a_1
  - libstdcxx-devel_linux-64=14.2.0=h41c2201_101
  - libstdcxx-ng=14.2.0=h4852527_1
  - libtiff=4.7.0=hd9ff511_3
  - libuuid=2.38.1=h0b41bf4_0
  - libwebp-base=1.5.0=h851e524_0
  - libxcb=1.17.0=h8a09558_0
  - libxcrypt=4.4.36=hd590300_1
  - libzlib=1.3.1=hb9d3cd8_2
  - make=4.4.1=hb9d3cd8_2
  - mpfr=4.2.1=h90cbb55_3
  - ncurses=6.5=h2d0b736_2
  - openblas=0.3.28=pthreads_h6ec200e_1
  - openssl=3.4.1=h7b32b05_0
  - pandoc=3.6.2=ha770c72_0
  - pango=1.54.0=h3a902e7_3
  - pcre2=10.44=hba22ea6_2
  - perl=5.32.1=7_hd590300_perl5
  - pixman=0.44.2=h29eaf8c_0
  - pthread-stubs=0.4=hb9d3cd8_1002
  - r-backports=1.5.0=r44hb1dbf0f_1
  - r-base=4.4.2=hc737e89_2
  - r-base64enc=0.1_3=r44hb1dbf0f_1007
  - r-bslib=0.9.0=r44hc72bb7e_0
  - r-cachem=1.1.0=r44hb1dbf0f_1
  - r-caret=6.0_94=r44hdb488b9_2
  - r-checkmate=2.3.2=r44hdb488b9_0
  - r-class=7.3_23=r44h2b5f3a1_0
  - r-cli=3.6.3=r44h0d4f4ea_1
  - r-clock=0.7.1=r44h0d4f4ea_0
  - r-cluster=2.1.8=r44hb67ce94_0
  - r-codetools=0.2_20=r44hc72bb7e_1
  - r-colorspace=2.1_1=r44hdb488b9_0
  - r-conquer=1.3.3=r44hb424bfc_4
  - r-cpp11=0.5.1=r44hc72bb7e_0
  - r-crayon=1.5.3=r44hc72bb7e_1
  - r-data.table=1.16.4=r44he23165d_0
  - r-deldir=2.0_4=r44hbcb9c34_1
  - r-diagram=1.6.5=r44ha770c72_3
  - r-digest=0.6.37=r44h0d4f4ea_0
  - r-dplyr=1.1.4=r44h0d4f4ea_1
  - r-e1071=1.7_16=r44h93ab643_0
  - r-ellipsis=0.3.2=r44hb1dbf0f_3
  - r-evaluate=1.0.3=r44hc72bb7e_0
  - r-fansi=1.0.6=r44hb1dbf0f_1
  - r-farver=2.1.2=r44ha18555a_1
  - r-fastmap=1.2.0=r44ha18555a_1
  - r-fontawesome=0.5.3=r44hc72bb7e_0
  - r-foreach=1.5.2=r44hc72bb7e_3
  - r-foreign=0.8_88=r44h2b5f3a1_0
  - r-formula=1.2_5=r44hc72bb7e_2
  - r-fs=1.6.5=r44h93ab643_0
  - r-future=1.34.0=r44h785f33e_0
  - r-future.apply=1.11.3=r44hc72bb7e_0
  - r-generics=0.1.3=r44hc72bb7e_3
  - r-ggplot2=3.5.1=r44hc72bb7e_1
  - r-globals=0.16.3=r44hc72bb7e_1
  - r-glue=1.8.0=r44h2b5f3a1_0
  - r-gower=1.0.1=r44hb1dbf0f_2
  - r-gridextra=2.3=r44hc72bb7e_1006
  - r-gtable=0.3.6=r44hc72bb7e_0
  - r-hardhat=1.4.1=r44hc72bb7e_0
  - r-highr=0.11=r44hc72bb7e_1
  - r-hmisc=5.2_2=r44hb67ce94_0
  - r-htmltable=2.4.3=r44hc72bb7e_0
  - r-htmltools=0.5.8.1=r44ha18555a_1
  - r-htmlwidgets=1.6.4=r44h785f33e_3
  - r-interp=1.1_6=r44ha936806_1
  - r-ipred=0.9_15=r44hdb488b9_1
  - r-isoband=0.2.7=r44ha18555a_3
  - r-iterators=1.0.14=r44hc72bb7e_3
  - r-jpeg=0.1_10=r44had62be2_6
  - r-jquerylib=0.1.4=r44hc72bb7e_3
  - r-jsonlite=1.8.9=r44h2b5f3a1_0
  - r-kernsmooth=2.23_26=r44h8461fee_0
  - r-knitr=1.49=r44hc72bb7e_0
  - r-labeling=0.4.3=r44hc72bb7e_1
  - r-lattice=0.22_6=r44hb1dbf0f_1
  - r-latticeextra=0.6_30=r44hc72bb7e_3
  - r-lava=1.8.1=r44hc72bb7e_0
  - r-lifecycle=1.0.4=r44hc72bb7e_1
  - r-listenv=0.9.1=r44hc72bb7e_1
  - r-lubridate=1.9.4=r44h2b5f3a1_0
  - r-magrittr=2.0.3=r44hb1dbf0f_3
  - r-mass=7.3_64=r44h2b5f3a1_0
  - r-matrix=1.7_2=r44h2ae2be5_0
  - r-matrixmodels=0.5_3=r44hc72bb7e_1
  - r-matrixstats=1.5.0=r44h2b5f3a1_0
  - r-memoise=2.0.1=r44hc72bb7e_3
  - r-mgcv=1.9_1=r44h0d28552_1
  - r-mime=0.12=r44hb1dbf0f_3
  - r-modelmetrics=1.2.2.2=r44h0d4f4ea_4
  - r-multcomp=1.4_28=r44hc72bb7e_0
  - r-munsell=0.5.1=r44hc72bb7e_1
  - r-mvtnorm=1.3_3=r44h9ad1c49_0
  - r-nlme=3.1_167=r44hb67ce94_0
  - r-nnet=7.3_20=r44h2b5f3a1_0
  - r-numderiv=2016.8_1.1=r44hc72bb7e_6
  - r-parallelly=1.42.0=r44h2b5f3a1_0
  - r-pillar=1.10.1=r44hc72bb7e_0
  - r-pkgconfig=2.0.3=r44hc72bb7e_4
  - r-plyr=1.8.9=r44ha18555a_1
  - r-png=0.1_8=r44h21f035c_2
  - r-polspline=1.1.25=r44h8461fee_2
  - r-proc=1.18.5=r44ha18555a_1
  - r-prodlim=2024.06.25=r44h0d4f4ea_1
  - r-progressr=0.15.1=r44hc72bb7e_0
  - r-proxy=0.4_27=r44hb1dbf0f_3
  - r-purrr=1.0.2=r44hdb488b9_1
  - r-quantreg=6.00=r44h012206f_0
  - r-r6=2.6.0=r44hc72bb7e_0
  - r-rappdirs=0.3.3=r44hb1dbf0f_3
  - r-rcolorbrewer=1.1_3=r44h785f33e_3
  - r-rcpp=1.0.14=r44h93ab643_0
  - r-rcpparmadillo=14.2.3_1=r44hc2d650c_0
  - r-rcppeigen=0.3.4.0.2=r44hb79369c_0
  - r-recipes=1.1.1=r44hc72bb7e_0
  - r-reshape2=1.4.4=r44h0d4f4ea_4
  - r-rlang=1.1.5=r44h93ab643_0
  - r-rmarkdown=2.29=r44hc72bb7e_0
  - r-rmeta=3.0=r44hc72bb7e_1006
  - r-rms=7.0_0=r44hb67ce94_0
  - r-rpart=4.1.24=r44h2b5f3a1_0
  - r-rstudioapi=0.17.1=r44hc72bb7e_0
  - r-sandwich=3.1_1=r44hc72bb7e_0
  - r-sass=0.4.9=r44ha18555a_1
  - r-scales=1.3.0=r44hc72bb7e_1
  - r-shape=1.4.6.1=r44ha770c72_1
  - r-sparsem=1.84_2=r44hc4980d5_0
  - r-sparsevctrs=0.2.0=r44h2b5f3a1_1
  - r-squarem=2021.1=r44hc72bb7e_3
  - r-stringi=1.8.4=r44h33cde33_3
  - r-stringr=1.5.1=r44h785f33e_1
  - r-survival=3.8_3=r44h2b5f3a1_0
  - r-th.data=1.1_3=r44hc72bb7e_0
  - r-tibble=3.2.1=r44hdb488b9_3
  - r-tidyr=1.3.1=r44h0d4f4ea_1
  - r-tidyselect=1.2.1=r44hc72bb7e_1
  - r-timechange=0.3.0=r44ha18555a_1
  - r-timedate=4041.110=r44hc72bb7e_0
  - r-tinytex=0.54=r44hc72bb7e_0
  - r-tzdb=0.4.0=r44ha18555a_2
  - r-utf8=1.2.4=r44hb1dbf0f_1
  - r-vctrs=0.6.5=r44h0d4f4ea_1
  - r-viridis=0.6.5=r44hc72bb7e_1
  - r-viridislite=0.4.2=r44hc72bb7e_2
  - r-withr=3.0.2=r44hc72bb7e_0
  - r-xfun=0.50=r44h93ab643_0
  - r-yaml=2.3.10=r44hdb488b9_0
  - r-zoo=1.8_12=r44hb1dbf0f_2
  - readline=8.2=h8228510_1
  - sed=4.8=he412f7d_0
  - sysroot_linux-64=2.17=h0157908_18
  - texlive-core=20230313=he8f7729_15
  - tk=8.6.13=noxft_h4845f30_101
  - tktable=2.10=h8bc8fbc_6
  - tzdata=2025a=h78e105d_0
  - xorg-libice=1.1.2=hb9d3cd8_0
  - xorg-libsm=1.2.5=he73a12e_0
  - xorg-libx11=1.8.10=h4f16b4b_1
  - xorg-libxau=1.0.12=hb9d3cd8_0
  - xorg-libxdmcp=1.1.5=hb9d3cd8_0
  - xorg-libxext=1.3.6=hb9d3cd8_0
  - xorg-libxrender=0.9.12=hb9d3cd8_0
  - xorg-libxt=1.3.1=hb9d3cd8_0
  - zstd=1.5.6=ha6fb4c9_0
prefix: /home/$USER/.conda/envs/rp_env

```

## Know issues and bug fixes on SURFsnellius
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

    Currently, you need to manually load texlive and GCC in order for several modules to run (**e.g. pcaer**) <br>
    You can also simply add it to your .bashrc file:
    ```
    module load 2022
    module load texlive/20230313-GCC-11.3.0
    ``` 
