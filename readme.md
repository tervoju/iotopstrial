# Guide to get iot operations installed

## hw and os


WSL2 environment

Description:    Ubuntu 22.04.3 LTS
Release:        22.04
Codename:       jammy

Thinkpad T16



install kube k3s from the k3s

```
curl -sfL https://get.k3s.io | sh - 
```

does not work in ubuntu wsl

```
mkdir ~/.kube
sudo KUBECONFIG=~/.kube/config:/etc/rancher/k3s/k3s.yaml kubectl config view --flatten > ~/.kube/merged
mv ~/.kube/merged ~/.kube/config
chmod  0600 ~/.kube/config
export KUBECONFIG=~/.kube/config
#switch to k3s context
kubectl config use-context default
```

download script and excecute

```
install kubeclt
```

login azure 

# Id of the subscription where your resource group and Arc-enabled cluster will be created

```
# variables
export SUBSCRIPTION_ID=XXXXXXX
# Azure region where the created resource group will be located
# Currently supported regions: "eastus", "eastus2", "westus", "westus2", "westus3", "westeurope", or "northeurope"
export LOCATION=northeurope
# Name of a new resource group to create which will hold the Arc-enabled cluster and Azure IoT Operations resources
export RESOURCE_GROUP=XXXXXX
# Name of the Arc-enabled cluster to create in your resource group
export CLUSTER_NAME=XXXXXX
```

```	
az connectedk8s connect -n $CLUSTER_NAME -l $LOCATION -g $RESOURCE_GROUP --subscription $SUBSCRIPTION_ID
```

need to have also kube configuration file in the command or in WSL config needs to be copied to win side to 
'/etc/rancher/k3s/k3s.yaml' should be copied to %USERPROFILE%.kube\config (or actually from ~/kube/config)

```
az connectedk8s connect -n $CLUSTER_NAME -l $LOCATION -g $RESOURCE_GROUP --subscription $SUBSCRIPTION_ID --kube-config ~/.kube/config
```

install helm

problems with service?

```
journalctl -xeu k3s.service
```

```
systemctl status k3s.service
```

```
sudo systemctl restart k3s
```

wrong host address

Unable to verify connectivity to the Kubernetes cluster.
Error occured while connecting to the kubernetes cluster: 
Error: HTTPSConnectionPool(host='127.0.0.1', port=6443): 

not needed ?
#link kubectl to k3s

#sudo ln -s /usr/local/bin/k3s /usr/local/bin/kubectl

export KUBECONFIG=~/.kube/config

```
hostname -I 
```

 https://172.26.107.18:6443

'/etc/rancher/k3s/k3s.yaml' should be copied to %USERPROFILE%.kube\config
change kubeconfig to match the hostname in the ~/.kube/config


sp

```
az ad sp show --id bc313c14-388c-4e7d-a58e-70017303ee3b --query id -o tsv
```
you get the object/id

20fe9baf-70fa-4c27-9199-XXXXXXXXXXXXXXX

```
az connectedk8s enable-features -n $CLUSTER_NAME -g $RESOURCE_GROUP --custom-locations-oid <objectId/id> --features cluster-connect custom-locations
```

custom locations 


az connectedk8s enable-features -n $CLUSTER_NAME -g $RESOURCE_GROUP --custom-locations-oid <XXXXXXXX> --features cluster-connect custom-locations --kube-config ~/.kube/config


az keyvault create --enable-rbac-authorization false --name "deviotkeyvault01" --resource-group "rg-tervo-sandbox"

-> azure arc -> iot Operations



az iot ops init --subscription <XXXX> -g rg-tervo-sandbox --cluster deviotclustertervo01 --kv-id /subscriptions/<XXXX>/resourceGroups/<rg_group>/providers/Microsoft.KeyVault/vaults/<devkeyvault> --custom-location <devcluster>-cl --target deviotclustertervo01-target --dp-instance <devcluster>-processor --simulate-plc --mq-instance mq-instance --mq-mode auto --mq-mem-profile medium --mq-backend-part 2 --mq-backend-rf 2 --mq-backend-workers 2 --mq-frontend-replicas 2 --mq-frontend-workers 2


Remote development is only supported using Kubernetes service environment 
variables. Set 'useKubernetesServiceEnvironmentVariables' task property to 'true'
for Bridge to Kubernetes's task and select the 'Learn More' button to understand the requirements.

