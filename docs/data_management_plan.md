# Data management plan

## Data storage
### Storage location 
### Data Backup plan

### Data version control
Since the data is often way too large, it's not easy to upload it to places like GitHub for version control. But, with OneDrive, you have the ability to backup and recover previous verions of a file.  The main thing to remember here is not to delete files; instead, overwrite them. If you delete a file without a backup, you can't get back any previous versions. If you're not putting your files on other platforms, think about giving different versions of a file different names (check out the "Naming" section for more on this). It is a bit redundant to keep multiple versions therefore implement regular data cleanup [TODO add link to sections]. Best practice for our large datasets is to keep a backup of the code that can recreate an older data version. That way, you can rebuild your dataset if you need to, without exhausting the storage space.

### Data retention policy
- ***Raw data:*** Retained indefinitely on private storage space
- ***Published data:*** Retained indefinitely on dataverse.nl as per university guidlines
- ***Processed data:*** Kept for a minimum of 1 year and maximum of 5 years after publication of the paper.
- ***Metadata:***  Kept for a minimum of 1 year and maximum of 5 years after publication of the paper. Relevant metadata of published datasets are kept on dataverse.nl with the published data. 

### Data cleanup
As you work on your project you should regularly perform data cleanup to prevent workspace storage overload (especially when working on CODELABZETA or ALICE), ideally on a monthly basis. Data cleanup involves deleting files such as earlier dataset versions, backup copies, and temporary files that have fulfilled their purpose. If your research direction has shifted (as it often does), and certain data files no longer align with your objectives, remove them. Additionally, if you've identified duplicate files that contain identical data, process them in a way that you only keep one copy. 

For personal reference, you can maintain a record of deleted files in the "docs" folder, including instructions on how to recreate them if necessary. You can also create a backup of the deleted files and keep that for 30 days then automatically delete these backups. 

Do not delete files that are under shared access without everyone's consent. Be cautious when deleting files, especially if you're not entirely sure whether they are still needed. If there's any doubt, it's often safer to archive them rather than delete them permanently. Make sure the codes you need to recreate the delete files are still available. 

## Data organization
### Naming conventions:
- For each project, use the format `project_name_YYYY`. For example, `ERP_analysis_2023`. This name is used for your root folder but also your github repository and everywhere else you have to refer to the project. 
- Use descriptive and standardized names for data files even if its a very long name. In the filename you can already include information of the data. For instance for EEG data you can add the filter used on the dataset: `bandpass_1_30_ERP_smartphone.mat` this file has ERP data around smartphone touches bandpass filtered from 1 to 30 Hz. Avoid abbreviations you create yourself so write smartphone instead of sp because in the future you will likely forget and other collaborators may not understand the meaning. 
- For version control append version numbers or dates to file names with the format `vX_MM_YYYY` when updating For instance, `bandpass_1_30_ERP_smartphone.mat_v2_08_2024.mat`.

### Folders:
Create and keep your datasets in seperate folders. Always keep a copy of the raw data untouched. Then the processed intermediary datasets and published dataset should also be seperated. 
```
project_name_YYYY
|-- data : any data required to run the codes or data results from processed codes
|   |-- raw : save the unprocessed files if not in a common repository
|   |-- metadata : data about your dataset
|   |-- published : save the data for publication package
|   |-- processed : save your processed datasets here (results)
|   |   |-- figures : Save your result plots here
```
These folders can even be divided in more subfolders. For instance in processed you may have a folder for EEG datasets and another for processed smartphone data. 

#### Metadata 
In the metadata you can write information about what is stored in the files. 
Here is what to include: 
- ***Filename*** 
- ***Creator*** 
- ***Created Date***
- ***Last Modified Date***
- ***Description***

Here is an example template for the description of matlab data: 
```
# Event-related potential (ERP) data 
**Format**
Type: cell
- erp_data{indx,1} = participant ID
- erp_data{indx,3} = trimmed (20%) ERP 
- erp_data{indx,3} = number of trials 
```

## Data sharing 
Data ***may not*** be shared with anybody outside CODELAB unless specific permission has been granted. (Even if it is just for a friend to help you write some code).

If data is to be shared among CODELAB members then there are two methods: 
1. Use a shared CODELAB computer such as CODELABZETA. On CODELABZETA move the data to the following folder: `/media/Storage/Common_Data_Storage` This folder has data that can be used and access by any lab member. 
2. If you don't have access to any CODELAB computers, the second way is to upload the data on a sharepoint. Easiest way is to upload a folder with the files on Leiden University onedrive where you have a lot more storage space. Best practice is to share to the person(s) directly (by inviting them via their Leiden email) rather than creating a link. Make sure the person(s) has been given the right permission either _view_ or _edit_ access. If this is not possible then create a share link with an expiration data and limit the access to people in Leiden University. Additionally you can also block download if necessary. 

Do not upload the data via teams chat. 

## Data security
If you are dealing with sensitive user information and are using your personal
