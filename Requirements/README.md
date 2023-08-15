# Step 1 - Requirements

#### Navigation
[Next Chapter: Prepare the Installation](../Prepare%20the%20Installation) 

#### Techzone Business Partner Access Information
https://github.com/IBM/itz-support-public/blob/main/IBM-Technology-Zone/IBM-Technology-Zone-Runbooks/BusinessPartnersAccess.md

#### Main installation documenation:
watson x.data https://www.ibm.com/docs/en/watsonxdata/1.0.x?topic=software-getting-started

#### Getting an API key to download the installation images:
https://myibm.ibm.com/products-services/containerlibrary?_gl=1*1yebie7*_ga_FYECCCS21D*MTY5MTk5NTI3MC4xMy4xLjE2OTE5OTU0MTIuMC4wLjA.

#### Orginal and main information about SNO for Cloudpak for Data:
https://github.ibm.com/claus-huempel/cpd-sno/blob/main/techzone/index.md

## Important: 
- request a 32Core x 128GB Memory
- do not select FIPS (must be disabled)

### To start with the Installation you need to request a Single Node Cluster Instance at IBM Techzone:

- Go to https://techzone.ibm.com/my/reservations/create/6495f9f85c870e00179901fa
- Click Reserve now
- Select Purpose -> Practice / Self-Eduction
- Click the checkbox to confirm that no customer data is being used
- Leave the opportunity number empty
- Enter a description for the Purpose
- Choose the geography DAL12 (other geographies are WDC04, LON02, LON06, FRA04 and TOK02)
- Leave the defaults for "End date time", "OCP/Kubernetes Cluster Network" and "Enable FIPS Security"
- Choose the master single node flavor. Choose at least the 32x256 flavor
- Choose the OpenShift version 4.12
- Leave the defaults for "OCP/Kubernetes Service Network"
- Eventually add notes
- Click Submit


<img width="1071" alt="image" src="https://media.github.ibm.com/user/50903/files/ccd0fbbb-893a-44c6-858a-83e5bae8ff4b">


