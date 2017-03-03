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

# Azure

how to create a VM:
 https://docs.docker.com/machine/drivers/azure/
 https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-linux-docker-machine
debian issues when running docker as different user:
 https://github.com/azuredk/docker-on-azure-hol/blob/master/exercise02/01-machine/README.md

install cli:
 https://docs.microsoft.com/en-us/azure/xplat-cli-install

create a default docker node:
```
docker-machine create --driver none --url=tcp://13.69.86.210:2376 gj-node-01
docker-machine ls
```

now some more stuff (still I didn't figure out how to join an existing vnet with M$, it will create its own docker VNet):
```
docker-machine create --driver azure \
--azure-subscription-id=d41c-my-subscription-id7c2 \
--azure-image=canonical:UbuntuServer:16.04.0-LTS:latest \
--azure-location=westeurope \
--azure-resource-group=K8S \
--azure-ssh-user=drevilkuko \
--azure-size=Standard_D2 \
--azure-static-public-ip \
--azure-open-port 80 \
gj-node-test-01
```

Words of wisdom "Standard_D2 is 2CPU 7GB RAM node", think if you actually need that. Test your app whether it is CPU, Disk or RAM greedy.

# Deploy EmailStudio

login to "your" azure registry: ```docker login cacidocker-caciuk.azurecr.io -u <username> -p <password>```

```
docker-machine ls
docker-machine env gj-node-test-01
eval $(docker-machine env gj-node-test-01)
docker info

#if the shit hits the fan:
docker-machine regenerate-certs gj-node-test-01

#specific to my docker compose files:
docker volume create --name=my-volume
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up [-d]
```

Kick out all your gests: ```docker-compose stop gj-node-test-03 gj-node-test-04 gj-node-test-05```
