### 1. Rebooting the OpenShift Master Node to Fix EOF `oc login` Problem
If you encounter an EOF (End of File) error while using the `oc login` command in your OpenShift environment, rebooting the master node can often resolve the issue. Follow these steps:
#### 1.2 Log In to Bastion Node with user root (not admin)
```py linenums="1"
ssh -i /tmp/ocp/cluster/id_rsa core@192.168.252.11
```
#### 1.3 Elevate to Root
```py linenums="1"
sudo su -
```
#### 1.4 Reboot the Master Node
```py linenums="1"
reboot
```


### 2. ibm-lakehouse-manage login-to-ocp login fails
The oc login command is working but the installer ibm-lakehouse-manage login-to-ocp fails with token expired. Please create a new token session as following:

#### 2.1 Validate the login
```py linenums="1"
oc login -u kubeadmin -p $SNO_CLUSTER_ADMIN_PWD $SNO_API_URL --insecure-skip-tls-verify
```
#### 2.2 Get the current Token
```py linenums="1"
export OCP_TOKEN="$(oc whoami -t)"
```
#### 2.3 Check the new login
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage login-to-ocp \
--token=${OCP_TOKEN} \
--server=${OCP_URL}
```
If the login with the token is not working, it is possible to execute the command with an user and password instead.

```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage login-to-ocp \
--user=${OCP_USERNAME} \
--password=${OCP_PASSWORD} \
--server=${OCP_URL}
