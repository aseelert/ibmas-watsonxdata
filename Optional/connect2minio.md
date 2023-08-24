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
use portforward to the service, so the console is available at http://localhost:9001 via Firefox 
```py linenums="1"
kubectl port-forward svc/ibm-lh-lakehouse-minio-svc -n $PROJECT_CPD_INSTANCE --address 0.0.0.0 9001:9001
```


### 1.2 get the internal IBM Lakehouse MinIO information
Get the S3 secrets to connect to the MinIO console:
```py linenums="1"
SECRET_NAME="ibm-lh-config-secret"
NAMESPACE="watsonxdata1-instance"

# Fetch the secret data using kubectl and store it in a variable
SECRET_DATA=$(kubectl get secret $SECRET_NAME -n $NAMESPACE -o json)

# Extract the data fields and decode them from base64
LH_INSTANCE_SECRET=$(echo $SECRET_DATA | jq -r '.data.LH_INSTANCE_SECRET' | base64 -d)
LH_KEYSTORE_PASSWORD=$(echo $SECRET_DATA | jq -r '.data.LH_KEYSTORE_PASSWORD' | base64 -d)
POSTGRES_PASSWORD=$(echo $SECRET_DATA | jq -r '.data.POSTGRES_PASSWORD' | base64 -d)
ENV_PROPERTIES=$(echo $SECRET_DATA | jq -r '.data."env.properties"' | base64 -d)

# Print the formatted data
echo "LH_INSTANCE_SECRET: $LH_INSTANCE_SECRET"
echo "LH_KEYSTORE_PASSWORD: $LH_KEYSTORE_PASSWORD"
echo "POSTGRES_PASSWORD: $POSTGRES_PASSWORD"
echo "env.properties: $ENV_PROPERTIES"
```
