# Dockerfiles for K8S and Docker Compose configuration

This repo allows you to build local HDFS-Spark clusters and containerized Kubernetes clusters

# To run the show

cd to gcloud or azure folder and run deploy.sh

If you are not using node selector (azure?) then delete selectors from:

 hdfs-spark-master-service.yaml
 zeppelin-controller.yaml
 
 ```
 #something like this:
   selector:
      name: spark-master
 #or this:
    nodeSelector:
      master: "true"
```

In your hdfs-spark-master-controller.yaml you need to create a k8s secret:

```
 imagePullSecrets:
- name: myregistrykey
```
