```
oc get secret ibm-zen-objectstore-secret -n $PROJECT_CPD_INSTANCE -o go-template='{{.data.accesskey | base64decode}}'
oc get secret ibm-zen-objectstore-secret -n $PROJECT_CPD_INSTANCE -o go-template='{{.data.secretkey | base64decode}}'
```
