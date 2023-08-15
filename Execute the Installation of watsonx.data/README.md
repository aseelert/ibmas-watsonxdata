# Step 3 - Install watsonx.data
##### Navigation
[Previous Chapter](../Prepare%20the%20Installation) or return to the [Introduction](../README.md).

in this comprehensive chapter, we will provide you with step-by-step instructions for successfully installing the IBM WatsonX Data application on your Red Hat Single Node Cluster (SNO). By the end of this chapter, you will have a clear understanding of how to set up and configure the WatsonX Data application on your cluster.

Building upon the introduction and prerequisites covered earlier, this installation guide will delve into the specific details necessary for a seamless and successful deployment. The focus will primarily be on the installation of the WatsonX Data application, ensuring that you have all the information you need to carry out the process efficiently.

By following the instructions outlined in this chapter, you will gain practical insights into provisioning the necessary resources, configuring the environment, and executing the installation process step by step. Our goal is to empower you with the knowledge and confidence to effectively set up the WatsonX Data application, enhancing your data-related capabilities within your Red Hat Single Node Cluster.

Let's dive into the installation process and embark on the journey to unleash the full potential of IBM WatsonX Data within your cluster environment.


## 3.1 Prepare the installation
The **screen** command is used to manage terminal sessions. To start a new session, use **screen -S session_name**, and to reattach to an existing session, use **screen -r session_name**. You can detach from a session by pressing **Ctrl-a followed by d**, and reattach using the reattach command. This enables you to run processes in the background, detach and reattach as needed.

  screen -S <name> creates a new shell
with 
  screen -r <name> you can always attach to the shell if you lost the shell
  using **screen -list** you can check for all open shells
```
screen -S installwatsonxdata
```

### 3.2 login to the cluster
```
/root/ibm-lh-manage/ibm-lakehouse-manage login-to-ocp \
--user=${OCP_USERNAME} \
--password=${OCP_PASSWORD} \
--server=${OCP_URL}
```
  
### 3.2.1 Set up the topology. Run the following commands:
```
/root/ibm-lh-manage/ibm-lakehouse-manage apply-cluster-components
```
```
/root/ibm-lh-manage/ibm-lakehouse-manage authorize-instance-topology
/root/ibm-lh-manage/ibm-lakehouse-manage setup-instance-topology
```

### 3.2.2 Run the following command to install Watsonx.data cartridge (CR) and accept the license agreement:
```
/root/ibm-lh-manage/ibm-lakehouse-manage install --license_acceptance=true
```
## 4. Run the following command to verify whether the catalog source is created.
```
oc get catalogsource -n ${PROJECT_CPD_OPS}
oc get csv -n ${PROJECT_CPD_OPS}
oc get po -n ibm-cert-manager
oc get po -n ibm-licensing
```

### 4.1 Verify the CR status. Run the following command:
```
/root/ibm-lh-manage/ibm-lakehouse-manage get-cr-status
```
```
# getting status for all installed components...  optionally add --components=<comma separated list of cpd components> for a specific set
# component,CR-kind,CR-name,status,version,creationtimestamp,reconciled-version,operator-info
watsonx_data,WxdAddon,wxdaddon,Completed,1.0.1,2023-08-14T15:17:36Z,1.0.1,IBM watsonx.data operator 1.0.1 build number v1.0.1-1054-20230721-214944-onprem-v1.0.1
analyticsengine,AnalyticsEngine,analyticsengine-sample,Completed,4.7.1,2023-08-14T14:52:23Z,4.7.1,268
cpd_platform,Ibmcpd,ibmcpd-cr,Completed,4.7.1,2023-08-14T14:07:05Z,--,cpdPlatform operator 4.1.0 build 10
zen,ZenService,lite-cr,Completed,5.0.0,2023-08-14T14:10:05Z,5.0.0,zen operator 5.0.0 build 277
```

```
oc get $(oc get Wxdaddon -o name -n ${PROJECT_CPD_INSTANCE}) -o custom-columns='VERSION:status.version,STATUS:status.wxdStatus,BUILD:.status.wxdBuildNumber' -n ${PROJECT_CPD_INSTANCE}
```
**Example**
```
VERSION   STATUS      BUILD
1.0.1     Completed   IBM watsonx.data operator 1.0.1 build number v1.0.1-1054-20230721-214944-onprem-v1.0.1
```
### 4.2 Get the Login credentials to access IBM Cockpit URL (ZEN)
```
/root/ibm-lh-manage/ibm-lakehouse-manage get-cpd-instance-details
```
**Example**
```
CPD Url: cpd-watsonxdata1-instance.apps.64da1ffc1bedbf00175f38c9.cloud.techzone.ibm.com
CPD Username: admin
CPD Password: 5SieL9rI6NFS
```
### 4.2.1 Get the watsonx.data external URL
```
oc get routes -A | grep cpd | awk '{print "https://" $3}'
```
### 5. Validate Memory
```
oc get nodes | grep -e "cpu  " -e "memory  "
```

##### Navigation
[Previous Chapter](../Prepare%20the%20Installation) or return to the [Introduction](../README.md).