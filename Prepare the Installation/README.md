# Step 2 - Prepare the installation of watsonx.data and the bastion node
[![Red Hat](https://img.shields.io/badge/Platform-Red%20Hat-red)](https://www.redhat.com)
[![CentOS](https://img.shields.io/badge/Platform-CentOS-yellow)](https://www.centos.org)
[![OpenShift](https://img.shields.io/badge/Platform-OpenShift-blue)](https://www.openshift.com)

In this chapter, we'll guide you through the Bastion Node for your Red Hat Single Node Cluster (SNO). A Red Hat Single Node Cluster (SNO) is a standalone instance of a Red Hat OpenShift cluster designed to run on a single node, providing a simplified environment for testing and development purposes. It encapsulates the core capabilities of OpenShift, enabling you to simulate a multi-node cluster experience on a single machine.

Now, let's delve into the concept of a bastion node. A bastion node, also known as a jump host or a gateway, serves as a secure entry point into a network or infrastructure. In the context of your SNO installation, the bastion node acts as an intermediary server that facilitates secure communication and management between your local machine and the Red Hat Single Node Cluster. It provides a controlled access point for performing administrative tasks, managing configurations, and executing commands within the cluster.

Throughout this chapter, we'll outline the steps required to set up and configure the bastion node within your Red Hat Single Node Cluster, enabling you to establish a secure and manageable environment for your development and testing activities.

#### Navigation
[Next Chapter: Execute the Installation of watsonx.data](../Execute%20the%20Installation%20of%20watsonx.data)  or return to the [Introduction](../README.md).


# Prepare the Bastion node for the installer

## Accessing Red Hat Single Node Cluster (SNO) Details

After successfully reserving a Red Hat Single Node Cluster (SNO) through TechZone, you can proceed to access important details related to your cluster setup. To retrieve these details, follow these steps:

1. **Go to:** Visit the TechZone platform at [https://techzone.ibm.com](https://techzone.ibm.com).

2. **Log In:** Log in to your TechZone account using your credentials.

3. **Access Reservations:** Navigate to the "My Reservations" section. You can usually find this in your account dashboard or a similar location.

4. **Select Current Image:** Locate and select your current Single Node OpenShift (VMware on IBM Cloud) image reservation from the list. This should correspond to the Red Hat Single Node Cluster you reserved.

5. **Retrieve Cluster Details:** Once you've selected your reservation, you should see a detailed view of your reserved SNO image. Look for information such as:

   - **User and Password:** Credentials to access your cluster. (user: kubeadmin)
   - **OpenShift Public URL:** The URL to access the OpenShift web console.
   - **Bastion Node Terminal (SSH) Details:** Information on how to SSH into the bastion node, which serves as a secure entry point into your cluster. 

6. **Accessing the Bastion Node:** Use the provided SSH details to connect to the bastion node. From the bastion node, you can perform administrative tasks, manage configurations, and execute commands within your SNO cluster. (user: admin and sudo su -)

By following these steps and retrieving the cluster details from the TechZone reservation page, you'll have the necessary information to effectively manage and work with your Red Hat Single Node Cluster environment.

#### Logon to the openshift console (example from reservation details)
**Example:**
```
https://console-openshift-console.apps.64da1ffc1bedbf00175f38c9.cloud.techzone.ibm.com

kubeadmin
zR6vy-FvZXh-IzfCn-SIG4x
```

#### Logon to the bastion node (example from reservation details)
Prepare the bastion node via SSH (details in the techzone reservation) 
**Example:**
```
ssh admin@api.64da1ffc1bedbf00175f38c9.cloud.techzone.ibm.com -p 40222
Password: yDVe43J8
```

#### switch from user admin to root via sudo
```
sudo su -
```
#### Install required Redhat base software
Install **NFS** and the **screen** software to set up NFS storage for watsonx.data, facilitating seamless access to storage resources while ensuring security through firewall services. This installation process can be conveniently executed using the screen utility, which operates as a background terminal service. This eliminates the need to append commands with & or utilize nohup, streamlining the configuration process.
```
yum -y install nfs-utils screen
systemctl enable --now nfs-server rpcbind
systemctl start nfs-server
mkdir /export
touch /etc/exports
echo "/export *(rw,sync,no_root_squash,no_all_squash)" >> /etc/exports
firewall-cmd --add-service=nfs --permanent
firewall-cmd --add-service={nfs3,mountd,rpc-bind} --permanent
firewall-cmd --reload
systemctl restart nfs-server
systemctl status nfs-server
```

#### Install the Cloudpak Installer CLI package
Stay up to date with the latest version of the IBM Installer by visiting the official GitHub repository at https://github.com/IBM/cpd-cli/releases. This ensures that you have access to the most current features and enhancements for a seamless installation experience.
```
wget https://github.com/IBM/cpd-cli/releases/download/v13.0.1/cpd-cli-linux-EE-13.0.1.tgz
tar -xzvf cpd-cli-linux-EE-13.0.1.tgz
mv cpd-cli-linux-EE-13.0.1-26/* .
rm -rf cpd-cli-linux-EE-13.0.1-26
rm -f cpd-cli-linux-EE-13.0.1.tgz
```

#### Get the IBM Entitlement Key 
[![IBM Entitlement Key ](https://img.shields.io/badge/Get%20IBM%20API%20Key-IBM%20Container%20Library-blue)](https://myibm.ibm.com/products-services/containerlibrary?_gl=1*1yebie7*_ga_FYECCCS21D*MTY5MTk5NTI3MC4xMy4xLjE2OTE5OTU0MTIuMC4wLjA)
Getting an API key to download the installation images. This API key will provide you access to the IBM Container Library where you can obtain the required installation images.
Click on the badge above or visit the [IBM Container Library](https://myibm.ibm.com/products-services/containerlibrary?_gl=1*1yebie7*_ga_FYECCCS21D*MTY5MTk5NTI3MC4xMy4xLjE2OTE5OTU0MTIuMC4wLjA) to obtain your API key.

#### Retrieve the API connection string for accessing OpenShift by obtaining the Red Hat HTTPS API URL.
<img width="383" alt="image" src="https://media.github.ibm.com/user/50903/files/c69cc0b2-016d-4680-b0f0-8f34dbdcdbaa">
logon to your cluster Desktop/Console URL and check for the cluster informations


#### Configure the data as specified in the TechZone reservation details:
```
export SNO_API_URL=https://api.64da1ffc1bedbf00175f38c9.cloud.techzone.ibm.com:6443
export SNO_CLUSTER_ADMIN_PWD=zR6vy-FvZXh-IzfCn-SIG4x
export SNO_IBM_ENTITLEMENT_KEY=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJJQk0gTWFya2V0cGxhY2UiLCJpYXQiOjE2MDY0NzEzNTksImp0aSI6IjkzNGY1ZjMxNTBjZjRiMjBhNTI0ZTA2MmJkZjNlNmRhIn0._4cHQE3w3iDhpKZocW0bL376zNG3ebzqYcJINNUUS7w
```

#### Update the hosts for to the API connection string
```
export SNO_API_HOST=$(echo $SNO_API_URL | sed 's/https:\/\///g' | sed 's/:6443//g')
echo "192.168.252.1 $SNO_API_HOST" >> /etc/hosts
```

#### Confirm the successful login to the OpenShift cluster.
```
oc login -u kubeadmin -p $SNO_CLUSTER_ADMIN_PWD $SNO_API_URL --insecure-skip-tls-verify
```

#### Initiate the Environment Setup using the Unix environment to configure the installation settings.
```
mkdir /root/ibm-lh-manage
cd /root/ibm-lh-manage/
```
```
tee ibm-lh-manage.env <<EOF 
# ------------------------------------------------------------------------------
# Watsonx.data  version
# ------------------------------------------------------------------------------
export LH_IMAGE_TAG=latest

# ------------------------------------------------------------------------------
# Watsonx.data settings
# ------------------------------------------------------------------------------
export DOCKER_EXE=podman
export UTILS_IMG=icr.io/cpopen/watsonx-data/ibm-lakehouse-manage-utils:\$LH_IMAGE_TAG
export IBM_LH_TOOLBOX=icr.io/cpopen/watsonx-data/ibm-lakehouse-toolbox:\$LH_IMAGE_TAG
export WORK_DIR=/root/ibm-lh-manage/.ibm-lh-manage-utils
export IMAGE_ARCH=x86-64
export PROD_REGISTRY=cp.icr.io
export PROD_USER=cp
# ------------------------------------------------------------------------------
# Projects
# ------------------------------------------------------------------------------
export PROJECT_CPD_OPS=watsonxdata1-operator
export PROJECT_CPD_INSTANCE=watsonxdata1-instance

# ------------------------------------------------------------------------------
# IBM Entitled Registry
# ------------------------------------------------------------------------------
export IBM_ENTITLEMENT_KEY=$SNO_IBM_ENTITLEMENT_KEY

# ------------------------------------------------------------------------------
# Cluster
# ------------------------------------------------------------------------------
export OCP_USERNAME="kubeadmin"
export OCP_PASSWORD=$SNO_CLUSTER_ADMIN_PWD
export OCP_TOKEN="$(oc whoami -t)"
export OCP_URL=$SNO_API_URL
export OPENSHIFT_TYPE=self-managed


# ------------------------------------------------------------------------------
# NFS Storage
# ------------------------------------------------------------------------------
export NFS_SERVER_LOCATION=192.168.252.2
export NFS_PATH=/export
export PROJECT_NFS_PROVISIONER=nfs-provisioner
export NFS_STORAGE_CLASS=nfs-storage-provisioner
export NFS_IMAGE=k8s.gcr.io/sig-storage/nfs-subdir-external-provisioner:v4.0.2
export STG_CLASS_BLOCK=nfs-storage-provisioner
export STG_CLASS_FILE=nfs-storage-provisioner
EOF
```


#### Activate the settings using the bash shell and source the environment.
```
source /root/ibm-lh-manage/ibm-lh-manage.env
echo "source /root/ibm-lh-manage/ibm-lh-manage.env" >> ~/.bashrc
```

#### Pull the watsonx.data ibm-lakehouse-toolbox image.
```
$DOCKER_EXE pull $IBM_LH_TOOLBOX
id=$($DOCKER_EXE create $IBM_LH_TOOLBOX)
$DOCKER_EXE cp $id:/opt - > /tmp/pkg.tar
$DOCKER_EXE rm $id
unset id
```

#### Extract the watsonx.data stand-alone pkg.tar file in to the /tmp directory. Verify that the checksum is correct by comparing the checksum
```
tar -xf /tmp/pkg.tar -C /tmp
cat /tmp/opt/bom.txt

cksum /tmp/opt/*/*

cp /tmp/opt/standalone/ibm-lakehouse-manage /root/ibm-lh-manage
cp /tmp/opt/standalone/README.txt /root/ibm-lh-manage
```

#### Run the following command to initialize the ibm-lh-manage-utils container.
```
/root/ibm-lh-manage/ibm-lakehouse-manage initialize
```

#### validate pod (optional): 
```
podman exec -ti ibm-lakehouse-manage-utils bash  
```

#### validate the login: 
```
/root/ibm-lh-manage/ibm-lakehouse-manage login-to-ocp \
--token=${OCP_TOKEN} \
--server=${OCP_URL}
```

#### Install NFS Provisioner and create a storage class for Watsonx.data
```
./ibm-lakehouse-manage setup-nfs-provisioner \
--nfs_server=${NFS_SERVER_LOCATION} \
--nfs_path=${NFS_PATH} \
--nfs_provisioner_ns=${PROJECT_NFS_PROVISIONER} \
--nfs_storageclass_name=${NFS_STORAGE_CLASS} \
--nfs_provisioner_image=${NFS_IMAGE}
```

#### Add the pull secret to the artifactory that contains watsonx.data images.
```
oc patch --type=merge --patch='{"spec":{"paused":true}}' machineconfigpool/master
oc patch --type=merge --patch='{"spec":{"paused":true}}' machineconfigpool/worker
```

```
/root/ibm-lh-manage/ibm-lakehouse-manage add-icr-cred-to-global-pull-secret --entitled_registry_key=${IBM_ENTITLEMENT_KEY}
```

When the pull secret is created, Red Hat OpenShift propagates it to every node that might take some time to complete. Therefore, wait until the UPDATED column displays True for all the worker nodes in the system config pool before you proceed to the next step.
```
watch oc get mcp
Every 2.0s: oc get mcp                                                                                                                                                                                               bastion-gym-lan: Mon Aug 14 05:08:17 2023

NAME     CONFIG                                             UPDATED   UPDATING   DEGRADED   MACHINECOUNT   READYMACHINECOUNT   UPDATEDMACHINECOUNT   DEGRADEDMACHINECOUNT   AGE
master   rendered-master-2df1fc6555c56bbafbc513f89eac366c   False     False	 False      1              0                   0                     0                      112m
worker   rendered-worker-5920c72cbaf105641bbd46b714c4c3ef   True      False	 False      0              0                   0                     0                      112m
```

```
oc patch --type=merge --patch='{"spec":{"paused":false}}' machineconfigpool/master
oc patch --type=merge --patch='{"spec":{"paused":false}}' machineconfigpool/worker
```
