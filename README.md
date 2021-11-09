# Cleanup unused Storage Volumes in IBM Cloud Accounts
Most of us are having challenges on viewing or clean-up unused/unattached Storage Volumes in our IBM Cloud accounts. These unused volumes will pile-up your Recurring Fees under "Unattached Services" in your invoice. 

Refer the below scripts to View or Delete unused/unattached Block or File Storage Volumes



## Login to IBM Cloud 
Download [ibmcloud cli](https://cloud.ibm.com/docs/cli?topic=cli-getting-started)

- ibmcloud login (or) ibmcloud login --sso
- Select an account



## View unused Storage Volumes
> Make sure you have required permissions to View Storage volumes
### Block Storage
```bash
for disk in $(ibmcloud sl block volume-list | awk -v var=" " '(NR>1) {print $1}' ); do count=0; 
for hosts in $(ibmcloud sl block access-list $disk --sortby id | awk -v var=" " '(NR>1) {print $1}' ); 
do count=$((count + 1)); done; if [[ ( $count -eq 0 )]]; then echo $disk; fi; done
```
### File Storage
```bash
for disk in $(ibmcloud sl file volume-list | awk -v var=" " '(NR>1) {print $1}' ); do count=0; 
for hosts in $(ibmcloud sl file access-list $disk --sortby id | awk -v var=" " '(NR>1) {print $1}' ); 
do count=$((count + 1)); done; if [[ ( $count -eq 0 )]]; then echo $disk; fi; done
```



## Delete unused Storage Volumes
> Make sure you have required permissions to Delete Storage volumes

### Block Storage
```bash
for disk in $(ibmcloud sl block volume-list | awk -v var=" " '(NR>1) {print $1}' ); do count=0; 
for hosts in $(ibmcloud sl block access-list $disk --sortby id | awk -v var=" " '(NR>1) {print $1}' ); 
do count=$((count + 1)); done; if [[ ( $count -eq 0 )]]; then echo $disk; ibmcloud sl block volume-cancel $disk --immediate -f; fi; done
```

### File Storage
```bash
for disk in $(ibmcloud sl file volume-list | awk -v var=" " '(NR>1) {print $1}' ); do count=0; 
for hosts in $(ibmcloud sl file access-list $disk --sortby id | awk -v var=" " '(NR>1) {print $1}' ); 
do count=$((count + 1)); done; if [[ ( $count -eq 0 )]]; then echo $disk; ibmcloud sl file volume-cancel $disk --immediate -f; fi; done
```
