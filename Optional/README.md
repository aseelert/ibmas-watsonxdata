## 1. Rebooting the OpenShift Master Node to Fix EOF `oc login` Problem
If you encounter an EOF (End of File) error while using the `oc login` command in your OpenShift environment, rebooting the master node can often resolve the issue. Follow these steps:
### 1.2 Log In to Bastion Node with user root (not admin)
```py linenums="1"
ssh -i /tmp/ocp/cluster/id_rsa core@192.168.252.11
```
### 1.3 Elevate to Root
```py linenums="1"
sudo su -
```
### 1.4 Reboot the Master Node
```py linenums="1"
reboot
```
