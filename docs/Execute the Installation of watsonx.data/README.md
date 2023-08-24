# Step 3 - Install watsonx.data
##### Navigation
[Previous Chapter](../Prepare%20the%20Installation) or return to the [Introduction](../README.md).

in this comprehensive chapter, we will provide you with step-by-step instructions for successfully installing the IBM WatsonX Data application on your Red Hat Single Node Cluster (SNO). By the end of this chapter, you will have a clear understanding of how to set up and configure the WatsonX Data application on your cluster.

Building upon the introduction and prerequisites covered earlier, this installation guide will delve into the specific details necessary for a seamless and successful deployment. The focus will primarily be on the installation of the WatsonX Data application, ensuring that you have all the information you need to carry out the process efficiently.

By following the instructions outlined in this chapter, you will gain practical insights into provisioning the necessary resources, configuring the environment, and executing the installation process step by step. Our goal is to empower you with the knowledge and confidence to effectively set up the WatsonX Data application, enhancing your data-related capabilities within your Red Hat Single Node Cluster.

Let's dive into the installation process and embark on the journey to unleash the full potential of IBM WatsonX Data within your cluster environment.


## 3.1 Prepare the installation
[![Linux](https://img.shields.io/badge/Linux-Command-Line-blue)
The **screen** command is used to manage terminal sessions. To start a new session, use **screen -S session_name**, and to reattach to an existing session, use **screen -r session_name**. You can detach from a session by pressing **Ctrl-a followed by d**, and reattach using the reattach command. This enables you to run processes in the background, detach and reattach as needed.

**Example** for this Demo:
```py linenums="1"
screen -S installwatsonxdata
```

**Details:**
```py linenums="1"
  screen -S <name> creates a new shell
```
with 
```py linenums="1"
  screen -r <name> you can always attach to the shell if you lost the shell
  using **screen -list** you can check for all open shells
```


### 3.2 login to the cluster
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage login-to-ocp \
--user=${OCP_USERNAME} \
--password=${OCP_PASSWORD} \
--server=${OCP_URL}
```
  
### 3.2.1 Set up the topology. Run the following commands:
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage apply-cluster-components
```
**Runtime:** about 1-2min
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage authorize-instance-topology
```
**Runtime:** about 1-2min
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage setup-instance-topology
```
**Runtime:** about 5min


### 3.2.2 Run the following command to install Watsonx.data cartridge (CR) and accept the license agreement:
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage install --license_acceptance=true
```
**Runtime:** about 2,5h
## 3.3 Run the following command to verify whether the catalog source is created.
```py linenums="1"
oc get catalogsource -n ${PROJECT_CPD_OPS}
oc get csv -n ${PROJECT_CPD_OPS}
oc get po -n ibm-cert-manager
oc get po -n ibm-licensing
```

### 3.3.1 Verify the CR status. Run the following command:
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage get-cr-status
```
```py linenums="1"
# getting status for all installed components...  optionally add --components=<comma separated list of cpd components> for a specific set
# component,CR-kind,CR-name,status,version,creationtimestamp,reconciled-version,operator-info
watsonx_data,WxdAddon,wxdaddon,Completed,1.0.1,2023-08-14T15:17:36Z,1.0.1,IBM watsonx.data operator 1.0.1 build number v1.0.1-1054-20230721-214944-onprem-v1.0.1
analyticsengine,AnalyticsEngine,analyticsengine-sample,Completed,4.7.1,2023-08-14T14:52:23Z,4.7.1,268
cpd_platform,Ibmcpd,ibmcpd-cr,Completed,4.7.1,2023-08-14T14:07:05Z,--,cpdPlatform operator 4.1.0 build 10
zen,ZenService,lite-cr,Completed,5.0.0,2023-08-14T14:10:05Z,5.0.0,zen operator 5.0.0 build 277
```

```py linenums="1"
oc get $(oc get Wxdaddon -o name -n ${PROJECT_CPD_INSTANCE}) -o custom-columns='VERSION:status.version,STATUS:status.wxdStatus,BUILD:.status.wxdBuildNumber' -n ${PROJECT_CPD_INSTANCE}
```
**Example**
```py linenums="1"
VERSION   STATUS      BUILD
1.0.1     Completed   IBM watsonx.data operator 1.0.1 build number v1.0.1-1054-20230721-214944-onprem-v1.0.1
```
### 3.3.2 Get the Login credentials to access IBM Cockpit URL (ZEN)
```py linenums="1"
/root/ibm-lh-manage/ibm-lakehouse-manage get-cpd-instance-details
```
**Example**
```py linenums="1"
CPD Url: cpd-watsonxdata1-instance.apps.64da1ffc1bedbf00175f38c9.cloud.techzone.ibm.com
CPD Username: admin
CPD Password: 5SieL9rI6NFS
```
### 3.4 Get the watsonx.data external URL
```py linenums="1"
oc get routes -A | grep cpd | awk '{print "https://" $3}'
```
### 3.5 Validate Memory
```py linenums="1"
oc adm top node
```
```py linenums="1"
NAME       CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
master-1   2875m        9%     25062Mi         19%
```
```py linenums="1"
oc adm top pod -n $PROJECT_CPD_INSTANCE
```
```py linenums="1"
NAME                                               CPU(cores)   MEMORY(bytes)
ibm-lh-lakehouse-hive-metastore-76cf5d8fd5-68hns   10m          329Mi
ibm-lh-lakehouse-minio-67cc69976b-8pvvk            0m           148Mi
ibm-lh-lakehouse-presto-01-presto-0                50m          1568Mi
ibm-lh-postgres-edb-1                              4m           112Mi
ibm-lh-postgres-edb-2                              4m           41Mi
ibm-lh-postgres-edb-3                              4m           38Mi
ibm-nginx-8665d8c6d9-nxw66                         6m           109Mi
ibm-nginx-8665d8c6d9-qq4js                         4m           94Mi
ibm-nginx-tester-554756d6f5-hk7t4                  4m           72Mi
lhconsole-api-76fd9f9646-shjbj                     12m          140Mi
lhconsole-nodeclient-dbfbc4465-q2fdf               0m           31Mi
lhconsole-ui-dd57d6c5f-bbrtj                       1m           4Mi
spark-hb-br-recovery-6b69c86964-4srh8              2m           0Mi
spark-hb-cloud-native-postgresql-1                 3m           61Mi
spark-hb-cloud-native-postgresql-2                 3m           58Mi
spark-hb-control-plane-69cf58b5cb-m6hqt            158m         1652Mi
spark-hb-create-trust-store-58669dcd57-4f8s8       46m          56Mi
spark-hb-deployer-agent-69c79755b8-zgwwb           2m           22Mi
spark-hb-nginx-94cdd5474-8knzb                     0m           6Mi
spark-hb-register-hb-dataplane-776dcbb54f-z2d47    4m           4Mi
usermgmt-85455dbcd-7shhr                           1m           55Mi
usermgmt-85455dbcd-snd5w                           0m           62Mi
zen-audit-74d75cdf76-kftgw                         5m           102Mi
zen-core-54575f65d6-9jv9d                          4m           111Mi
zen-core-54575f65d6-wqwpc                          3m           99Mi
zen-core-api-cb7458bbb-5f2nw                       20m          352Mi
zen-core-api-cb7458bbb-r6qcw                       15m          346Mi
zen-metastore-edb-1                                4m           88Mi
zen-metastore-edb-2                                6m           81Mi
zen-minio-0                                        32m          363Mi
zen-minio-1                                        35m          362Mi
zen-minio-2                                        27m          365Mi
zen-watchdog-6b8c9c9987-jkbcr                      1m           102Mi
zen-watcher-8b88f4858-sx4l9                        12m          273Mi
```

##### Navigation
[Previous Chapter](../Prepare%20the%20Installation) or return to the [Introduction](../README.md).