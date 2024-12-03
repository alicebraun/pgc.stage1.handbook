# Summary of PGC data intake
***
**Snellius server to YODA portal** <br>
**Source document**: [Summary of PGC data intake - Snellius server to YODA portal](https://docs.google.com/document/d/1VGSvFdKB4S-Tox3COG8II5Q340JXvrfE7RyQ9G9fByg/edit?tab=t.0) <br>
**Author**: Daniel P Howrigan [(howrigan@broadinstitute.org)](mailto:howrigan@broadinstitute.org)

## Snellius intake and upload summary

## STEP 1: Finding and joining the PGC via the web
***
- Navigate to the PGC website: [https://pgc.unc.edu/about-us/join-pgc/](https://pgc.unc.edu/about-us/join-pgc/) 
- Clicking on [https://pgcdataaccess.formstack.com/forms/pgc_data_inquiry](https://pgcdataaccess.formstack.com/forms/pgc_data_inquiry)

## STEP 2: Fill out data intake form phase 1
***
- Form submitted by data owner
- Received by DIO <br>
- Form sent to DRC representative <br>

Data inquiry (phase one, or form 1):
[https://pgcdataaccess.formstack.com/forms/pgc_data_inquiry](https://pgcdataaccess.formstack.com/forms/pgc_data_inquiry)


## STEP 3: Get approval from DRC representative
***
- Email communication between DIO and DRC representative
- Approval/rejection/additional info needed sent to data owner

## STEP 4: FIll out data intake form phase 2
***
- Form submitted by data owner
- Form sent to workgroup PGC DRC representative

Data intake (phase two, or form 2):
[https://pgcdataaccess.formstack.com/forms/pgc_data_intake](https://pgcdataaccess.formstack.com/forms/pgc_data_intake)

## STEP 5: Get approval from DRC representative
***
- Email sent to PGC DRC rep and  stage 1 analyst
- Email sent to data owner about getting a Snellius account and uploading data to Snellius
!!! YODA
    - Directory created in Yoda disease subfolder <br>
    - Access to subfolder granted by adding data owner email to subfolder

## STEP 6: Getting a Snellius account
***
- Fill out required forms and project
- Wait for approval
- Get IP cleared for access to server
!!! YODA
      - Data owner enters email address into login portal<br>
      - Prompted to create password<br>
      - Upon entry, data owner can view accessible folder <br>

## STEP 7: Uploading data to Snellius
***
Copy data to home directory
Change access permissions for S1 analyst
Email S1 analyst that data is ready<br>
!!! YODA
      - Data owner can upload files via GUI
      - Files will need to have metadata provided

## STEP 8: Data copied to DRC-specific working directory
***
S1 analyst copies all contents of the data directory to a DRC-specific data intake directory
DRC directory location example for MDD workgroup:
```
/gpfs/work5/0/
└── pgcdac
    └── DWFV2CJb8Piv_0116_pgc_data
        └── pgcdrc
            └── mdd
                └── incoming_datasets
                    └── data1

```
!!! YODA
      - Data owner submits the files to the “vault”, which retains the current state of the folder
      - The data manager gets notified that files have been submitted, and can accept or reject the submission