## 1. Install MinIO at Redhat

## 1.1 Introduction before deploy the MinIO setup
This guide provides a comprehensive walkthrough on setting up a Minio S3 cluster within the context of an OpenShift environment, specifically tailored for a Single Node OpenShift (SNO) deployment.

To successfully complete this setup, please ensure the following prerequisites are met:

1. You have logged in as the `kubeadmin` user to your OpenShift cluster. This user typically has the necessary permissions to perform administrative tasks.

2. You have the ability to pull container images from Dockerhub. This is crucial for fetching the required Minio container images.

3. You have a pre-configured storage class named `nfs-storage-provisioner`. This storage class will facilitate the provisioning of persistent storage for your Minio S3 cluster.

By following the steps outlined in this guide, you will be able to establish a functional Minio S3 cluster on your Single Node OpenShift instance. This cluster will enable you to store and manage objects within a distributed, scalable, and high-performance S3-compatible storage environment.

Please note that the successful completion of this setup will empower you to harness the benefits of Minio's robust object storage capabilities within your OpenShift environment.


### 2. Download the specific release
```pq linenums="1"
wget https://github.com/vmware-tanzu/velero/releases/download/v1.6.0/velero-v1.6.0-linux-amd64.tar.gz
```
### 3. Extract the software
```pq linenums="1"
tar xvzf velero-v1.6.0-linux-amd64.tar.gz
mv velero-v1.6.0-linux-amd64/* .
rm -rf velero-v1.6.0-linux-amd64
rm -rf velero-v1.6.0-linux-amd64.tar.gz
```

### 4. Apply the Deployment
```pq linenums="1"
oc apply -f examples/minio/00-minio-deployment.yaml
```

### 4.1 Patch the MinIO Deployment to a specific release
```pq linenums="1"
oc set image deployment/minio minio=minio/minio:RELEASE.2021-04-22T15-44-28Z -n velero
```

### 4.2 Create PVCs for MinIO config (1GB) and data (100GB)
```sql linenums="1"
oc apply -f - <<EOF
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  namespace: velero
  name: minio-config-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: nfs-storage-provisioner
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  namespace: velero
  name: minio-storage-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 100Gi
  storageClassName: nfs-storage-provisioner
EOF
```  
### 4.3 Apply the PVCs to the Deployment  
```sql linenums="1"
oc set volume deployment.apps/minio -n velero \
  --add \
  --overwrite \
  --name=config \
  --mount-path=/config \
  --type=persistentVolumeClaim \
  --claim-name=minio-config-pvc

oc set volume deployment.apps/minio -n velero \
  --add \
  --overwrite \
  --name=storage \
  --mount-path=/storage \
  --type=persistentVolumeClaim \
  --claim-name=minio-storage-pvc
```
### 4.4 Set ressource limits
```sql linenums="1"
oc set resources deployment minio -n velero --requests=cpu=500m,memory=256Mi --limits=cpu=1,memory=1Gi
```
### 4.5 Expose the service
```sql linenums="1"
oc patch svc minio -n velero --type=json -p '[{"op": "replace", "path": "/spec/ports/0/port", "value": 9077}, {"op": "replace", "path": "/spec/ports/0/name", "value": "ibmas-svc-9077"}]'
```
```sql linenums="1"
oc expose svc minio -n velero --port=ibmas-svc-9077 --name=minio-route --wildcard-policy=None
```
### 5. Test the connection and login
Before logging in, please note that this is a purely **HTTP** service. Most web browsers automatically redirect to **HTTPS**. During our testing, we found that Firefox works well with this setup. However, Safari and Chrome may automatically switch to the HTTPS protocol.

Keep these credentials handy for a smooth login experience.

```sql linenums="1"
hostname=$(oc get route minio-route -n velero -o=jsonpath='{.spec.host}')
access_key=$(oc get deployment minio -n velero -o=jsonpath='{.spec.template.spec.containers[0].env[?(@.name=="MINIO_ACCESS_KEY")].value}')
secret_key=$(oc get deployment minio -n velero -o=jsonpath='{.spec.template.spec.containers[0].env[?(@.name=="MINIO_SECRET_KEY")].value}')
echo "Hostname: $hostname, Accesskey: $access_key, Secretkey: $secret_key"
```
