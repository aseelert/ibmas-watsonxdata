## Rebooting the OpenShift Master Node to Fix EOF `oc login` Problem

If you encounter an EOF (End of File) error while using the `oc login` command in your OpenShift environment, rebooting the master node can often resolve the issue. Follow these steps:

```bash
# 1. Log In to Bastion Node with user root (not admin)
ssh -i /tmp/ocp/cluster/id_rsa core@192.168.252.11

# 2. Elevate to Root
sudo su -

# 3. Reboot the Master Node
reboot
