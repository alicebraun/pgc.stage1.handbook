# Snellius data upload
***
**Source document**: <br> 
[PGC Data Intake: 
Uploading data to the Snellius server](https://docs.google.com/document/d/1RuoCeLBPTzpFTMB9cHejgTK-qfEiyj-GG7hAXnkx6BE/edit?tab=t.0) <br>
**Authors**: <br>
Prof. Stephan Ripke, M.D., Ph.D., Group Leader, [ sripke@broadinstitute.org ](mailto:sripke@broadinstitute.org)<br>
Daniel Howrigan, Ph.D., Group Leader, [howrigan@broadinstitute.org](mailto:howrigan@broadinstitute.org)<br>
Vassily (Vasa) Trubetskoy, B.A., M.Sc, [vassily@broadinstitute.org](mailto:vassily@broadinstitute.org)<br>
Sarah Colbert, B.A., [sarah.colbert@icahn.mssm.edu](mailto:sarah.colbert@icahn.mssm.edu)<br>
***

## Uploading data to the Snellius server
***
**This document is shared with all investigators who are uploading data**<br>
`UPDATED August 2020 to use NFSv4 Access Control Lists for data access`

**Important**: 
If you have a Snellius account already you can skip step1 (get Snellius account). But it is highly recommended to still create a new subdirectory within your home directory (and copy your to-be-shared data there) and not share data from somewhere in your working directory structure. Some of these permission settings are difficult to revoke.<br>

GOAL: Securely upload your genetic data to the Snellius server, where all PGC data is being stored<br>

REQUIREMENT: A user account on the Snellius server<br>

## STEPS overview:
1. Get Snellius account <br>
2. Create data upload subdirectory on Snellius<br>
3. Upload genotype/intensity data files<br>
4. Change home directory permissions<br>
5. Change permissions in data upload directory<br>
6. Notify PGC collaborator of uploaded data subdirectory<br>

## STEP 1: Get Snellius account and access the server with scp access
***
If you do not already have a Snellius account, you will need to apply for a login account. Please follow the instructions in the [Obtaining a Snellius Account google doc](https://docs.google.com/document/d/11aXCcivo8PFsMCtf-QMAq5B0eCL4-RuMQft5DZt3Pug/edit?tab=t.0#heading=h.b4wl0z5ansba) to do so. 

Once you have an account, you can login via command line to the Snellius server. For the purpose of this documentation we always assume your account to be `user`. Assuming that you have already completed the above instructions, you should now be able to log in with full access using the host "snellius.surf.nl" like so:
```
ssh user@snellius.surf.nl
```

NOTE: the "doornode" host is not suitable for data upload/download using protocols such as SFTP, SCP, or Rsync (i.e. this is needed for uploading data!!). To have full access with SCP/SFTP and other protocols, you must use the above host after whitelisting your IP address as described in the “Obtaining a Snellius Account” document. 

## STEP 2: Create data upload subdirectory on Snellius
***
Create subdirectory in your home directory (in this example named ‘data_upload’):

```
mkdir -p ~/data_upload
``` 

**NOTE**: You can provide any name to the subdirectory, so long as you let the PGC stage 1 analyst know

## STEP 3: Upload genotype/intensity data files
***

Again, this is only possible, after you have registered your IP address (see above). On Doornode-access this would not work.
Prepare intensity files (Affy CEL or Illumina report files) according to the CNV data intake readme. Reach out to the CNV analyst (pgc.stage1.cnv@gmail.com) with any technical questions. pgc.stage1.cnv@gmail.com should have been copied on your initial correspondence with the DRC representative.

Then copy your dataset files to this subdirectory using scp (secure copy protocol):

```
scp files.to.copy user@snellius.surf.nl:~/data_upload
```


Importantly, the above command is not performed within the Snellius server, but from the computer where your data is currently stored.
## STEP 4: Change home directory permissions 
***
Make sure you are logged into your account on Snellius and in your home directory:  <br>

```
cd ~
```
Run the following command:

```
nfs4_setfacl -a A::pgca1xxx@snellius.surf.nl:x ~
``` 
Where pgca1xxx is the stage 1 analyst (xxx being the disease code) that will copy over data to the appropriate data receiving subdirectory. See Appendix for a description of what this command is doing. Importantly, this does not allow the user pgca1xxx to read any other data in your home directory, just traverse into it.

Here is the list of active pgca1 Snellius users:
``` 
pgca1scz 	Schizophrenia (uid: 50695)
pgca1tos	Tourette (uid: 50703)
pgca1mdd	Major Depression (uid: 50694)
pgca1pts	PTSD (uid: 50699)
pgca1anx	Anxiety Disorders (uid: 50698)
pgca1adh	ADHD (uid: 50697)
pgca1sud	Substance Use (uid: 50700)
pgca1asd	Autism (uid: 50696)
pgca1ocd	OCD (uid: 50701)
pgca1bip	Bipolar Disorder (uid: 50693)
pgca1cnv	CNV work group (uid: 51064)
pgca1sui 	Suicide (uid: 59585) 
pgca1fgen  Functional Genomics
```

## STEP 5: Change permissions in the data upload directory
***
Change in June 2022: not via ACL settings from here - 

Provide access to your data for “everyone”:
```
chmod -R o+rx ~/data_upload 
```

STEP 6: Notify PGC collaborator of uploaded data directory
Email the PGC stage 1 analyst that you've uploaded your genotype data to the Snellius server. Make sure to send the full directory path in your email. You can find this with the command `pwd` (you need to be in the folder that holds the files you want to share).

```
pwd
``` 

(this will be something like /home/user/data_upload)

After files are copied over by the PGC stage 1 analyst, you can remove the permissions for other users. 

```
chmod -R o-rx ~/data_upload
```
(You can also delete the files, although if we need to re-access the uploaded data for any reason, you'll need to go through the upload process again.)

General comment: we recommend doing so for all files in your home dir, otherwise some older ACL permitted logins might still have access to data in your home directory.
```
chmod -R o-rx ~/*
``` 
We recommend also removing the ACL permissions for your homedir. First read out the exact ACL-settings (otherwise removing will fail):

```
nfs4_getfacl ~
```
Keep this exact setting in mind when setting the next command, which should be similar to this one:

``` 
nfs4_setfacl -x A::pgca1xxx@surf.nl:rxtcy ~
```


Please check whether you successfully removed ACL permissions using the ‘nfs4_getfacl’ command (see Appendix). 


Please let us know if you have questions or run into any issues:  <br>
[sripke@broadinstitute.org](mailto:sripke@broadinstitute.org)<br>
[howrigan@broadinstitute.org](mailto:howrigan@broadinstitute.org)<br>
Special thanks to Manuel Mattheisen for help in adopting the ACL version of the data uploads.


# Appendix: Understanding NFSv4 ACL Commands
***
LISA switched to NFSv4 Access Control Lists (ACLs), allowing users to share files between accounts on the cluster. While newer and more powerful than the old permissions systems, they are more complicated. Here we explain what each ACL modification command does, so that the user understands what data they are exposing.

As of now we recommend using ACL settings only for homedirectories, since only here we are forced to use them. If you use a mix of ACL settings and classic permissions (via chmod) the outcome is not always predictable.

In order for a user to access data in a directory, several conditions need to be satisfied:

The user needs to be able to traverse into a directory.
The user needs to be able to read meta-data associated with the directory and all of its contents (meta-data such as filenames, size, permissions, etc.).
The user needs to be able to access the contents of files in the directory.


Our first command sets adds traversal permission for pgca1xxx user for the home directory of the uploading user:

```
nfs4_setfacl -a A::pgca1xxx@surf.nl:x ~
``` 
Each piece of the command can be read as follows:

`nfs4_setfacl` (program used to modify access control list)<br>
`-a` (flag specifying that we want to “add” something to the list)<br>
`A::pgca1xxx@surf.nl:x` (String saying “allow[A] user[pgca1xxx] on this server[@surf.nl] to execute [x]) <br>
`~` (The directory whose List we are modifying, here the home directory ~)

The result is that the user pgca1xxx is able to traverse into the uploading users home directory.

Deleted content (see below).

For your home directory:

```
nfs4_getfacl ~
```

As a result you should see something like the following on your screen:
``` 
A::OWNER@:rwaDxtTcCy
A::pgca1xxx@surf.nl:xtcy
A::GROUP@:tcy
A::EVERYONE@:tcy
```

The `A::pgca1xxx@surf.nl:xtcy` (or similar) entry should only exist when you added the stage 1 analyst to the access control list (i.e. have used the -a flag). The pgca1xxx represents the stage 1 analyst account name or the uid of the stage 1 analyst account on Snellius (usually a 5-digit number (see the list above)). If you can only see the lines containing ‘OWNER’, ‘GROUP’, and ‘EVERYONE’ this means you either successfully removed access for the stage 1 analyst again (i.e. have used the -x flag) or in case you intended to add access rights (i.e. have used the -a flag) something went wrong. In the latter case please contact your stage 1 analyst.
