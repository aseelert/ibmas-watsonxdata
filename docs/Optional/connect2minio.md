## 1. Connect to the internal Lakehouse MinIO instance

### 1.1 Make Minio console service available for external access
Expose the port and retrieve the remote url 
```py linenums="1"
oc expose svc/ibm-lh-lakehouse-minio-svc
oc get route ibm-lh-lakehouse-minio-svc -n $PROJECT_CPD_INSTANCE
```
**Example output:**
```py linenums="1"
NAME                         HOST/PORT                                                                                               PATH   SERVICES                     PORT                 TERMINATION   WILDCARD
ibm-lh-lakehouse-minio-svc   ibm-lh-lakehouse-minio-svc-watsonxdata1-instance.apps.64e30977c502c400176b4b03.cloud.techzone.ibm.com          ibm-lh-lakehouse-minio-svc   consoletoberemoved                 None
```
#### 1.1.2 Access the MinIO console frontend
Currently **only Firefox** can handle the console via **http** as Chrome and Safari will switch to **https** which is currently not working.
http://ibm-lh-lakehouse-minio-svc-watsonxdata1-instance.apps.64e30977c502c400176b4b03.cloud.techzone.ibm.com

#### 1.1.3 Alternative without create a route
use portforward to the service, so the console is available at(http://localhost:9001) via Firefox 
```py linenums="1"
kubectl port-forward svc/ibm-lh-lakehouse-minio-svc -n $PROJECT_CPD_INSTANCE --address 0.0.0.0 9001:9001
```


### 1.2 get the internal IBM Lakehouse MinIO information
Get the S3 secrets to connect to the MinIO console:
**LH_S3_ACCESS_KEY and LH_S3_SECRET_KEY**
```py linenums="1"
# Fetch the secret data using kubectl and store it in a variable
SECRET_DATA=$(kubectl get secret ibm-lh-config-secret -n $PROJECT_CPD_INSTANCE -o json)

# Extract the data fields and decode them from base64
ENV_PROPERTIES=$(echo $SECRET_DATA | jq -r '.data."env.properties"' | base64 -d)

# Print the formatted data
echo "env.properties: $ENV_PROPERTIES" | grep "LH_S3_"
```
