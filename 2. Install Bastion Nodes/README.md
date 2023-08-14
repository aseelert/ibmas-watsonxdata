# Prepare the Bastion node for the installer

https://console-openshift-console.apps.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com
kubeadmin
JPyfX-wtYp5-2dp6s-X9N95

### Get the IBM API
Getting an api key to download the installation images.
https://myibm.ibm.com/products-services/containerlibrary?_gl=1*1yebie7*_ga_FYECCCS21D*MTY5MTk5NTI3MC4xMy4xLjE2OTE5OTU0MTIuMC4wLjA.

### Logon to the bastion node
Prepare the bastion node via SSH (details in the techzone reservation)
```
ssh admin@api.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com -p 40222
```

```
sudo su -
```
### Install required Redhat base software
install NFS and the screen software to provide NFS storage for Watsonx.data and allow access using the firewall service. The installation can be done with screen, as this is a kind of background service/terminal, no need to run commands with & and/or nohup
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

### Install the Cloudpak Installer CLI package
Check the latest version for the IBM Installer
https://github.com/IBM/cpd-cli/releases
```
wget https://github.com/IBM/cpd-cli/releases/download/v13.0.1/cpd-cli-linux-EE-13.0.1.tgz
tar -xzvf cpd-cli-linux-EE-13.0.1.tgz
mv cpd-cli-linux-EE-13.0.1-26/* .
rm -rf cpd-cli-linux-EE-13.0.1-26
rm -f cpd-cli-linux-EE-13.0.1.tgz
```

## Get the Redhat https API connection string
<img width="383" alt="image" src="https://media.github.ibm.com/user/50903/files/c69cc0b2-016d-4680-b0f0-8f34dbdcdbaa">
logon to your cluster Desktop/Console URL and check for the cluster informations


Set the data according to the Techzone Welcome Mail:
```
export SNO_API_URL=https://api.64d9cc7d49d91f0017b60a02.cloud.techzone.ibm.com:6443
export SNO_CLUSTER_ADMIN_PWD=jYaM7-YTSvR-cFY9P-LofjF
export SNO_IBM_ENTITLEMENT_KEY=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJJQk0gTWFya2V0cGxhY2UiLCJpYXQiOjE2MDY0NzEzNTksImp0aSI6IjkzNGY1ZjMxNTBjZjRiMjBhNTI0ZTA2MmJkZjNlNmRhIn0._4cHQE3w3iDhpKZocW0bL376zNG3ebzqYcJINNUUS7w
```

#### Update the hosts for to the AP connection string
```
export SNO_API_HOST=$(echo $SNO_API_URL | sed 's/https:\/\///g' | sed 's/:6443//g')
echo "192.168.252.1 $SNO_API_HOST" >> /etc/hosts
```

#### verify the openshif cluster login
```
oc login -u kubeadmin -p $SNO_CLUSTER_ADMIN_PWD $SNO_API_URL --insecure-skip-tls-verify
```

## Start using the Environment Setup
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


#### Enable the settings 
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

###### validate pod (optional): 
```
podman exec -ti ibm-lakehouse-manage-utils bash  
```

###### validate the login: 
```
/root/ibm-lh-manage/ibm-lakehouse-manage login-to-ocp \
--token=${OCP_TOKEN} \
--server=${OCP_URL}
```

## Install NFS Provisioner and create a storage class for Watsonx.data
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
