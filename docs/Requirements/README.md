# Step 1 - Requirements for Watsonx.data Installation
##### Navigation
[Next Chapter: Prepare the Installation](../Prepare%20the%20Installation) 

Before proceeding with the installation of watsonx.data, it's important to ensure that your environment meets the necessary prerequisites. This section outlines the key requirements that need to be addressed to successfully deploy watsonx.data on your Red Hat cluster. These requirements lay the foundation for a smooth and efficient installation process, allowing you to leverage the capabilities of watsonx.data seamlessly.

In this section, we will cover essential aspects such as provisioning a Red Hat cluster on the TechZone platform, which serves as the underlying infrastructure for your watsonx.data deployment. Properly fulfilling these requirements will help create a robust foundation for your processing tasks using watsonx.data.

Let's begin by exploring the steps involved in meeting these prerequisites and preparing your environment for the installation of watsonx.data.

## 2. Summary for prerequisites

Before you begin the installation process, make sure you have the following prerequisites in place:

1. **IBMid Credentials:** Have your IBMid credentials ready to log in and access the necessary content on TechZone.

2. **TechZone Access:** Ensure access to [techzone.ibm.com](https://techzone.ibm.com), where you'll find essential information and resources related to the installation.

3. **IBM Entitlement Key:** Keep your IBM Entitlement Key handy, as it will be required during the installation for accessing IBM resources.

4. **Terminal or Putty:** Have a terminal session or Putty installed to execute commands and manage the installation.

5. **Chrome Browser:** Use Chrome as your preferred browser (avoid Safari on Mac) for optimal compatibility with the installation process.

With these prerequisites fulfilled, you'll be well-equipped to navigate through the installation process smoothly and efficiently.


## 3. Useful Links

### 3.1 Techzone Business Partner Access Information
[![Techzone Business Partner Access Information](https://img.shields.io/badge/Access-Documentation-blue)](https://github.com/IBM/itz-support-public/blob/main/IBM-Technology-Zone/IBM-Technology-Zone-Runbooks/BusinessPartnersAccess.md)

Learn about accessing Techzone as a Business Partner. Business Partners (BPs) Authenticated on IBM Partner Plus (PP) Have Access to IBM Technology Zone Using Their IBM Ids.

### 3.2 Main Installation Documentation: Watson x.data
[![Main Installation Documentation](https://img.shields.io/badge/Documentation-Getting%20Started-blue)](https://www.ibm.com/docs/en/watsonxdata/1.0.x?topic=software-getting-started)

IBM® watsonx.data is a new open architecture lakehouse that combines the elements of the data warehouse and data lakes. The best-in-class features and optimizations available on the watsonx.data make it an optimal choice for next generation data analytics and automation.

watsonx.data is a unique solution that allows co-existence of open source technologies and proprietary products. It offers a single platform where you can store the data or attach data sources for managing and analyzing your enterprise data.

Use watsonx.data to store any type of data (structured, semi-structured, and unstructured) and make that data accessible directly for Artificial Intelligence (AI) and Business Intelligence (BI). You can also attach your data sources to watsonx.data, which helps to reduce data duplication and cost of storing data in multiple places. It uses open data formats with APIs and machine learning libraries, making it easier for data scientists and data engineers to use the data. watsonx.data architecture enforces schema and data integrity, making it easier to implement robust data security and governance mechanisms.



### 3.3 Getting an IBM Entitlement Key to Download Installation Images
[![IBM Entitlement Key](https://img.shields.io/badge/Get%20API%20Key-IBM%20Container%20Library-blue)](https://myibm.ibm.com/products-services/containerlibrary?_gl=1*1yebie7*_ga_FYECCCS21D*MTY5MTk5NTI3MC4xMy4xLjE2OTE5OTU0MTIuMC4wLjA)

Obtain an API key to download installation images. Access your container software. With your entitlement key, you can access all of your container software in the IBM Entitled Registry. A complete list of container software that you own can be found in the Container Software Library.

Use any active entitlement key to log in to the image registry and retrieve any container software that you own.
— You can have a maximum of (5) entitlement keys.
— Once a key is deleted, it is no longer valid.

### 3.4 Original and Main Information about SNO for Cloudpak for Data
[![Original SNO Information](https://img.shields.io/badge/SNO%20Information-Read%20Here-green)](https://github.ibm.com/claus-huempel/cpd-sno/blob/main/techzone/index.md)

Access the original documentation about a Single Node Openshift Cluster for a Cloudpak for Data Installation.


## 4. Important: 
- request a 32Core x 128GB Memory
- do not select FIPS (must be disabled)

### 4.1 To start with the Installation you need to request a Single Node Cluster Instance at IBM Techzone:

- Go to (https://techzone.ibm.com/my/reservations/create/6495f9f85c870e00179901fa)
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

#### Navigation
[Next Chapter: Prepare the Installation](../Prepare%20the%20Installation) 


