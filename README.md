- watson x.data https://www.ibm.com/docs/en/watsonxdata/1.0.x?topic=software-getting-started
- Go to https://techzone.ibm.com/my/reservations/create/6495f9f85c870e00179901fa
- Click Reserve now
- Select Purpose -> Practice / Self-Eduction
- Click the checkbox to confirm that no customer data is being used
- Leave the opportunity number empty
- Enter a description for the Purpose
- Choose the geography DAL12 (other geographies are WDC04, LON02, LON06, FRA04 and TOK02)
- Leave the defaults for "End date time", "OCP/Kubernetes Cluster Network" and "Enable FIPS Security"
- Choose the master single node flavor. Choose the 64x256 flavor
- Choose the OpenShift version 4.12
- Leave the defaults for "OCP/Kubernetes Service Network"
- Eventually add notes
- Click Submit




Getting an api key to download the installation images.
https://myibm.ibm.com/products-services/containerlibrary?_gl=1*1yebie7*_ga_FYECCCS21D*MTY5MTk5NTI3MC4xMy4xLjE2OTE5OTU0MTIuMC4wLjA.


https://github.ibm.com/claus-huempel/cpd-sno/blob/main/techzone/index.md



https://console-openshift-console.apps.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com
kubeadmin
JPyfX-wtYp5-2dp6s-X9N95

in case of EOF error:
```
ssh core@192.168.252.11 -i /tmp/ocp/cluster/id_rsa
sudo su -
shutdown -Fr now
```

Prepare the bastion node
ssh admin@api.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com -p 40222
jPQoDybh

```
sudo su -
```

Bastion Node:
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

Check the latest version for the IBM Installer
https://github.com/IBM/cpd-cli/releases
```
wget https://github.com/IBM/cpd-cli/releases/download/v13.0.1/cpd-cli-linux-EE-13.0.1.tgz
tar -xzvf cpd-cli-linux-EE-13.0.1.tgz
mv cpd-cli-linux-EE-13.0.0-9/* .
rm -rf cpd-cli-linux-EE-13.0.0-9
rm -f cpd-cli-linux-EE-13.0.1.tgz
```

Set the data according to the Techzone Welcome Mail:
```
export SNO_API_URL=https://api.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com:6443
export SNO_CLUSTER_ADMIN_PWD=JPyfX-wtYp5-2dp6s-X9N95
export SNO_IBM_ENTITLEMENT_KEY=\
eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJJQk0gTWFya2V0cGxhY2UiLCJpYXQiOjE2MDY0NzEzNTksImp0aSI6IjkzNGY1ZjMxNTBjZjRiMjBhNTI0ZTA2MmJkZjNlNmRhIn0._4cHQE3w3iDhpKZocW0bL376zNG3ebzqYcJINNUUS7w
```

```
export SNO_API_HOST=$(echo $SNO_API_URL | sed 's/https:\/\///g' | sed 's/:6443//g')
echo "192.168.252.1 $SNO_API_HOST" >> /etc/hosts
```

```
oc login -u kubeadmin -p $SNO_CLUSTER_ADMIN_PWD $SNO_API_URL --insecure-skip-tls-verify
```

```
mkdir /root/ibm-lh-manage
cd /root/ibm-lh-manage/
```

vi ibm-lh-manage.env
```
export SNO_API_URL=https://api.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com:6443
export SNO_CLUSTER_ADMIN_PWD=JPyfX-wtYp5-2dp6s-X9N95
export SNO_IBM_ENTITLEMENT_KEY=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJJQk0gTWFya2V0cGxhY2UiLCJpYXQiOjE2MDY0NzEzNTksImp0aSI6IjkzNGY1ZjMxNTBjZjRiMjBhNTI0ZTA2MmJkZjNlNmRhIn0._4cHQE3w3iDhpKZocW0bL376zNG3ebzqYcJINNUUS7w

export LH_IMAGE_TAG=latest
export DOCKER_EXE=podman
export UTILS_IMG=icr.io/cpopen/watsonx-data/ibm-lakehouse-manage-utils:$LH_IMAGE_TAG
export IBM_LH_TOOLBOX=icr.io/cpopen/watsonx-data/ibm-lakehouse-toolbox:$LH_IMAGE_TAG
export WORK_DIR=/root/ibm-lh-manage/.ibm-lh-manage-utils
export OCP_URL=https://api.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com:6443
export OPENSHIFT_TYPE=self-managed
export IMAGE_ARCH=x86-64
export PROJECT_CPD_OPS=cpd-operator
export PROJECT_CPD_INSTANCE=cpd-instance
export STG_CLASS_BLOCK=nfs-storage-provisioner
export STG_CLASS_FILE=nfs-storage-provisioner
export PROD_REGISTRY=cp.icr.io
export PROD_USER=cp
export IBM_ENTITLEMENT_KEY=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJJQk0gTWFya2V0cGxhY2UiLCJpYXQiOjE2MDY0NzEzNTksImp0aSI6IjkzNGY1ZjMxNTBjZjRiMjBhNTI0ZTA2MmJkZjNlNmRhIn0._4cHQE3w3iDhpKZocW0bL376zNG3ebzqYcJINNUUS7w

export OCP_USERNAME="kubeadmin"
export OCP_PASSWORD="$SNO_CLUSTER_ADMIN_PWD"
export OCP_TOKEN="$(oc whoami -t)"

export NFS_SERVER_LOCATION=192.168.252.2
export NFS_PATH=/export
export PROJECT_NFS_PROVISIONER=nfs-provisioner
export NFS_STORAGE_CLASS=nfs-storage-provisioner
export NFS_IMAGE=k8s.gcr.io/sig-storage/nfs-subdir-external-provisioner:v4.0.2
```



```
source /root/ibm-lh-manage/ibm-lh-manage.env
```

```
$DOCKER_EXE pull $IBM_LH_TOOLBOX
id=$($DOCKER_EXE create $IBM_LH_TOOLBOX)
$DOCKER_EXE cp $id:/opt - > /tmp/pkg.tar
$DOCKER_EXE rm $id
unset id
```

```
tar -xf /tmp/pkg.tar -C /tmp
cat /tmp/opt/bom.txt

cksum /tmp/opt/*/*

cp /tmp/opt/standalone/ibm-lakehouse-manage /root/ibm-lh-manage
cp /tmp/opt/standalone/README.txt /root/ibm-lh-manage
```

```
/root/ibm-lh-manage/ibm-lakehouse-manage initialize
```

validate pod (optional): 
```
podman exec -ti ibm-lakehouse-manage-utils bash  
```

```
/root/ibm-lh-manage/ibm-lakehouse-manage login-to-ocp \
--token=${OCP_TOKEN} \
--server=${OCP_URL}
```

```
./ibm-lakehouse-manage setup-nfs-provisioner \
--nfs_server=${NFS_SERVER_LOCATION} \
--nfs_path=${NFS_PATH} \
--nfs_provisioner_ns=${PROJECT_NFS_PROVISIONER} \
--nfs_storageclass_name=${NFS_STORAGE_CLASS} \
--nfs_provisioner_image=${NFS_IMAGE}
```

```
oc patch --type=merge --patch='{"spec":{"paused":true}}' machineconfigpool/master
oc patch --type=merge --patch='{"spec":{"paused":true}}' machineconfigpool/worker
```

```
/root/ibm-lh-manage/ibm-lakehouse-manage add-icr-cred-to-global-pull-secret --entitled_registry_key=${IBM_ENTITLEMENT_KEY}
```

```
oc patch --type=merge --patch='{"spec":{"paused":false}}' machineconfigpool/master
oc patch --type=merge --patch='{"spec":{"paused":false}}' machineconfigpool/worker
```

```
/root/ibm-lh-manage/ibm-lakehouse-manage apply-cluster-components
/root/ibm-lh-manage/ibm-lakehouse-manage authorize-instance-topology
/root/ibm-lh-manage/ibm-lakehouse-manage setup-instance-topology
/root/ibm-lh-manage/ibm-lakehouse-manage install --license_acceptance=true
/root/ibm-lh-manage/ibm-lakehouse-manage get-cr-status
```

```
oc get $(oc get Wxdaddon -o name -n ${PROJECT_CPD_INSTANCE}) -o custom-columns='VERSION:status.version,STATUS:status.wxdStatus,BUILD:.status.wxdBuildNumber' -n ${PROJECT_CPD_INSTANCE}

VERSION   STATUS      BUILD
1.0.1     Completed   IBM watsonx.data operator 1.0.1 build number v1.0.1-1054-20230721-214944-onprem-v1.0.1
```

```
[root@bastion-gym-lan ibm-lh-manage]# /root/ibm-lh-manage/ibm-lakehouse-manage get-cpd-instance-details
get-cpd-instance-details
CPD Url: cpd-cpd-instance.apps.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com
CPD Username: admin
CPD Password: dAThquPvKNc7
```

```
oc get routes -A | grep cpd | awk '{print "https://" $3}'
```
oc get nodes | grep -e "cpu  " -e "memory  "


https://cpd-cpd-instance.apps.64d4c88651ac6c0017670f2e.cloud.techzone.ibm.com/watsonx-data/#/?instanceId=1691681073886232&serviceInstanceNamespace=cpd-instance&serviceInstanceDisplayName=lakehouse
