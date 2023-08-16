## 1. Minio (still in progress)
### 1.2 get internal IBM Lakehouse Minio information
```py linenums="1"
echo "Access Key:" && echo "$(oc get secret ibm-zen-objectstore-secret -n $PROJECT_CPD_INSTANCE -o go-template='{{.data.accesskey | base64decode}}')" \
&& echo "Secret Key:" && echo "$(oc get secret ibm-zen-objectstore-secret -n $PROJECT_CPD_INSTANCE -o go-template='{{.data.secretkey | base64decode}}')"
 
Access Key:
8927889015015009
Secret Key:
wuuIeSjrQyHQbNTX
```
