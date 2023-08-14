#### Prepare the installation
start screen command, to execute the running steps in background, to avoid connectivity errors
  screen -S <name> creates a new shell
with 
  screen -r <name> you can always attach to the shell if you lost the shell
  using screen -list you can check for open shells
```
screen -S installwatsonxdata
```

#### login to the cluster
```
/root/ibm-lh-manage/ibm-lakehouse-manage/ibm-lakehouse-manage login-to-ocp \
--user=${OCP_USERNAME} \
--password=${OCP_PASSWORD} \
--server=${OCP_URL}
```
  
#### Set up the topology. Run the following commands:
```
/root/ibm-lh-manage/ibm-lakehouse-manage apply-cluster-components
```
```
/root/ibm-lh-manage/ibm-lakehouse-manage authorize-instance-topology
/root/ibm-lh-manage/ibm-lakehouse-manage setup-instance-topology
```

#### Run the following command to accept the license agreement:
```
/root/ibm-lh-manage/ibm-lakehouse-manage install --license_acceptance=true
```
#### Run the following command to verify whether the catalog source is created.
```
oc get catalogsource -n ${PROJECT_CPD_OPS}
oc get csv -n ${PROJECT_CPD_OPS}
```

#### Verify the CR status. Run the following command:
```
/root/ibm-lh-manage/ibm-lakehouse-manage get-cr-status


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
