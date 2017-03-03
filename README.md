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

# Tutorials

simple tutorial how to use kubeadm to create your k8s cluster:
https://kubernetes.io/docs/tutorials/

Words of wisdom "run kubeadm init only on your master/s". If you messed up, you can always run ```kubeadm reset (in case of errors)``` to reset (delete) all the configs and start over.

install your cluster including weave network:
https://deploy-preview-1521--kubernetes-io-vnext-staging.netlify.com/docs/getting-started-guides/kubeadm/

join your nodes (run this on your node after you init your master):
```
kubeadm join --token=ff93.my-master-toke-after-initff paster-IP
```

Install all the shit on all nodes:
```
apt-get install clusterssh
cssh --username=gjanak -a 'sudo apt-get install [package-name] -y'  IP1 IP2 IP3 IP4
```
