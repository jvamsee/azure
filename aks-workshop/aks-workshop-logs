Requesting a Cloud Shell.Succeeded.
Connecting terminal...

vamsi@Azure:~$ REGION_NAME=eastus
vamsi@Azure:~$ RESOURCE_GROUP=aksworkshop
vamsi@Azure:~$ SUBNET_NAME=aks-subnet
vamsi@Azure:~$ VNET_NAME=aks-vnet
vamsi@Azure:~$
vamsi@Azure:~$ az group create \
>   --name $RESOURCE_GROUP \
>   --location $REGION_NAME
{
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop",
  "location": "eastus",
  "managedBy": null,
  "name": "aksworkshop",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ az network vnet create \
>   --resource-group $RESOURCE_GROUP \
>   --location $REGION_NAME \
>   --name $VNET_NAME \
>   --address-prefixes 10.0.0.0/8 \
>   --subnet-name $SUBNET_NAME \
>   --subnet-prefixes 10.240.0.0/16
{
  "newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/8"
      ]
    },
    "bgpCommunities": null,
    "ddosProtectionPlan": null,
    "dhcpOptions": {
      "dnsServers": []
    },
    "enableDdosProtection": false,
    "enableVmProtection": false,
    "etag": "W/\"143cbcd5-e430-449e-8291-b367f7e62271\"",
    "extendedLocation": null,
    "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/virtualNetworks/aks-vnet",
    "ipAllocations": null,
    "location": "eastus",
    "name": "aks-vnet",
    "provisioningState": "Succeeded",
    "resourceGroup": "aksworkshop",
    "resourceGuid": "1f05138e-76a5-467b-8192-38cba053f5a5",
    "subnets": [
      {
        "addressPrefix": "10.240.0.0/16",
        "addressPrefixes": null,
        "delegations": [],
        "etag": "W/\"143cbcd5-e430-449e-8291-b367f7e62271\"",
        "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/virtualNetworks/aks-vnet/subnets/aks-subnet",
        "ipAllocations": null,
        "ipConfigurationProfiles": null,
        "ipConfigurations": null,
        "name": "aks-subnet",
        "natGateway": null,
        "networkSecurityGroup": null,
        "privateEndpointNetworkPolicies": "Enabled",
        "privateEndpoints": null,
        "privateLinkServiceNetworkPolicies": "Enabled",
        "provisioningState": "Succeeded",
        "purpose": null,
        "resourceGroup": "aksworkshop",
        "resourceNavigationLinks": null,
        "routeTable": null,
        "serviceAssociationLinks": null,
        "serviceEndpointPolicies": null,
        "serviceEndpoints": null,
        "type": "Microsoft.Network/virtualNetworks/subnets"
      }
    ],
    "tags": {},
    "type": "Microsoft.Network/virtualNetworks",
    "virtualNetworkPeerings": []
  }
}
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ SUBNET_ID=$(az network vnet subnet show \
>   --resource-group $RESOURCE_GROUP \
>   --vnet-name $VNET_NAME \
>   --name $SUBNET_NAME \
>   --query id -o tsv)
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ VERSION=$(az aks get-versions \
>   --location $REGION_NAME \
>   --query 'orchestrators[?!isPreview] | [-1].orchestratorVersion' \
>   --output tsv)
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ AKS_CLUSTER_NAME=aksworkshop-$RANDOM
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ az aks create \
>   --resource-group $RESOURCE_GROUP \
>   --name $AKS_CLUSTER_NAME \
>   --vm-set-type VirtualMachineScaleSets \
>   --node-count 2 \
>   --load-balancer-sku standard \
>   --location $REGION_NAME \
>   --kubernetes-version $VERSION \
>   --network-plugin azure \
>   --vnet-subnet-id $SUBNET_ID \
>   --service-cidr 10.2.0.0/24 \
>   --dns-service-ip 10.2.0.10 \
>   --docker-bridge-address 172.17.0.1/16 \
>   --generate-ssh-keys
It is highly recommended to use USER assigned identity (option --assign-identity) when you want to bring your ownsubnet, which will have no latency for the role assignment to take effect. When using SYSTEM assigned identity, azure-cli will grant Network Contributor role to the system assigned identity after the cluster is created, and the role assignment will take some time to take effect, see https://docs.microsoft.com/en-us/azure/aks/use-managed-identity, proceed to create cluster with system assigned identity? (y/N): y
{- Finished ..
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "count": 2,
      "enableAutoScaling": null,
      "enableNodePublicIp": false,
      "maxCount": null,
      "maxPods": 30,
      "minCount": null,
      "mode": "System",
      "name": "nodepool1",
      "nodeImageVersion": "AKSUbuntu-1804containerd-2020.12.15",
      "nodeLabels": {},
      "nodeTaints": null,
      "orchestratorVersion": "1.19.3",
      "osDiskSizeGb": 128,
      "osDiskType": "Managed",
      "osType": "Linux",
      "powerState": {
        "code": "Running"
      },
      "provisioningState": "Succeeded",
      "proximityPlacementGroupId": null,
      "scaleSetEvictionPolicy": null,
      "scaleSetPriority": null,
      "spotMaxPrice": null,
      "tags": null,
      "type": "VirtualMachineScaleSets",
      "upgradeSettings": null,
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/virtualNetworks/aks-vnet/subnets/aks-subnet"
    }
  ],
  "apiServerAccessProfile": null,
  "autoScalerProfile": null,
  "diskEncryptionSetId": null,
  "dnsPrefix": "aksworksho-aksworkshop-c70c6f",
  "enablePodSecurityPolicy": null,
  "enableRbac": true,
  "fqdn": "aksworksho-aksworkshop-c70c6f-18e76dd9.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/aksworkshop/providers/Microsoft.ContainerService/managedClusters/aksworkshop-2016",
  "identity": {
    "principalId": "d4458ce1-1627-4238-8703-4863c508862c",
    "tenantId": "ec34354a-7cbd-41c6-88ec-1b55a6393a74",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "e03de4d1-222f-4f7f-918e-437c6f2f732d",
      "objectId": "9412bc10-5f71-412c-b914-2b51a81be0d0",
      "resourceId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.ManagedIdentity/userAssignedIdentities/aksworkshop-2016-agentpool"
    }
  },
  "kubernetesVersion": "1.19.3",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzZ2slqLUV3nBt6U7k8gTTdC3bsb5JcKfo+86QV3HnisbtRtAZz6c6INqkftvuR5bN1LWrbjP2qEF3BVjRgylkQqbdlTSYRdWctJwkj4EIeomCn6DxzvKmd47n2qqVENo+ULNUTwSxa618NqRMP3zbW/lSHk4Q8Y/EGeldh6wwrb07tkuTo80D+EcvkUvHHlbRHflyN2Zpjdk4DCZDtCbDU1YTM0iuGRaVs/bvpBm2L8A80XwjrsNNJeMuaWdX20XI+xqQBi2U34uMAL41QpfzzP75RM5mDS583M1RHS57L5aOPaVyc8MIm/ggZ1YiigIHmg2drRUs5ZRvU8zWEpI7"
        }
      ]
    }
  },
  "location": "eastus",
  "maxAgentPools": 10,
  "name": "aksworkshop-2016",
  "networkProfile": {
    "dnsServiceIp": "10.2.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerProfile": {
      "allocatedOutboundPorts": null,
      "effectiveOutboundIps": [
        {
          "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.Network/publicIPAddresses/f8980e4a-21c3-4deb-ae1a-0d2c94be459d",
          "resourceGroup": "MC_aksworkshop_aksworkshop-2016_eastus"
        }
      ],
      "idleTimeoutInMinutes": null,
      "managedOutboundIps": {
        "count": 1
      },
      "outboundIpPrefixes": null,
      "outboundIps": null
    },
    "loadBalancerSku": "Standard",
    "networkMode": null,
    "networkPlugin": "azure",
    "networkPolicy": null,
    "outboundType": "loadBalancer",
    "podCidr": null,
    "serviceCidr": "10.2.0.0/24"
  },
  "nodeResourceGroup": "MC_aksworkshop_aksworkshop-2016_eastus",
  "powerState": {
    "code": "Running"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "aksworkshop",
  "servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  },
  "sku": {
    "name": "Basic",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters",
  "windowsProfile": {
    "adminPassword": null,
    "adminUsername": "azureuser",
    "licenseType": null
  }
}
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ az aks enable-addons --resource-group aksworkshop --name aksworkshop-2016 --addons http_application_routing
{- Finished ..
  "aadProfile": null,
  "addonProfiles": {
    "httpApplicationRouting": {
      "config": {
        "HTTPApplicationRoutingZoneName": "f172fe29680645f682f9.eastus.aksapp.io"
      },
      "enabled": true,
      "identity": {
        "clientId": "78b68ecf-1b67-4f2c-9caa-a2afb5872ab2",
        "objectId": "8dd07c1e-9daa-4e03-8e0a-138d7993483f",
        "resourceId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.ManagedIdentity/userAssignedIdentities/httpapplicationrouting-aksworkshop-2016"
      }
    }
  },
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "count": 2,
      "enableAutoScaling": null,
      "enableNodePublicIp": false,
      "maxCount": null,
      "maxPods": 30,
      "minCount": null,
      "mode": "System",
      "name": "nodepool1",
      "nodeImageVersion": "AKSUbuntu-1804containerd-2020.12.15",
      "nodeLabels": {},
      "nodeTaints": null,
      "orchestratorVersion": "1.19.3",
      "osDiskSizeGb": 128,
      "osDiskType": "Managed",
      "osType": "Linux",
      "powerState": {
        "code": "Running"
      },
      "provisioningState": "Succeeded",
      "proximityPlacementGroupId": null,
      "scaleSetEvictionPolicy": null,
      "scaleSetPriority": null,
      "spotMaxPrice": null,
      "tags": null,
      "type": "VirtualMachineScaleSets",
      "upgradeSettings": null,
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/virtualNetworks/aks-vnet/subnets/aks-subnet"
    }
  ],
  "apiServerAccessProfile": null,
  "autoScalerProfile": null,
  "diskEncryptionSetId": null,
  "dnsPrefix": "aksworksho-aksworkshop-c70c6f",
  "enablePodSecurityPolicy": null,
  "enableRbac": true,
  "fqdn": "aksworksho-aksworkshop-c70c6f-18e76dd9.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/aksworkshop/providers/Microsoft.ContainerService/managedClusters/aksworkshop-2016",
  "identity": {
    "principalId": "d4458ce1-1627-4238-8703-4863c508862c",
    "tenantId": "ec34354a-7cbd-41c6-88ec-1b55a6393a74",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "e03de4d1-222f-4f7f-918e-437c6f2f732d",
      "objectId": "9412bc10-5f71-412c-b914-2b51a81be0d0",
      "resourceId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.ManagedIdentity/userAssignedIdentities/aksworkshop-2016-agentpool"
    }
  },
  "kubernetesVersion": "1.19.3",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzZ2slqLUV3nBt6U7k8gTTdC3bsb5JcKfo+86QV3HnisbtRtAZz6c6INqkftvuR5bN1LWrbjP2qEF3BVjRgylkQqbdlTSYRdWctJwkj4EIeomCn6DxzvKmd47n2qqVENo+ULNUTwSxa618NqRMP3zbW/lSHk4Q8Y/EGeldh6wwrb07tkuTo80D+EcvkUvHHlbRHflyN2Zpjdk4DCZDtCbDU1YTM0iuGRaVs/bvpBm2L8A80XwjrsNNJeMuaWdX20XI+xqQBi2U34uMAL41QpfzzP75RM5mDS583M1RHS57L5aOPaVyc8MIm/ggZ1YiigIHmg2drRUs5ZRvU8zWEpI7"
        }
      ]
    }
  },
  "location": "eastus",
  "maxAgentPools": 10,
  "name": "aksworkshop-2016",
  "networkProfile": {
    "dnsServiceIp": "10.2.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerProfile": {
      "allocatedOutboundPorts": null,
      "effectiveOutboundIps": [
        {
          "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.Network/publicIPAddresses/f8980e4a-21c3-4deb-ae1a-0d2c94be459d",
          "resourceGroup": "MC_aksworkshop_aksworkshop-2016_eastus"
        }
      ],
      "idleTimeoutInMinutes": null,
      "managedOutboundIps": {
        "count": 1
      },
      "outboundIpPrefixes": null,
      "outboundIps": null
    },
    "loadBalancerSku": "Standard",
    "networkMode": null,
    "networkPlugin": "azure",
    "networkPolicy": null,
    "outboundType": "loadBalancer",
    "podCidr": null,
    "serviceCidr": "10.2.0.0/24"
  },
  "nodeResourceGroup": "MC_aksworkshop_aksworkshop-2016_eastus",
  "powerState": {
    "code": "Running"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "aksworkshop",
  "servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  },
  "sku": {
    "name": "Basic",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters",
  "windowsProfile": {
    "adminPassword": null,
    "adminUsername": "azureuser",
    "licenseType": null
  }
}
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ az aks get-credentials \
>   --resource-group $RESOURCE_GROUP \
>   --name $AKS_CLUSTER_NAME
Merged "aksworkshop-2016" as current context in /home/vamsi/.kube/config
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ az aks show --resource-group aksworkshop --name aksworkshop-2016 --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table
Result
-------------------------------------
f172fe29680645f682f9.eastus.aksapp.io
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ kubectl get nodes
NAME                                STATUS   ROLES   AGE   VERSION
aks-nodepool1-23986151-vmss000000   Ready    agent   32m   v1.19.3
aks-nodepool1-23986151-vmss000001   Ready    agent   32m   v1.19.3
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ kubectl get namespace
NAME              STATUS   AGE
default           Active   34m
kube-node-lease   Active   34m
kube-public       Active   34m
kube-system       Active   34m
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ kubectl create namespace ratingsapp
namespace/ratingsapp created
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ kubectl get namespace
NAME              STATUS   AGE
default           Active   34m
kube-node-lease   Active   34m
kube-public       Active   34m
kube-system       Active   34m
ratingsapp        Active   6s
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ ACR_NAME=acr$RANDOM
vamsi@Azure:~$
vamsi@Azure:~$ az acr create \
>   --resource-group $RESOURCE_GROUP \
>   --location $REGION_NAME \
>   --name $ACR_NAME \
>   --sku standard
{- Finished ..
  "adminUserEnabled": false,
  "creationDate": "2021-01-02T21:16:56.098675+00:00",
  "dataEndpointEnabled": false,
  "dataEndpointHostNames": [],
  "encryption": {
    "keyVaultProperties": null,
    "status": "disabled"
  },
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.ContainerRegistry/registries/acr7839",
  "identity": null,
  "location": "eastus",
  "loginServer": "acr7839.azurecr.io",
  "name": "acr7839",
  "networkRuleSet": null,
  "policies": {
    "quarantinePolicy": {
      "status": "disabled"
    },
    "retentionPolicy": {
      "days": 7,
      "lastUpdatedTime": "2021-01-02T21:16:57.111950+00:00",
      "status": "disabled"
    },
    "trustPolicy": {
      "status": "disabled",
      "type": "Notary"
    }
  },
  "privateEndpointConnections": [],
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "resourceGroup": "aksworkshop",
  "sku": {
    "name": "Standard",
    "tier": "Standard"
  },
  "status": null,
  "storageAccount": null,
  "systemData": {
    "createdAt": "2021-01-02T21:16:56.0986753+00:00",
    "createdBy": "crazyvamsi8@gmail.com",
    "createdByType": "User",
    "lastModifiedAt": "2021-01-02T21:16:56.0986753+00:00",
    "lastModifiedBy": "crazyvamsi8@gmail.com",
    "lastModifiedByType": "User"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
vamsi@Azure:~$
vamsi@Azure:~$
vamsi@Azure:~$ cd aks-workshop/
vamsi@Azure:~/aks-workshop$ ls
aks-workshop.md      mslearn-aks-workshop-ratings-api  ratings-api-service.yaml      ratings-web-ingress-bkp.yaml                       ratings-web-ingress.yaml
cluster-issuer.yaml  mslearn-aks-workshop-ratings-web  ratings-web-deployment1.yaml  ratings-web-ingress-old.yaml                       ratings-web-service-old.yaml
Kubernetes           rating-api-deployment.yaml        ratings-web-deployment.yaml   ratings-web-ingress-use-without-customdomain.yaml  ratings-web-service.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ cd mslearn-aks-workshop-ratings-api
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$ ls
app.js  CODE_OF_CONDUCT.md  data  Dockerfile  LICENSE  models  nodemon.json  package.json  package-lock.json  README.md  routes  SECURITY.md  server.js  views
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$ az acr build -t ratings-api:v1 -r $ACR_NAME .
Packing source code into tar to upload...
Excluding '.gitignore' based on default ignore rules
Excluding '.git' based on default ignore rules
Uploading archived source code from '/tmp/build_archive_66f7060a365e46cab9767cf54ff654b6.tar.gz'...
Sending context (14.987 KiB) to registry: acr7839...
Queued a build with ID: ca1
Waiting for an agent...
2021/01/02 21:17:44 Downloading source code...
2021/01/02 21:17:45 Finished downloading source code
2021/01/02 21:17:45 Using acb_vol_aa6fc1e1-2880-4e15-8936-6bee00245fc9 as the home volume
2021/01/02 21:17:45 Setting up Docker configuration...
2021/01/02 21:17:46 Successfully set up Docker configuration
2021/01/02 21:17:46 Logging in to registry: acr7839.azurecr.io
2021/01/02 21:17:47 Successfully logged into acr7839.azurecr.io
2021/01/02 21:17:47 Executing step ID: build. Timeout(sec): 28800, Working directory: '', Network: ''
2021/01/02 21:17:47 Scanning for dependencies...
2021/01/02 21:17:48 Successfully scanned dependencies
2021/01/02 21:17:48 Launching container with name: build
Sending build context to Docker daemon  68.61kB
Step 1/8 : FROM node:13.5-alpine
13.5-alpine: Pulling from library/node
e6b0cf9c0882: Pulling fs layer
ab436df1df6f: Pulling fs layer
470300a8a365: Pulling fs layer
84e7c11579ee: Pulling fs layer
84e7c11579ee: Waiting
470300a8a365: Verifying Checksum
470300a8a365: Download complete
e6b0cf9c0882: Verifying Checksum
e6b0cf9c0882: Download complete
84e7c11579ee: Verifying Checksum
84e7c11579ee: Download complete
e6b0cf9c0882: Pull complete
ab436df1df6f: Download complete
ab436df1df6f: Pull complete
470300a8a365: Pull complete
84e7c11579ee: Pull complete
Digest: sha256:a5a7ff4267a810a019c7c3732b3c463a892a61937d84ee952c34af2fb486058d
Status: Downloaded newer image for node:13.5-alpine
 ---> e1495e4ac50d
Step 2/8 : WORKDIR /usr/src/app
 ---> Running in 87e57e5dc64a
Removing intermediate container 87e57e5dc64a
 ---> 0d7ef7662041
Step 3/8 : RUN apk update && apk add python g++ make && rm -rf /var/cache/apk/*
 ---> Running in 7d3e7383b525
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/community/x86_64/APKINDEX.tar.gz
v3.11.7-16-g9915bfe10d [http://dl-cdn.alpinelinux.org/alpine/v3.11/main]
v3.11.7 [http://dl-cdn.alpinelinux.org/alpine/v3.11/community]
OK: 11279 distinct packages available
(1/23) Upgrading musl (1.1.24-r0 -> 1.1.24-r3)
(2/23) Upgrading libstdc++ (9.2.0-r3 -> 9.3.0-r0)
(3/23) Installing binutils (2.33.1-r0)
(4/23) Installing gmp (6.1.2-r1)
(5/23) Installing isl (0.18-r0)
(6/23) Installing libgomp (9.3.0-r0)
(7/23) Installing libatomic (9.3.0-r0)
(8/23) Installing mpfr4 (4.0.2-r1)
(9/23) Installing mpc1 (1.1.0-r1)
(10/23) Installing gcc (9.3.0-r0)
(11/23) Installing musl-dev (1.1.24-r3)
(12/23) Installing libc-dev (0.7.2-r0)
(13/23) Installing g++ (9.3.0-r0)
(14/23) Installing make (4.2.1-r2)
(15/23) Installing libbz2 (1.0.8-r1)
(16/23) Installing expat (2.2.9-r1)
(17/23) Installing libffi (3.2.1-r6)
(18/23) Installing gdbm (1.13-r1)
(19/23) Installing ncurses-terminfo-base (6.1_p20200118-r4)
(20/23) Installing ncurses-libs (6.1_p20200118-r4)
(21/23) Installing readline (8.0.1-r0)
(22/23) Installing sqlite-libs (3.30.1-r2)
(23/23) Installing python2 (2.7.18-r0)
Executing busybox-1.31.1-r8.trigger
OK: 212 MiB in 37 packages
Removing intermediate container 7d3e7383b525
 ---> b76cd752f508
Step 4/8 : COPY package*.json ./
 ---> f3751773aba0
Step 5/8 : RUN npm install
 ---> Running in e8c7ddd8afb2
npm WARN rating-api@1.3.5 No repository field.
npm WARN rating-api@1.3.5 No license field.

added 106 packages from 97 contributors and audited 107 packages in 3.688s
found 10 vulnerabilities (4 low, 1 moderate, 5 high)
  run `npm audit fix` to fix them, or `npm audit` for details
Removing intermediate container e8c7ddd8afb2
 ---> 24082d791564
Step 6/8 : COPY . .
 ---> f6c7c5d1b75f
Step 7/8 : EXPOSE 3000
 ---> Running in 6fc7c868b8c0
Removing intermediate container 6fc7c868b8c0
 ---> 706ad313b227
Step 8/8 : CMD [ "npm", "start"]
 ---> Running in 1448f07baeda
Removing intermediate container 1448f07baeda
 ---> 3d5199481242
Successfully built 3d5199481242
Successfully tagged acr7839.azurecr.io/ratings-api:v1
2021/01/02 21:18:14 Successfully executed container: build
2021/01/02 21:18:14 Executing step ID: push. Timeout(sec): 3600, Working directory: '', Network: ''
2021/01/02 21:18:14 Pushing image: acr7839.azurecr.io/ratings-api:v1, attempt 1
The push refers to repository [acr7839.azurecr.io/ratings-api]
12cf2f404ef5: Preparing
e2c7ab1492f0: Preparing
8d4d7a783904: Preparing
d589b40649e7: Preparing
dde734ed3515: Preparing
efd6e0da275f: Preparing
b352b61d0fe4: Preparing
d06ff5e5272b: Preparing
6b27de954cca: Preparing
efd6e0da275f: Waiting
b352b61d0fe4: Waiting
d06ff5e5272b: Waiting
6b27de954cca: Waiting
8d4d7a783904: Pushed
dde734ed3515: Pushed
12cf2f404ef5: Pushed
e2c7ab1492f0: Pushed
efd6e0da275f: Pushed
b352b61d0fe4: Pushed
6b27de954cca: Pushed
d589b40649e7: Pushed
d06ff5e5272b: Pushed
v1: digest: sha256:6007588228f8b604c81b332cac576ea473833360c45f4bcb5abaa78142127b4c size: 2204
2021/01/02 21:18:36 Successfully pushed image: acr7839.azurecr.io/ratings-api:v1
2021/01/02 21:18:36 Step ID: build marked as successful (elapsed time in seconds: 27.100601)
2021/01/02 21:18:36 Populating digests for step ID: build...
2021/01/02 21:18:37 Successfully populated digests for step ID: build
2021/01/02 21:18:37 Step ID: push marked as successful (elapsed time in seconds: 21.488105)
2021/01/02 21:18:37 The following dependencies were found:
2021/01/02 21:18:37
- image:
    registry: acr7839.azurecr.io
    repository: ratings-api
    tag: v1
    digest: sha256:6007588228f8b604c81b332cac576ea473833360c45f4bcb5abaa78142127b4c
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 13.5-alpine
    digest: sha256:a5a7ff4267a810a019c7c3732b3c463a892a61937d84ee952c34af2fb486058d
  git: {}

Run ID: ca1 was successful after 55s
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$ cd
vamsi@Azure:~$ cd aks-workshop/
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ cd mslearn-aks-workshop-ratings-web
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$ ls
CODE_OF_CONDUCT.md  Dockerfile  LICENSE  package.json  package-lock.json  postcss.config.js  README.md  SECURITY.md  src  static  webpack.config.js
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$ az acr build -t ratings-api:v1 -r $ACR_NAME .
Packing source code into tar to upload...
Excluding '.gitignore' based on default ignore rules
Excluding '.git' based on default ignore rules
Uploading archived source code from '/tmp/build_archive_66f7060a365e46cab9767cf54ff654b6.tar.gz'...
Sending context (14.987 KiB) to registry: acr7839...
Queued a build with ID: ca1
Waiting for an agent...
2021/01/02 21:17:44 Downloading source code...
2021/01/02 21:17:45 Finished downloading source code
2021/01/02 21:17:45 Using acb_vol_aa6fc1e1-2880-4e15-8936-6bee00245fc9 as the home volume
2021/01/02 21:17:45 Setting up Docker configuration...
2021/01/02 21:17:46 Successfully set up Docker configuration
2021/01/02 21:17:46 Logging in to registry: acr7839.azurecr.io
2021/01/02 21:17:47 Successfully logged into acr7839.azurecr.io
2021/01/02 21:17:47 Executing step ID: build. Timeout(sec): 28800, Working directory: '', Network: ''
2021/01/02 21:17:47 Scanning for dependencies...
2021/01/02 21:17:48 Successfully scanned dependencies
2021/01/02 21:17:48 Launching container with name: build
Sending build context to Docker daemon  68.61kB
Step 1/8 : FROM node:13.5-alpine
13.5-alpine: Pulling from library/node
e6b0cf9c0882: Pulling fs layer
ab436df1df6f: Pulling fs layer
470300a8a365: Pulling fs layer
84e7c11579ee: Pulling fs layer
84e7c11579ee: Waiting
470300a8a365: Verifying Checksum
470300a8a365: Download complete
e6b0cf9c0882: Verifying Checksum
e6b0cf9c0882: Download complete
84e7c11579ee: Verifying Checksum
84e7c11579ee: Download complete
e6b0cf9c0882: Pull complete
ab436df1df6f: Download complete
ab436df1df6f: Pull complete
470300a8a365: Pull complete
84e7c11579ee: Pull complete
Digest: sha256:a5a7ff4267a810a019c7c3732b3c463a892a61937d84ee952c34af2fb486058d
Status: Downloaded newer image for node:13.5-alpine
 ---> e1495e4ac50d
Step 2/8 : WORKDIR /usr/src/app
 ---> Running in 87e57e5dc64a
Removing intermediate container 87e57e5dc64a
 ---> 0d7ef7662041
Step 3/8 : RUN apk update && apk add python g++ make && rm -rf /var/cache/apk/*
 ---> Running in 7d3e7383b525
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/community/x86_64/APKINDEX.tar.gz
v3.11.7-16-g9915bfe10d [http://dl-cdn.alpinelinux.org/alpine/v3.11/main]
v3.11.7 [http://dl-cdn.alpinelinux.org/alpine/v3.11/community]
OK: 11279 distinct packages available
(1/23) Upgrading musl (1.1.24-r0 -> 1.1.24-r3)
(2/23) Upgrading libstdc++ (9.2.0-r3 -> 9.3.0-r0)
(3/23) Installing binutils (2.33.1-r0)
(4/23) Installing gmp (6.1.2-r1)
(5/23) Installing isl (0.18-r0)
(6/23) Installing libgomp (9.3.0-r0)
(7/23) Installing libatomic (9.3.0-r0)
(8/23) Installing mpfr4 (4.0.2-r1)
(9/23) Installing mpc1 (1.1.0-r1)
(10/23) Installing gcc (9.3.0-r0)
(11/23) Installing musl-dev (1.1.24-r3)
(12/23) Installing libc-dev (0.7.2-r0)
(13/23) Installing g++ (9.3.0-r0)
(14/23) Installing make (4.2.1-r2)
(15/23) Installing libbz2 (1.0.8-r1)
(16/23) Installing expat (2.2.9-r1)
(17/23) Installing libffi (3.2.1-r6)
(18/23) Installing gdbm (1.13-r1)
(19/23) Installing ncurses-terminfo-base (6.1_p20200118-r4)
(20/23) Installing ncurses-libs (6.1_p20200118-r4)
(21/23) Installing readline (8.0.1-r0)
(22/23) Installing sqlite-libs (3.30.1-r2)
(23/23) Installing python2 (2.7.18-r0)
Executing busybox-1.31.1-r8.trigger
OK: 212 MiB in 37 packages
Removing intermediate container 7d3e7383b525
 ---> b76cd752f508
Step 4/8 : COPY package*.json ./
 ---> f3751773aba0
Step 5/8 : RUN npm install
 ---> Running in e8c7ddd8afb2
npm WARN rating-api@1.3.5 No repository field.
npm WARN rating-api@1.3.5 No license field.

added 106 packages from 97 contributors and audited 107 packages in 3.688s
found 10 vulnerabilities (4 low, 1 moderate, 5 high)
  run `npm audit fix` to fix them, or `npm audit` for details
Removing intermediate container e8c7ddd8afb2
 ---> 24082d791564
Step 6/8 : COPY . .
 ---> f6c7c5d1b75f
Step 7/8 : EXPOSE 3000
 ---> Running in 6fc7c868b8c0
Removing intermediate container 6fc7c868b8c0
 ---> 706ad313b227
Step 8/8 : CMD [ "npm", "start"]
 ---> Running in 1448f07baeda
Removing intermediate container 1448f07baeda
 ---> 3d5199481242
Successfully built 3d5199481242
Successfully tagged acr7839.azurecr.io/ratings-api:v1
2021/01/02 21:18:14 Successfully executed container: build
2021/01/02 21:18:14 Executing step ID: push. Timeout(sec): 3600, Working directory: '', Network: ''
2021/01/02 21:18:14 Pushing image: acr7839.azurecr.io/ratings-api:v1, attempt 1
The push refers to repository [acr7839.azurecr.io/ratings-api]
12cf2f404ef5: Preparing
e2c7ab1492f0: Preparing
8d4d7a783904: Preparing
d589b40649e7: Preparing
dde734ed3515: Preparing
efd6e0da275f: Preparing
b352b61d0fe4: Preparing
d06ff5e5272b: Preparing
6b27de954cca: Preparing
efd6e0da275f: Waiting
b352b61d0fe4: Waiting
d06ff5e5272b: Waiting
6b27de954cca: Waiting
8d4d7a783904: Pushed
dde734ed3515: Pushed
12cf2f404ef5: Pushed
e2c7ab1492f0: Pushed
efd6e0da275f: Pushed
b352b61d0fe4: Pushed
6b27de954cca: Pushed
d589b40649e7: Pushed
d06ff5e5272b: Pushed
v1: digest: sha256:6007588228f8b604c81b332cac576ea473833360c45f4bcb5abaa78142127b4c size: 2204
2021/01/02 21:18:36 Successfully pushed image: acr7839.azurecr.io/ratings-api:v1
2021/01/02 21:18:36 Step ID: build marked as successful (elapsed time in seconds: 27.100601)
2021/01/02 21:18:36 Populating digests for step ID: build...
2021/01/02 21:18:37 Successfully populated digests for step ID: build
2021/01/02 21:18:37 Step ID: push marked as successful (elapsed time in seconds: 21.488105)
2021/01/02 21:18:37 The following dependencies were found:
2021/01/02 21:18:37
- image:
    registry: acr7839.azurecr.io
    repository: ratings-api
    tag: v1
    digest: sha256:6007588228f8b604c81b332cac576ea473833360c45f4bcb5abaa78142127b4c
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 13.5-alpine
    digest: sha256:a5a7ff4267a810a019c7c3732b3c463a892a61937d84ee952c34af2fb486058d
  git: {}

Run ID: ca1 was successful after 55s
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-api$ cd
vamsi@Azure:~$ cd aks-workshop/
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ cd mslearn-aks-workshop-ratings-website
bash: cd: mslearn-aks-workshop-ratings-website: No such file or directory
vamsi@Azure:~/aks-workshop$ cd mslearn-aks-workshop-ratings-web
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$ ls
CODE_OF_CONDUCT.md  Dockerfile  LICENSE  package.json  package-lock.json  postcss.config.js  README.md  SECURITY.md  src  static  webpack.config.js
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$ az acr build -t ratings-web:v1 -r $ACR_NAME .
Packing source code into tar to upload...
Excluding '.gitignore' based on default ignore rules
Excluding '.git' based on default ignore rules
Uploading archived source code from '/tmp/build_archive_0fca9e1d24d349d38ff067fa3f934e06.tar.gz'...
Sending context (1.207 MiB) to registry: acr7839...
Queued a build with ID: ca2
Waiting for an agent...
2021/01/02 21:21:06 Downloading source code...
2021/01/02 21:21:07 Finished downloading source code
2021/01/02 21:21:08 Using acb_vol_39e6acc2-799d-4723-8119-890621e1246d as the home volume
2021/01/02 21:21:08 Setting up Docker configuration...
2021/01/02 21:21:09 Successfully set up Docker configuration
2021/01/02 21:21:09 Logging in to registry: acr7839.azurecr.io
2021/01/02 21:21:10 Successfully logged into acr7839.azurecr.io
2021/01/02 21:21:10 Executing step ID: build. Timeout(sec): 28800, Working directory: '', Network: ''
2021/01/02 21:21:10 Scanning for dependencies...
2021/01/02 21:21:11 Successfully scanned dependencies
2021/01/02 21:21:11 Launching container with name: build
Sending build context to Docker daemon  1.627MB
Step 1/8 : FROM node:13.5-alpine
13.5-alpine: Pulling from library/node
e6b0cf9c0882: Pulling fs layer
ab436df1df6f: Pulling fs layer
470300a8a365: Pulling fs layer
84e7c11579ee: Pulling fs layer
84e7c11579ee: Waiting
470300a8a365: Verifying Checksum
470300a8a365: Download complete
e6b0cf9c0882: Verifying Checksum
e6b0cf9c0882: Download complete
84e7c11579ee: Verifying Checksum
84e7c11579ee: Download complete
e6b0cf9c0882: Pull complete
ab436df1df6f: Verifying Checksum
ab436df1df6f: Download complete
ab436df1df6f: Pull complete
470300a8a365: Pull complete
84e7c11579ee: Pull complete
Digest: sha256:a5a7ff4267a810a019c7c3732b3c463a892a61937d84ee952c34af2fb486058d
Status: Downloaded newer image for node:13.5-alpine
 ---> e1495e4ac50d
Step 2/8 : WORKDIR /usr/src/app
 ---> Running in 0017f22eb34e
Removing intermediate container 0017f22eb34e
 ---> e476c4f3f773
Step 3/8 : RUN apk update && apk add python g++ make && rm -rf /var/cache/apk/*
 ---> Running in d146f8e9b720
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/community/x86_64/APKINDEX.tar.gz
v3.11.7-16-g9915bfe10d [http://dl-cdn.alpinelinux.org/alpine/v3.11/main]
v3.11.7 [http://dl-cdn.alpinelinux.org/alpine/v3.11/community]
OK: 11279 distinct packages available
(1/23) Upgrading musl (1.1.24-r0 -> 1.1.24-r3)
(2/23) Upgrading libstdc++ (9.2.0-r3 -> 9.3.0-r0)
(3/23) Installing binutils (2.33.1-r0)
(4/23) Installing gmp (6.1.2-r1)
(5/23) Installing isl (0.18-r0)
(6/23) Installing libgomp (9.3.0-r0)
(7/23) Installing libatomic (9.3.0-r0)
(8/23) Installing mpfr4 (4.0.2-r1)
(9/23) Installing mpc1 (1.1.0-r1)
(10/23) Installing gcc (9.3.0-r0)
(11/23) Installing musl-dev (1.1.24-r3)
(12/23) Installing libc-dev (0.7.2-r0)
(13/23) Installing g++ (9.3.0-r0)
(14/23) Installing make (4.2.1-r2)
(15/23) Installing libbz2 (1.0.8-r1)
(16/23) Installing expat (2.2.9-r1)
(17/23) Installing libffi (3.2.1-r6)
(18/23) Installing gdbm (1.13-r1)
(19/23) Installing ncurses-terminfo-base (6.1_p20200118-r4)
(20/23) Installing ncurses-libs (6.1_p20200118-r4)
(21/23) Installing readline (8.0.1-r0)
(22/23) Installing sqlite-libs (3.30.1-r2)
(23/23) Installing python2 (2.7.18-r0)
Executing busybox-1.31.1-r8.trigger
OK: 212 MiB in 37 packages
Removing intermediate container d146f8e9b720
 ---> 02ce1872e324
Step 4/8 : COPY package*.json ./
 ---> 111db1239d95
Step 5/8 : RUN npm install
 ---> Running in 563b36c212bf


> node-sass@4.12.0 install /usr/src/app/node_modules/node-sass
> node scripts/install.js

Downloading binary from https://github.com/sass/node-sass/releases/download/v4.12.0/linux_musl-x64-79_binding.node
Cannot download "https://github.com/sass/node-sass/releases/download/v4.12.0/linux_musl-x64-79_binding.node":

HTTP error 404 Not Found

Hint: If github.com is not accessible in your location
      try setting a proxy via HTTP_PROXY, e.g.

      export HTTP_PROXY=http://example.com:1234

or configure npm proxy via

      npm config set proxy http://example.com:8080

> core-js@2.6.9 postinstall /usr/src/app/node_modules/core-js
> node scripts/postinstall || echo "ignore"

Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!

The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:
> https://opencollective.com/core-js
> https://www.patreon.com/zloirock

Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)


> uglifyjs-webpack-plugin@0.4.6 postinstall /usr/src/app/node_modules/uglifyjs-webpack-plugin
> node lib/post_install.js


> node-sass@4.12.0 postinstall /usr/src/app/node_modules/node-sass
> node scripts/build.js

Building: /usr/local/bin/node /usr/src/app/node_modules/node-gyp/bin/node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsass_library=
gyp info it worked if it ends with ok
gyp verb cli [
gyp verb cli   '/usr/local/bin/node',
gyp verb cli   '/usr/src/app/node_modules/node-gyp/bin/node-gyp.js',
gyp verb cli   'rebuild',
gyp verb cli   '--verbose',
gyp verb cli   '--libsass_ext=',
gyp verb cli   '--libsass_cflags=',
gyp verb cli   '--libsass_ldflags=',
gyp verb cli   '--libsass_library='
gyp verb cli ]
gyp info using node-gyp@3.8.0
gyp info using node@13.5.0 | linux | x64
gyp verb command rebuild []
gyp verb command clean []
gyp verb clean removing "build" directory
gyp verb command configure []
gyp verb check python checking for Python executable "python2" in the PATH
gyp verb `which` succeeded python2 /usr/bin/python2
gyp verb check python version `/usr/bin/python2 -c "import sys; print "2.7.18
gyp verb check python version .%s.%s" % sys.version_info[:3];"` returned: %j
gyp verb get node dir no --target version specified, falling back to host node version: 13.5.0
gyp verb command install [ '13.5.0' ]
gyp verb install input version string "13.5.0"
gyp verb install installing version: 13.5.0
gyp verb install --ensure was passed, so won't reinstall if already installed
gyp verb install version not already installed, continuing with install 13.5.0
gyp verb ensuring nodedir is created /root/.node-gyp/13.5.0
gyp verb created nodedir /root/.node-gyp
gyp http GET https://unofficial-builds.nodejs.org/download/release/v13.5.0/node-v13.5.0-headers.tar.gz
gyp http 200 https://unofficial-builds.nodejs.org/download/release/v13.5.0/node-v13.5.0-headers.tar.gz
gyp verb extracted file from tarball include/node/common.gypi
gyp verb extracted file from tarball include/node/config.gypi
gyp verb extracted file from tarball include/node/node.h
gyp verb extracted file from tarball include/node/node_api.h
gyp verb extracted file from tarball include/node/js_native_api.h
gyp verb extracted file from tarball include/node/js_native_api_types.h
gyp verb extracted file from tarball include/node/node_api_types.h
gyp verb extracted file from tarball include/node/node_buffer.h
gyp verb extracted file from tarball include/node/node_object_wrap.h
gyp verb extracted file from tarball include/node/node_version.h
gyp verb extracted file from tarball include/node/v8-value-serializer-version.h
gyp verb extracted file from tarball include/node/v8-version-string.h
gyp verb extracted file from tarball include/node/v8-wasm-trap-handler-posix.h
gyp verb extracted file from tarball include/node/v8-wasm-trap-handler-win.h
gyp verb extracted file from tarball include/node/v8-platform.h
gyp verb extracted file from tarball include/node/v8-testing.h
gyp verb extracted file from tarball include/node/v8.h
gyp verb extracted file from tarball include/node/v8-util.h
gyp verb extracted file from tarball include/node/v8-internal.h
gyp verb extracted file from tarball include/node/v8-profiler.h
gyp verb extracted file from tarball include/node/v8-version.h
gyp verb extracted file from tarball include/node/v8config.h
gyp verb extracted file from tarball include/node/libplatform/libplatform-export.h
gyp verb extracted file from tarball include/node/libplatform/libplatform.h
gyp verb extracted file from tarball include/node/libplatform/v8-tracing.h
gyp verb extracted file from tarball include/node/uv/aix.h
gyp verb extracted file from tarball include/node/uv/android-ifaddrs.h
gyp verb extracted file from tarball include/node/uv/bsd.h
gyp verb extracted file from tarball include/node/uv/darwin.h
gyp verb extracted file from tarball include/node/uv/errno.h
gyp verb extracted file from tarball include/node/uv/linux.h
gyp verb extracted file from tarball include/node/uv/os390.h
gyp verb extracted file from tarball include/node/uv/posix.h
gyp verb extracted file from tarball include/node/uv/stdint-msvc2008.h
gyp verb extracted file from tarball include/node/uv/sunos.h
gyp verb extracted file from tarball include/node/uv/threadpool.h
gyp verb extracted file from tarball include/node/uv/tree.h
gyp verb extracted file from tarball include/node/uv/unix.h
gyp verb extracted file from tarball include/node/uv/win.h
gyp verb extracted file from tarball include/node/uv/version.h
gyp verb extracted file from tarball include/node/uv.h
gyp verb extracted file from tarball include/node/openssl/aes.h
gyp verb extracted file from tarball include/node/openssl/asn1.h
gyp verb extracted file from tarball include/node/openssl/asn1_mac.h
gyp verb extracted file from tarball include/node/openssl/asn1err.h
gyp verb extracted file from tarball include/node/openssl/asn1t.h
gyp verb extracted file from tarball include/node/openssl/async.h
gyp verb extracted file from tarball include/node/openssl/asyncerr.h
gyp verb extracted file from tarball include/node/openssl/bio.h
gyp verb extracted file from tarball include/node/openssl/bioerr.h
gyp verb extracted file from tarball include/node/openssl/blowfish.h
gyp verb extracted file from tarball include/node/openssl/bn.h
gyp verb extracted file from tarball include/node/openssl/bnerr.h
gyp verb extracted file from tarball include/node/openssl/buffer.h
gyp verb extracted file from tarball include/node/openssl/buffererr.h
gyp verb extracted file from tarball include/node/openssl/camellia.h
gyp verb extracted file from tarball include/node/openssl/cast.h
gyp verb extracted file from tarball include/node/openssl/cmac.h
gyp verb extracted file from tarball include/node/openssl/cms.h
gyp verb extracted file from tarball include/node/openssl/cmserr.h
gyp verb extracted file from tarball include/node/openssl/comp.h
gyp verb extracted file from tarball include/node/openssl/comperr.h
gyp verb extracted file from tarball include/node/openssl/conf.h
gyp verb extracted file from tarball include/node/openssl/conf_api.h
gyp verb extracted file from tarball include/node/openssl/conferr.h
gyp verb extracted file from tarball include/node/openssl/crypto.h
gyp verb extracted file from tarball include/node/openssl/cryptoerr.h
gyp verb extracted file from tarball include/node/openssl/ct.h
gyp verb extracted file from tarball include/node/openssl/cterr.h
gyp verb extracted file from tarball include/node/openssl/des.h
gyp verb extracted file from tarball include/node/openssl/dh.h
gyp verb extracted file from tarball include/node/openssl/dherr.h
gyp verb extracted file from tarball include/node/openssl/dsa.h
gyp verb extracted file from tarball include/node/openssl/dsaerr.h
gyp verb extracted file from tarball include/node/openssl/dtls1.h
gyp verb extracted file from tarball include/node/openssl/e_os2.h
gyp verb extracted file from tarball include/node/openssl/ebcdic.h
gyp verb extracted file from tarball include/node/openssl/ec.h
gyp verb extracted file from tarball include/node/openssl/ecdh.h
gyp verb extracted file from tarball include/node/openssl/ecdsa.h
gyp verb extracted file from tarball include/node/openssl/ecerr.h
gyp verb extracted file from tarball include/node/openssl/engine.h
gyp verb extracted file from tarball include/node/openssl/engineerr.h
gyp verb extracted file from tarball include/node/openssl/err.h
gyp verb extracted file from tarball include/node/openssl/evp.h
gyp verb extracted file from tarball include/node/openssl/evperr.h
gyp verb extracted file from tarball include/node/openssl/hmac.h
gyp verb extracted file from tarball include/node/openssl/idea.h
gyp verb extracted file from tarball include/node/openssl/kdf.h
gyp verb extracted file from tarball include/node/openssl/kdferr.h
gyp verb extracted file from tarball include/node/openssl/lhash.h
gyp verb extracted file from tarball include/node/openssl/md2.h
gyp verb extracted file from tarball include/node/openssl/md4.h
gyp verb extracted file from tarball include/node/openssl/md5.h
gyp verb extracted file from tarball include/node/openssl/mdc2.h
gyp verb extracted file from tarball include/node/openssl/modes.h
gyp verb extracted file from tarball include/node/openssl/obj_mac.h
gyp verb extracted file from tarball include/node/openssl/objects.h
gyp verb extracted file from tarball include/node/openssl/objectserr.h
gyp verb extracted file from tarball include/node/openssl/ocsp.h
gyp verb extracted file from tarball include/node/openssl/ocsperr.h
gyp verb extracted file from tarball include/node/openssl/opensslv.h
gyp verb extracted file from tarball include/node/openssl/ossl_typ.h
gyp verb extracted file from tarball include/node/openssl/pem.h
gyp verb extracted file from tarball include/node/openssl/pem2.h
gyp verb extracted file from tarball include/node/openssl/pemerr.h
gyp verb extracted file from tarball include/node/openssl/pkcs12.h
gyp verb extracted file from tarball include/node/openssl/pkcs12err.h
gyp verb extracted file from tarball include/node/openssl/pkcs7.h
gyp verb extracted file from tarball include/node/openssl/pkcs7err.h
gyp verb extracted file from tarball include/node/openssl/rand.h
gyp verb extracted file from tarball include/node/openssl/rand_drbg.h
gyp verb extracted file from tarball include/node/openssl/randerr.h
gyp verb extracted file from tarball include/node/openssl/rc2.h
gyp verb extracted file from tarball include/node/openssl/rc4.h
gyp verb extracted file from tarball include/node/openssl/rc5.h
gyp verb extracted file from tarball include/node/openssl/ripemd.h
gyp verb extracted file from tarball include/node/openssl/rsa.h
gyp verb extracted file from tarball include/node/openssl/rsaerr.h
gyp verb extracted file from tarball include/node/openssl/safestack.h
gyp verb extracted file from tarball include/node/openssl/seed.h
gyp verb extracted file from tarball include/node/openssl/sha.h
gyp verb extracted file from tarball include/node/openssl/srp.h
gyp verb extracted file from tarball include/node/openssl/srtp.h
gyp verb extracted file from tarball include/node/openssl/ssl.h
gyp verb extracted file from tarball include/node/openssl/ssl2.h
gyp verb extracted file from tarball include/node/openssl/ssl3.h
gyp verb extracted file from tarball include/node/openssl/sslerr.h
gyp verb extracted file from tarball include/node/openssl/stack.h
gyp verb extracted file from tarball include/node/openssl/store.h
gyp verb extracted file from tarball include/node/openssl/storeerr.h
gyp verb extracted file from tarball include/node/openssl/symhacks.h
gyp verb extracted file from tarball include/node/openssl/tls1.h
gyp verb extracted file from tarball include/node/openssl/ts.h
gyp verb extracted file from tarball include/node/openssl/tserr.h
gyp verb extracted file from tarball include/node/openssl/txt_db.h
gyp verb extracted file from tarball include/node/openssl/ui.h
gyp verb extracted file from tarball include/node/openssl/uierr.h
gyp verb extracted file from tarball include/node/openssl/whrlpool.h
gyp verb extracted file from tarball include/node/openssl/x509.h
gyp verb extracted file from tarball include/node/openssl/x509_vfy.h
gyp verb extracted file from tarball include/node/openssl/x509err.h
gyp verb extracted file from tarball include/node/openssl/x509v3.h
gyp verb extracted file from tarball include/node/openssl/x509v3err.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-elf/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64le/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86_64/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64A/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix64-gcc/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin64-x86_64-cc/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/aix-gcc/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x86_64/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris64-x86_64-gcc/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/BSD-x86/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-aarch64/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-armv4/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm/crypto/include/internal/bn_conf.h
gyp verb content checksum node-v13.5.0-headers.tar.gz c18ee69d7706e97c5e7ed3ffdfef50b2cf66e773b2168a03a3dc4170b0475d49
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/solaris-x86-gcc/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-mips64/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/darwin-i386-cc/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-ppc64/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux-x32/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN32/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux64-s390x/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm_avx2/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm_avx2/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm_avx2/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm_avx2/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm_avx2/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/linux32-s390x/asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64-ARM/no-asm/crypto/include/internal/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64-ARM/no-asm/crypto/include/internal/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64-ARM/no-asm/crypto/buildinf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64-ARM/no-asm/include/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/archs/VC-WIN64-ARM/no-asm/include/progs.h
gyp verb extracted file from tarball include/node/openssl/bn_conf.h
gyp verb extracted file from tarball include/node/openssl/bn_conf_asm.h
gyp verb extracted file from tarball include/node/openssl/bn_conf_no-asm.h
gyp verb extracted file from tarball include/node/openssl/dso_conf.h
gyp verb extracted file from tarball include/node/openssl/dso_conf_asm.h
gyp verb extracted file from tarball include/node/openssl/dso_conf_no-asm.h
gyp verb extracted file from tarball include/node/openssl/opensslconf.h
gyp verb extracted file from tarball include/node/openssl/opensslconf_asm.h
gyp verb extracted file from tarball include/node/openssl/opensslconf_no-asm.h
gyp verb extracted file from tarball include/node/zconf.h
gyp verb extracted file from tarball include/node/zlib.h
gyp verb tarball done parsing tarball
gyp verb check download content checksum, need to download `SHASUMS256.txt`...
gyp verb checksum url https://unofficial-builds.nodejs.org/download/release/v13.5.0/SHASUMS256.txt
gyp http GET https://unofficial-builds.nodejs.org/download/release/v13.5.0/SHASUMS256.txt
gyp http 200 https://unofficial-builds.nodejs.org/download/release/v13.5.0/SHASUMS256.txt
gyp verb checksum data {"node-v13.5.0-headers.tar.gz":"c18ee69d7706e97c5e7ed3ffdfef50b2cf66e773b2168a03a3dc4170b0475d49","node-v13.5.0-headers.tar.xz":"885d4c9b3a37f608117cfa9ae1ab834635fccbbb16831cad996e868b371a32a6","node-v13.5.0-linux-armv6l.tar.gz":"7fb25cc48d8a88da43465380dbe48216f5e480fd9f6d7224f48a26241b090fcf","node-v13.5.0-linux-armv6l.tar.xz":"ed5dcba22fa3c3e3721972d15e158a37b0bf6769e2442d9926473609a4afcb63","node-v13.5.0-linux-x64-musl.tar.gz":"706723648ceba4c12b636ad3dc0a4af20c5685f2192a4aa17b28927c2ea310e9","node-v13.5.0-linux-x64-musl.tar.xz":"1d476cfc39cd7ebb1bcb40ab5bf9934ef27f6780ff2b47e31db67b15471c560f","node-v13.5.0-linux-x86.tar.gz":"f858ce1e109494f2d5bc0350ace1088e8b0e9ad3a85b910b02a7fd3483568eae","node-v13.5.0-linux-x86.tar.xz":"d5399cd902586479a2479ad445b4cb63913ee81cbc8e7fd2ce7b34d1afab1e5a"}
gyp verb download contents checksum {"node-v13.5.0-headers.tar.gz":"c18ee69d7706e97c5e7ed3ffdfef50b2cf66e773b2168a03a3dc4170b0475d49"}
gyp verb validating download checksum for node-v13.5.0-headers.tar.gz (c18ee69d7706e97c5e7ed3ffdfef50b2cf66e773b2168a03a3dc4170b0475d49 == c18ee69d7706e97c5e7ed3ffdfef50b2cf66e773b2168a03a3dc4170b0475d49)
gyp verb get node dir target node version installed: 13.5.0
gyp verb build dir attempting to create "build" dir: /usr/src/app/node_modules/node-sass/build
gyp verb build dir "build" dir needed to be created? /usr/src/app/node_modules/node-sass/build
gyp verb build/config.gypi creating config file
gyp verb build/config.gypi writing out config file: /usr/src/app/node_modules/node-sass/build/config.gypi
gyp verb config.gypi checking for gypi file: /usr/src/app/node_modules/node-sass/config.gypi
gyp verb common.gypi checking for gypi file: /usr/src/app/node_modules/node-sass/common.gypi
gyp verb gyp gyp format was not specified; forcing "make"
gyp info spawn /usr/bin/python2
gyp info spawn args [
gyp info spawn args   '/usr/src/app/node_modules/node-gyp/gyp/gyp_main.py',
gyp info spawn args   'binding.gyp',
gyp info spawn args   '-f',
gyp info spawn args   'make',
gyp info spawn args   '-I',
gyp info spawn args   '/usr/src/app/node_modules/node-sass/build/config.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/usr/src/app/node_modules/node-gyp/addon.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/root/.node-gyp/13.5.0/include/node/common.gypi',
gyp info spawn args   '-Dlibrary=shared_library',
gyp info spawn args   '-Dvisibility=default',
gyp info spawn args   '-Dnode_root_dir=/root/.node-gyp/13.5.0',
gyp info spawn args   '-Dnode_gyp_dir=/usr/src/app/node_modules/node-gyp',
gyp info spawn args   '-Dnode_lib_file=/root/.node-gyp/13.5.0/<(target_arch)/node.lib',
gyp info spawn args   '-Dmodule_root_dir=/usr/src/app/node_modules/node-sass',
gyp info spawn args   '-Dnode_engine=v8',
gyp info spawn args   '--depth=.',
gyp info spawn args   '--no-parallel',
gyp info spawn args   '--generator-output',
gyp info spawn args   'build',
gyp info spawn args   '-Goutput_dir=.'
gyp info spawn args ]
gyp verb command build []
gyp verb build type Release
gyp verb architecture x64
gyp verb node dev dir /root/.node-gyp/13.5.0
gyp verb `which` succeeded for `make` /usr/bin/make
gyp info spawn make
gyp info spawn args [ 'V=1', 'BUILDTYPE=Release', '-C', 'build' ]
make: Entering directory '/usr/src/app/node_modules/node-sass/build'
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/ast.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/ast.o ../src/libsass/src/ast.cpp

  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/ast_fwd_decl.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/ast_fwd_decl.o ../src/libsass/src/ast_fwd_decl.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/backtrace.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/backtrace.o ../src/libsass/src/backtrace.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/base64vlq.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/base64vlq.o ../src/libsass/src/base64vlq.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/bind.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/bind.o ../src/libsass/src/bind.cpp
  cc '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node -I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include-I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer  -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/cencode.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/cencode.o ../src/libsass/src/cencode.c
../src/libsass/src/cencode.c: In function 'base64_encode_block':
../src/libsass/src/cencode.c:48:11: warning: this statement may fall through [-Wimplicit-fallthrough=]
   48 |    result = (fragment & 0x003) << 4;
      |    ~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~
../src/libsass/src/cencode.c:52:2: note: here
   52 |  case step_B:
      |  ^~~~
../src/libsass/src/cencode.c:62:11: warning: this statement may fall through [-Wimplicit-fallthrough=]
   62 |    result = (fragment & 0x00f) << 2;
      |    ~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~
../src/libsass/src/cencode.c:66:2: note: here
   66 |  case step_C:
      |  ^~~~
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/check_nesting.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/check_nesting.o ../src/libsass/src/check_nesting.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/color_maps.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/color_maps.o ../src/libsass/src/color_maps.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/constants.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/constants.o ../src/libsass/src/constants.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/context.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/context.o ../src/libsass/src/context.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/cssize.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/cssize.o ../src/libsass/src/cssize.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/emitter.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/emitter.o ../src/libsass/src/emitter.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/environment.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/environment.o ../src/libsass/src/environment.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/error_handling.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/error_handling.o ../src/libsass/src/error_handling.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/eval.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/eval.o ../src/libsass/src/eval.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/expand.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/expand.o ../src/libsass/src/expand.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/extend.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/extend.o ../src/libsass/src/extend.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/file.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/file.o ../src/libsass/src/file.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/functions.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/functions.o ../src/libsass/src/functions.cpp
../src/libsass/src/functions.cpp: In function 'void Sass::Functions::handle_utf8_error(const Sass::ParserState&, Sass::Backtraces)':
../src/libsass/src/functions.cpp:110:20: warning: catching polymorphic type 'class utf8::invalid_code_point' by value [-Wcatch-value=]
  110 |       catch (utf8::invalid_code_point) {
      |                    ^~~~~~~~~~~~~~~~~~
../src/libsass/src/functions.cpp:114:20: warning: catching polymorphic type 'class utf8::not_enough_room' by value [-Wcatch-value=]
  114 |       catch (utf8::not_enough_room) {
      |                    ^~~~~~~~~~~~~~~
../src/libsass/src/functions.cpp:118:20: warning: catching polymorphic type 'class utf8::invalid_utf8' by value [-Wcatch-value=]
  118 |       catch (utf8::invalid_utf8) {
      |                    ^~~~~~~~~~~~
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/inspect.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/inspect.o ../src/libsass/src/inspect.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/json.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/json.o ../src/libsass/src/json.cpp
../src/libsass/src/json.cpp: In function 'char* json_encode_string(const char*)':
../src/libsass/src/json.cpp:405:15: warning: catching polymorphic type 'class std::exception' by value [-Wcatch-value=]
  405 |   catch (std::exception) {
      |               ^~~~~~~~~
../src/libsass/src/json.cpp: In function 'char* json_stringify(const JsonNode*, const char*)':
../src/libsass/src/json.cpp:424:15: warning: catching polymorphic type 'class std::exception' by value [-Wcatch-value=]
  424 |   catch (std::exception) {
      |               ^~~~~~~~~
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/lexer.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/lexer.o ../src/libsass/src/lexer.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/listize.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/listize.o ../src/libsass/src/listize.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/memory/SharedPtr.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/memory/SharedPtr.o ../src/libsass/src/memory/SharedPtr.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/node.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/node.o ../src/libsass/src/node.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/operators.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/operators.o ../src/libsass/src/operators.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/output.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/output.o ../src/libsass/src/output.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/parser.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/parser.o ../src/libsass/src/parser.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/plugins.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/plugins.o ../src/libsass/src/plugins.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/position.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/position.o ../src/libsass/src/position.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/prelexer.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/prelexer.o ../src/libsass/src/prelexer.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/remove_placeholders.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/remove_placeholders.o ../src/libsass/src/remove_placeholders.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/sass.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/sass.o ../src/libsass/src/sass.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/sass2scss.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/sass2scss.o ../src/libsass/src/sass2scss.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/sass_context.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/sass_context.o ../src/libsass/src/sass_context.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/sass_functions.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/sass_functions.o ../src/libsass/src/sass_functions.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/sass_util.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/sass_util.o ../src/libsass/src/sass_util.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/sass_values.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/sass_values.o ../src/libsass/src/sass_values.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/source_map.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/source_map.o ../src/libsass/src/source_map.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/subset_map.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/subset_map.o ../src/libsass/src/subset_map.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/to_c.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/to_c.o ../src/libsass/src/to_c.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/to_value.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/to_value.o ../src/libsass/src/to_value.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/units.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/units.o ../src/libsass/src/units.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/utf8_string.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/utf8_string.o ../src/libsass/src/utf8_string.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/util.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/util.o ../src/libsass/src/util.cpp
  g++ '-DNODE_GYP_MODULE_NAME=libsass' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DLIBSASS_VERSION="3.5.4"' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -std=gnu++1y -std=c++0x -fexceptions -frtti -MMD -MF ./Release/.deps/Release/obj.target/libsass/src/libsass/src/values.o.d.raw   -c -o Release/obj.target/libsass/src/libsass/src/values.o ../src/libsass/src/values.cpp
  rm -f Release/obj.target/src/sass.a && ar crs Release/obj.target/src/sass.a Release/obj.target/libsass/src/libsass/src/ast.o Release/obj.target/libsass/src/libsass/src/ast_fwd_decl.o Release/obj.target/libsass/src/libsass/src/backtrace.o Release/obj.target/libsass/src/libsass/src/base64vlq.o Release/obj.target/libsass/src/libsass/src/bind.o Release/obj.target/libsass/src/libsass/src/cencode.o Release/obj.target/libsass/src/libsass/src/check_nesting.o Release/obj.target/libsass/src/libsass/src/color_maps.o Release/obj.target/libsass/src/libsass/src/constants.o Release/obj.target/libsass/src/libsass/src/context.o Release/obj.target/libsass/src/libsass/src/cssize.o Release/obj.target/libsass/src/libsass/src/emitter.o Release/obj.target/libsass/src/libsass/src/environment.o Release/obj.target/libsass/src/libsass/src/error_handling.o Release/obj.target/libsass/src/libsass/src/eval.o Release/obj.target/libsass/src/libsass/src/expand.o Release/obj.target/libsass/src/libsass/src/extend.o Release/obj.target/libsass/src/libsass/src/file.o Release/obj.target/libsass/src/libsass/src/functions.o Release/obj.target/libsass/src/libsass/src/inspect.o Release/obj.target/libsass/src/libsass/src/json.o Release/obj.target/libsass/src/libsass/src/lexer.o Release/obj.target/libsass/src/libsass/src/listize.o Release/obj.target/libsass/src/libsass/src/memory/SharedPtr.o Release/obj.target/libsass/src/libsass/src/node.o Release/obj.target/libsass/src/libsass/src/operators.o Release/obj.target/libsass/src/libsass/src/output.o Release/obj.target/libsass/src/libsass/src/parser.o Release/obj.target/libsass/src/libsass/src/plugins.o Release/obj.target/libsass/src/libsass/src/position.o Release/obj.target/libsass/src/libsass/src/prelexer.o Release/obj.target/libsass/src/libsass/src/remove_placeholders.o Release/obj.target/libsass/src/libsass/src/sass.o Release/obj.target/libsass/src/libsass/src/sass2scss.o Release/obj.target/libsass/src/libsass/src/sass_context.o Release/obj.target/libsass/src/libsass/src/sass_functions.o Release/obj.target/libsass/src/libsass/src/sass_util.o Release/obj.target/libsass/src/libsass/src/sass_values.o Release/obj.target/libsass/src/libsass/src/source_map.o Release/obj.target/libsass/src/libsass/src/subset_map.o Release/obj.target/libsass/src/libsass/src/to_c.o Release/obj.target/libsass/src/libsass/src/to_value.o Release/obj.target/libsass/src/libsass/src/units.o Release/obj.target/libsass/src/libsass/src/utf8_string.o Release/obj.target/libsass/src/libsass/src/util.o Release/obj.target/libsass/src/libsass/src/values.o
  rm -rf "Release/sass.a" && cp -af "Release/obj.target/src/sass.a" "Release/sass.a"
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/binding.o.d.raw   -c -o Release/obj.target/binding/src/binding.o ../src/binding.cpp
In file included from ../src/binding.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
../src/binding.cpp: In function 'Nan::NAN_METHOD_RETURN_TYPE render(Nan::NAN_METHOD_ARGS_TYPE)':
../src/binding.cpp:284:98: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
  284 |     int status = uv_queue_work(uv_default_loop(), &ctx_w->request, compile_it, (uv_after_work_cb)MakeCallback);
      |                                                                                                  ^~~~~~~~~~~~
../src/binding.cpp: In function 'Nan::NAN_METHOD_RETURN_TYPE render_file(Nan::NAN_METHOD_ARGS_TYPE)':
../src/binding.cpp:320:98: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
  320 |     int status = uv_queue_work(uv_default_loop(), &ctx_w->request, compile_it, (uv_after_work_cb)MakeCallback);
      |                                                                                                  ^~~~~~~~~~~~
In file included from ../../nan/nan.h:54,
                 from ../src/binding.cpp:1:
../src/binding.cpp: At global scope:
/root/.node-gyp/13.5.0/include/node/node.h:611:43: warning: cast between incompatible function types from 'void (*)(Nan::ADDON_REGISTER_FUNCTION_ARGS_TYPE)' {aka 'void(*)(v8::Local<v8::Object>)'} to 'node::addon_register_func' {aka 'void (*)(v8::Local<v8::Object>, v8::Local<v8::Value>, void*)'} [-Wcast-function-type]
  611 |       (node::addon_register_func) (regfunc),                          \
      |                                           ^
/root/.node-gyp/13.5.0/include/node/node.h:645:3: note: in expansion of macro 'NODE_MODULE_X'
  645 |   NODE_MODULE_X(modname, regfunc, NULL, 0)  // NOLINT (readability/null_usage)
      |   ^~~~~~~~~~~~~
../src/binding.cpp:358:1: note: in expansion of macro 'NODE_MODULE'
  358 | NODE_MODULE(binding, RegisterModule);
      | ^~~~~~~~~~~
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/binding.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/create_string.o.d.raw   -c -o Release/obj.target/binding/src/create_string.o ../src/create_string.cpp
In file included from ../src/create_string.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/create_string.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/custom_function_bridge.o.d.raw  -c -o Release/obj.target/binding/src/custom_function_bridge.o ../src/custom_function_bridge.cpp
In file included from ../src/custom_function_bridge.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/custom_function_bridge.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/custom_importer_bridge.o.d.raw  -c -o Release/obj.target/binding/src/custom_importer_bridge.o ../src/custom_importer_bridge.cpp
In file included from ../src/custom_importer_bridge.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/custom_importer_bridge.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_context_wrapper.o.d.raw-c -o Release/obj.target/binding/src/sass_context_wrapper.o ../src/sass_context_wrapper.cpp
In file included from ../src/sass_context_wrapper.h:6,
                 from ../src/sass_context_wrapper.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_context_wrapper.h:6,
                 from ../src/sass_context_wrapper.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/boolean.o.d.raw   -c -o Release/obj.target/binding/src/sass_types/boolean.o ../src/sass_types/boolean.cpp
In file included from ../src/sass_types/boolean.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/boolean.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/color.o.d.raw   -c -o Release/obj.target/binding/src/sass_types/color.o ../src/sass_types/color.cpp
In file included from ../src/sass_types/color.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/color.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/error.o.d.raw   -c -o Release/obj.target/binding/src/sass_types/error.o ../src/sass_types/error.cpp
In file included from ../src/sass_types/error.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/error.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/factory.o.d.raw   -c -o Release/obj.target/binding/src/sass_types/factory.o ../src/sass_types/factory.cpp
In file included from ../src/sass_types/factory.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/factory.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/list.o.d.raw   -c -o Release/obj.target/binding/src/sass_types/list.o ../src/sass_types/list.cpp
In file included from ../src/sass_types/list.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/list.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/map.o.d.raw   -c -oRelease/obj.target/binding/src/sass_types/map.o ../src/sass_types/map.cpp
In file included from ../src/sass_types/map.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/map.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/null.o.d.raw   -c -o Release/obj.target/binding/src/sass_types/null.o ../src/sass_types/null.cpp
In file included from ../src/sass_types/null.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/null.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/number.o.d.raw   -c-o Release/obj.target/binding/src/sass_types/number.o ../src/sass_types/number.cpp
In file included from ../src/sass_types/number.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/number.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ '-DNODE_GYP_MODULE_NAME=binding' '-DUSING_UV_SHARED=1' '-DUSING_V8_SHARED=1' '-DV8_DEPRECATION_WARNINGS=1' '-DV8_DEPRECATION_WARNINGS' '-DV8_IMMINENT_DEPRECATION_WARNINGS' '-D_LARGEFILE_SOURCE' '-D_FILE_OFFSET_BITS=64' '-DOPENSSL_NO_PINSHARED' '-DOPENSSL_THREADS' '-DBUILDING_NODE_EXTENSION' -I/root/.node-gyp/13.5.0/include/node-I/root/.node-gyp/13.5.0/src -I/root/.node-gyp/13.5.0/deps/openssl/config -I/root/.node-gyp/13.5.0/deps/openssl/openssl/include -I/root/.node-gyp/13.5.0/deps/uv/include -I/root/.node-gyp/13.5.0/deps/zlib -I/root/.node-gyp/13.5.0/deps/v8/include -I../../nan -I../src/libsass/include  -fPIC -pthread -Wall -Wextra -Wno-unused-parameter -m64 -O3 -fno-omit-frame-pointer -fno-rtti -fno-exceptions -std=gnu++1y -std=c++0x -MMD -MF ./Release/.deps/Release/obj.target/binding/src/sass_types/string.o.d.raw   -c-o Release/obj.target/binding/src/sass_types/string.o ../src/sass_types/string.cpp
In file included from ../src/sass_types/string.cpp:1:
../../nan/nan.h: In function 'void Nan::AsyncQueueWorker(Nan::AsyncWorker*)':
../../nan/nan.h:2298:62: warning: cast between incompatible function types from 'void (*)(uv_work_t*)' {aka 'void (*)(uv_work_s*)'} to 'uv_after_work_cb' {aka 'void (*)(uv_work_s*, int)'} [-Wcast-function-type]
 2298 |     , reinterpret_cast<uv_after_work_cb>(AsyncExecuteComplete)
      |                                                              ^
In file included from /root/.node-gyp/13.5.0/include/node/node.h:63,
                 from ../../nan/nan.h:54,
                 from ../src/sass_types/string.cpp:1:
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = node::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)]':
/root/.node-gyp/13.5.0/include/node/node_object_wrap.h:84:78:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<node::ObjectWrap>::Callback' {aka 'void(*)(const v8::WeakCallbackInfo<node::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
10400 |                reinterpret_cast<Callback>(callback), type);
      |                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/root/.node-gyp/13.5.0/include/node/v8.h: In instantiation of 'void v8::PersistentBase<T>::SetWeak(P*, typename v8::WeakCallbackInfo<P>::Callback, v8::WeakCallbackType) [with P = Nan::ObjectWrap; T = v8::Object; typename v8::WeakCallbackInfo<P>::Callback = void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)]':
../../nan/nan_object_wrap.h:65:61:   required from here
/root/.node-gyp/13.5.0/include/node/v8.h:10400:16: warning: cast between incompatible function types from 'v8::WeakCallbackInfo<Nan::ObjectWrap>::Callback' {aka 'void (*)(const v8::WeakCallbackInfo<Nan::ObjectWrap>&)'} to 'Callback' {aka 'void (*)(const v8::WeakCallbackInfo<void>&)'} [-Wcast-function-type]
  g++ -shared -pthread -rdynamic -m64  -Wl,-soname=binding.node -o Release/obj.target/binding.node -Wl,--start-group Release/obj.target/binding/src/binding.o Release/obj.target/binding/src/create_string.o Release/obj.target/binding/src/custom_function_bridge.o Release/obj.target/binding/src/custom_importer_bridge.o Release/obj.target/binding/src/sass_context_wrapper.o Release/obj.target/binding/src/sass_types/boolean.o Release/obj.target/binding/src/sass_types/color.o Release/obj.target/binding/src/sass_types/error.o Release/obj.target/binding/src/sass_types/factory.o Release/obj.target/binding/src/sass_types/list.o Release/obj.target/binding/src/sass_types/map.oRelease/obj.target/binding/src/sass_types/null.o Release/obj.target/binding/src/sass_types/number.o Release/obj.target/binding/src/sass_types/string.o Release/obj.target/src/sass.a -Wl,--end-group
  rm -rf "Release/binding.node" && cp -af "Release/obj.target/binding.node" "Release/binding.node"
make: Leaving directory '/usr/src/app/node_modules/node-sass/build'
gyp info ok
Installed to /usr/src/app/node_modules/node-sass/vendor/linux_musl-x64-79/binding.node
npm WARN rating-web@1.4.0 No repository field.
npm WARN The package autoprefixer is included as both a dev and production dependency.
npm WARN The package babel-core is included as both a dev and production dependency.
npm WARN The package babel-loader is included as both a dev and production dependency.
npm WARN The package babel-preset-env is included as both a dev and production dependency.
npm WARN The package babel-preset-stage-2 is included as both a dev and production dependency.
npm WARN The package compression-webpack-plugin is included as both a dev and production dependency.
npm WARN The package cross-env is included as both a dev and production dependency.
npm WARN The package css-loader is included as both a dev and production dependency.
npm WARN The package file-loader is included as both a dev and production dependency.
npm WARN The package html-webpack-plugin is included as both a dev and production dependency.
npm WARN The package node-sass is included as both a dev and production dependency.
npm WARN The package postcss-loader is included as both a dev and production dependency.
npm WARN The package rimraf is included as both a dev and production dependency.
npm WARN The package sass-loader is included as both a dev and production dependency.
npm WARN The package style-loader is included as both a dev and production dependency.
npm WARN The package url-loader is included as both a dev and production dependency.
npm WARN The package vue is included as both a dev and production dependency.
npm WARN The package vue-loader is included as both a dev and production dependency.
npm WARN The package vue-style-loader is included as both a dev and production dependency.
npm WARN The package webpack is included as both a dev and production dependency.
npm WARN The package webpack-dev-server is included as both a dev and production dependency.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.9 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.9: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

added 1152 packages from 681 contributors and audited 1222 packages in 188.947s
found 505 vulnerabilities (495 low, 4 moderate, 6 high)
  run `npm audit fix` to fix them, or `npm audit` for details
Removing intermediate container 563b36c212bf
 ---> 7e9b9f18f9c4
Step 6/8 : COPY . .
 ---> f0a08012dc00
Step 7/8 : EXPOSE 8080
 ---> Running in 4194b12b6af3
Removing intermediate container 4194b12b6af3
 ---> 906712ff6085
Step 8/8 : CMD [ "npm", "start"]
 ---> Running in a7e86c5b68f3
Removing intermediate container a7e86c5b68f3
 ---> e5191206d98f
Successfully built e5191206d98f
Successfully tagged acr7839.azurecr.io/ratings-web:v1
2021/01/02 21:24:52 Successfully executed container: build
2021/01/02 21:24:52 Executing step ID: push. Timeout(sec): 3600, Working directory: '', Network: ''
2021/01/02 21:24:52 Pushing image: acr7839.azurecr.io/ratings-web:v1, attempt 1
The push refers to repository [acr7839.azurecr.io/ratings-web]
564c07fa2d0d: Preparing
21a20436c76c: Preparing
3472615b3d0e: Preparing
6d27413be3bf: Preparing
9246f9309cd9: Preparing
efd6e0da275f: Preparing
b352b61d0fe4: Preparing
d06ff5e5272b: Preparing
6b27de954cca: Preparing
b352b61d0fe4: Waiting
efd6e0da275f: Waiting
d06ff5e5272b: Waiting
6b27de954cca: Waiting
3472615b3d0e: Pushed
9246f9309cd9: Pushed
564c07fa2d0d: Pushed
efd6e0da275f: Pushed
b352b61d0fe4: Pushed
6b27de954cca: Pushed
d06ff5e5272b: Pushed
21a20436c76c: Pushed
6d27413be3bf: Pushed
v1: digest: sha256:9ba7e50bd6566e3997b4c0e69665f322e24d7156f6087ef36d8055eb052b2844 size: 2209
2021/01/02 21:25:24 Successfully pushed image: acr7839.azurecr.io/ratings-web:v1
2021/01/02 21:25:24 Step ID: build marked as successful (elapsed time in seconds: 222.395981)
2021/01/02 21:25:24 Populating digests for step ID: build...
2021/01/02 21:25:26 Successfully populated digests for step ID: build
2021/01/02 21:25:26 Step ID: push marked as successful (elapsed time in seconds: 31.992890)
2021/01/02 21:25:26 The following dependencies were found:
2021/01/02 21:25:26
- image:
    registry: acr7839.azurecr.io
    repository: ratings-web
    tag: v1
    digest: sha256:9ba7e50bd6566e3997b4c0e69665f322e24d7156f6087ef36d8055eb052b2844
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 13.5-alpine
    digest: sha256:a5a7ff4267a810a019c7c3732b3c463a892a61937d84ee952c34af2fb486058d
  git: {}

Run ID: ca2 was successful after 4m21s
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$ az acr repository list --name $ACR_NAME --output table
Result
-----------
ratings-api
ratings-web
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$ az aks update \
>   --name $AKS_CLUSTER_NAME \
>   --resource-group $RESOURCE_GROUP \
>   --attach-acr $ACR_NAME
{- Finished ..
  "aadProfile": null,
  "addonProfiles": {
    "httpApplicationRouting": {
      "config": {
        "HTTPApplicationRoutingZoneName": "f172fe29680645f682f9.eastus.aksapp.io"
      },
      "enabled": true,
      "identity": {
        "clientId": "78b68ecf-1b67-4f2c-9caa-a2afb5872ab2",
        "objectId": "8dd07c1e-9daa-4e03-8e0a-138d7993483f",
        "resourceId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.ManagedIdentity/userAssignedIdentities/httpapplicationrouting-aksworkshop-2016"
      }
    }
  },
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "count": 2,
      "enableAutoScaling": null,
      "enableNodePublicIp": false,
      "maxCount": null,
      "maxPods": 30,
      "minCount": null,
      "mode": "System",
      "name": "nodepool1",
      "nodeImageVersion": "AKSUbuntu-1804containerd-2020.12.15",
      "nodeLabels": {},
      "nodeTaints": null,
      "orchestratorVersion": "1.19.3",
      "osDiskSizeGb": 128,
      "osDiskType": "Managed",
      "osType": "Linux",
      "powerState": {
        "code": "Running"
      },
      "provisioningState": "Succeeded",
      "proximityPlacementGroupId": null,
      "scaleSetEvictionPolicy": null,
      "scaleSetPriority": null,
      "spotMaxPrice": null,
      "tags": null,
      "type": "VirtualMachineScaleSets",
      "upgradeSettings": null,
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/virtualNetworks/aks-vnet/subnets/aks-subnet"
    }
  ],
  "apiServerAccessProfile": null,
  "autoScalerProfile": null,
  "diskEncryptionSetId": null,
  "dnsPrefix": "aksworksho-aksworkshop-c70c6f",
  "enablePodSecurityPolicy": null,
  "enableRbac": true,
  "fqdn": "aksworksho-aksworkshop-c70c6f-18e76dd9.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/aksworkshop/providers/Microsoft.ContainerService/managedClusters/aksworkshop-2016",
  "identity": {
    "principalId": "d4458ce1-1627-4238-8703-4863c508862c",
    "tenantId": "ec34354a-7cbd-41c6-88ec-1b55a6393a74",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "e03de4d1-222f-4f7f-918e-437c6f2f732d",
      "objectId": "9412bc10-5f71-412c-b914-2b51a81be0d0",
      "resourceId": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourcegroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.ManagedIdentity/userAssignedIdentities/aksworkshop-2016-agentpool"
    }
  },
  "kubernetesVersion": "1.19.3",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzZ2slqLUV3nBt6U7k8gTTdC3bsb5JcKfo+86QV3HnisbtRtAZz6c6INqkftvuR5bN1LWrbjP2qEF3BVjRgylkQqbdlTSYRdWctJwkj4EIeomCn6DxzvKmd47n2qqVENo+ULNUTwSxa618NqRMP3zbW/lSHk4Q8Y/EGeldh6wwrb07tkuTo80D+EcvkUvHHlbRHflyN2Zpjdk4DCZDtCbDU1YTM0iuGRaVs/bvpBm2L8A80XwjrsNNJeMuaWdX20XI+xqQBi2U34uMAL41QpfzzP75RM5mDS583M1RHS57L5aOPaVyc8MIm/ggZ1YiigIHmg2drRUs5ZRvU8zWEpI7"
        }
      ]
    }
  },
  "location": "eastus",
  "maxAgentPools": 10,
  "name": "aksworkshop-2016",
  "networkProfile": {
    "dnsServiceIp": "10.2.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerProfile": {
      "allocatedOutboundPorts": null,
      "effectiveOutboundIps": [
        {
          "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/MC_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.Network/publicIPAddresses/f8980e4a-21c3-4deb-ae1a-0d2c94be459d",
          "resourceGroup": "MC_aksworkshop_aksworkshop-2016_eastus"
        }
      ],
      "idleTimeoutInMinutes": null,
      "managedOutboundIps": {
        "count": 1
      },
      "outboundIpPrefixes": null,
      "outboundIps": null
    },
    "loadBalancerSku": "Standard",
    "networkMode": null,
    "networkPlugin": "azure",
    "networkPolicy": null,
    "outboundType": "loadBalancer",
    "podCidr": null,
    "serviceCidr": "10.2.0.0/24"
  },
  "nodeResourceGroup": "MC_aksworkshop_aksworkshop-2016_eastus",
  "powerState": {
    "code": "Running"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "aksworkshop",
  "servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  },
  "sku": {
    "name": "Basic",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters",
  "windowsProfile": {
    "adminPassword": null,
    "adminUsername": "azureuser",
    "licenseType": null
  }
}
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$
vamsi@Azure:~/aks-workshop/mslearn-aks-workshop-ratings-web$ cd ..
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ ls
aks-workshop.md      mslearn-aks-workshop-ratings-api  ratings-api-service.yaml      ratings-web-ingress-bkp.yaml                       ratings-web-ingress.yaml
cluster-issuer.yaml  mslearn-aks-workshop-ratings-web  ratings-web-deployment1.yaml  ratings-web-ingress-old.yaml                       ratings-web-service-old.yaml
Kubernetes           rating-api-deployment.yaml        ratings-web-deployment.yaml   ratings-web-ingress-use-without-customdomain.yaml  ratings-web-service.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" already exists with the same configuration, skipping
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm search repo bitnami
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION
bitnami/bitnami-common                  0.0.9           0.0.9           DEPRECATED Chart with custom templates used in ...
bitnami/airflow                         7.1.0           1.10.14         Apache Airflow is a platform to programmaticall...
bitnami/apache                          8.0.3           2.4.46          Chart for Apache HTTP Server
bitnami/aspnet-core                     1.1.0           3.1.9           ASP.NET Core is an open-source framework create...
bitnami/cassandra                       7.1.2           3.11.9          Apache Cassandra is a free and open-source dist...
bitnami/common                          1.2.3           1.2.3           A Library Helm Chart for grouping common logic ...
bitnami/consul                          9.1.0           1.9.1           Highly available and distributed service discov...
bitnami/contour                         3.2.0           1.11.0          Contour Ingress controller for Kubernetes
bitnami/discourse                       2.1.0           2.6.0           A Helm chart for deploying Discourse to Kubernetes
bitnami/dokuwiki                        10.0.5          20200729.0.0    DokuWiki is a standards-compliant, simple to us...
bitnami/drupal                          10.0.7          9.1.0           One of the most versatile open source content m...
bitnami/ejbca                           2.0.3           6.15.2-6        Enterprise class PKI Certificate Authority buil...
bitnami/elasticsearch                   13.0.5          7.10.1          A highly scalable open-source full-text search ...
bitnami/etcd                            5.4.1           3.4.14          etcd is a distributed key value store that prov...
bitnami/external-dns                    4.5.0           0.7.5           ExternalDNS is a Kubernetes addon that configur...
bitnami/fluentd                         3.2.1           1.11.5          Fluentd is an open source data collector for un...
bitnami/ghost                           11.1.9          3.40.2          A simple, powerful publishing platform that all...
bitnami/grafana                         4.2.4           7.3.6           Grafana is an open source, feature rich metrics...
bitnami/harbor                          9.2.1           2.1.2           Harbor is an an open source trusted cloud nativ...
bitnami/influxdb                        1.1.6           1.8.3           InfluxDB is an open source time-series database...
bitnami/jasperreports                   10.0.1          7.8.0           The JasperReports server can be used as a stand...
bitnami/jenkins                         7.0.3           2.263.1         The leading open source automation server
bitnami/joomla                          9.0.5           3.9.23          PHP content management system (CMS) for publish...
bitnami/kafka                           12.5.0          2.7.0           Apache Kafka is a distributed streaming platform.
bitnami/keycloak                        1.1.5           11.0.3          Keycloak is a high performance Java-based ident...
bitnami/kiam                            0.2.0           3.6.0           kiam is a proxy that captures AWS Metadata API ...
bitnami/kibana                          6.2.2           7.10.1          Kibana is an open source, browser based analyti...
bitnami/kong                            3.1.0           2.2.1           Kong is a scalable, open source API layer (aka ...
bitnami/kube-prometheus                 3.3.2           0.44.1          kube-prometheus collects Kubernetes manifests t...
bitnami/kube-state-metrics              1.1.3           1.9.7           kube-state-metrics is a simple service that lis...
bitnami/kubeapps                        5.0.0           2.0.1           Kubeapps is a dashboard for your Kubernetes clu...
bitnami/kubernetes-event-exporter       1.0.5           0.9.0           This tool allows exporting the often missed Kub...
bitnami/kubewatch                       3.0.2           0.1.0           Kubewatch is a Kubernetes watcher that currentl...
bitnami/logstash                        2.0.2           7.10.1          Logstash is an open source, server-side data pr...
bitnami/magento                         16.0.2          2.4.1           A feature-rich flexible e-commerce solution. It...
bitnami/mariadb                         9.2.0           10.5.8          Fast, reliable, scalable, and easy to use open-...
bitnami/mariadb-cluster                 1.0.2           10.2.14         DEPRECATED Chart to create a Highly available M...
bitnami/mariadb-galera                  5.3.4           10.5.8          MariaDB Galera is a multi-master database clust...
bitnami/mean                            6.1.2           4.6.2           DEPRECATED MEAN is a free and open-source JavaS...
bitnami/mediawiki                       12.0.3          1.35.1          Extremely powerful, scalable software and a fea...
bitnami/memcached                       5.4.1           1.6.9           Chart for Memcached
bitnami/metallb                         2.0.2           0.9.5           The Metal LB for Kubernetes
bitnami/metrics-server                  5.3.3           0.4.1           Metrics Server is a cluster-wide aggregator of ...
bitnami/minio                           4.1.11          2020.12.29      MinIO is an object storage server, compatible w...
bitnami/mongodb                         10.3.3          4.4.3           NoSQL document-oriented database that stores JS...
bitnami/mongodb-sharded                 3.2.7           4.4.3           NoSQL document-oriented database that stores JS...
bitnami/moodle                          10.1.3          3.10.0          Moodle is a learning platform designed to provi...
bitnami/mxnet                           2.1.2           1.7.0           A flexible and efficient library for deep learning
bitnami/mysql                           8.2.3           8.0.22          Chart to create a Highly available MySQL cluster
bitnami/nats                            6.0.3           2.1.9           An open-source, cloud-native messaging system
bitnami/nginx                           8.2.3           1.19.6          Chart for the nginx server
bitnami/nginx-ingress-controller        7.0.9           0.43.0          Chart for the nginx Ingress controller
bitnami/node                            14.1.0          14.15.3         Event-driven I/O server-side JavaScript environ...
bitnami/node-exporter                   2.1.2           1.0.1           Prometheus exporter for hardware and OS metrics...
bitnami/odoo                            17.0.2          14.0.20201210   A suite of web based open source business apps.
bitnami/opencart                        9.0.4           3.0.3-6         A free and open source e-commerce platform for ...
bitnami/orangehrm                       9.0.2           4.6.0-1         OrangeHRM is a free HR management system that o...
bitnami/osclass                         9.0.2           3.9.0           Osclass is a php script that allows you to quic...
bitnami/owncloud                        9.0.1           10.6.0          A file sharing server that puts the control and...
bitnami/parse                           13.2.3          4.9.3           Parse is a platform that enables users to add a...
bitnami/phabricator                     10.2.2          2020.42.0       Collection of open source web applications that...
bitnami/phpbb                           9.0.2           3.3.2           Community forum that supports the notion of use...
bitnami/phpmyadmin                      8.0.2           5.0.4           phpMyAdmin is an mysql administration frontend
bitnami/postgresql                      10.2.0          11.10.0         Chart for PostgreSQL, an object-relational data...
bitnami/postgresql-ha                   6.3.3           11.10.0         Chart for PostgreSQL with HA architecture (usin...
bitnami/prestashop                      12.1.3          1.7.7-0         A popular open source ecommerce solution. Profe...
bitnami/prometheus-operator             0.31.1          0.41.0          DEPRECATED The Prometheus Operator for Kubernet...
bitnami/pytorch                         2.1.0           1.7.0           Deep learning platform that accelerates the tra...
bitnami/rabbitmq                        8.6.1           3.8.9           Open source message broker software that implem...
bitnami/redis                           12.3.0          6.0.9           Open source, advanced key-value store. It is of...
bitnami/redis-cluster                   4.2.2           6.0.9           Open source, advanced key-value store. It is of...
bitnami/redmine                         15.0.3          4.1.1           A flexible project management web application.
bitnami/spark                           4.4.0           3.0.1           Spark is a fast and general-purpose cluster com...
bitnami/spring-cloud-dataflow           2.4.1           2.7.0           Spring Cloud Data Flow is a microservices-based...
bitnami/sugarcrm                        1.0.6           6.5.26          DEPRECATED SugarCRM enables businesses to creat...
bitnami/suitecrm                        9.1.1           7.11.18         SuiteCRM is a completely open source enterprise...
bitnami/tensorflow-inception            3.3.2           1.13.0          DEPRECATED Open-source software library for ser...
bitnami/tensorflow-resnet               3.1.1           2.4.0           Open-source software library serving the ResNet...
bitnami/testlink                        9.0.4           1.9.20          Web-based test management system that facilitat...
bitnami/thanos                          3.3.0           0.17.2          Thanos is a highly available metrics system tha...
bitnami/tomcat                          8.0.0           9.0.41          Chart for Apache Tomcat
bitnami/wavefront                       1.1.1           1.2.6           Chart for Wavefront Collector for Kubernetes
bitnami/wildfly                         7.0.1           21.0.2          Chart for Wildfly
bitnami/wordpress                       10.2.1          5.6.0           Web publishing platform for building blogs and ...
bitnami/zookeeper                       6.3.0           3.6.2           A centralized service for maintaining configura...
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm install ratings bitnami/mongodb --namespace ratingsapp --set auth.username=vamsee,auth.password=vamsee,auth.database=ratingsdb
NAME: ratings
LAST DEPLOYED: Sat Jan  2 21:32:03 2021
NAMESPACE: ratingsapp
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

MongoDB can be accessed on the following DNS name(s) and ports from within your cluster:

    ratings-mongodb.ratingsapp.svc.cluster.local

To get the root password run:

    export MONGODB_ROOT_PASSWORD=$(kubectl get secret --namespace ratingsapp ratings-mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 --decode)

To get the password for "vamsee" run:

    export MONGODB_PASSWORD=$(kubectl get secret --namespace ratingsapp ratings-mongodb -o jsonpath="{.data.mongodb-password}" | base64 --decode)

To connect to your database, create a MongoDB client container:

    kubectl run --namespace ratingsapp ratings-mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=$MONGODB_ROOT_PASSWORD" --image docker.io/bitnami/mongodb:4.4.3-debian-10-r0 --command -- bash

Then, run the following command:
    mongo admin --host "ratings-mongodb" --authenticationDatabase admin -u root -p $MONGODB_ROOT_PASSWORD

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace ratingsapp svc/ratings-mongodb 27017:27017 &
    mongo --host 127.0.0.1 --authenticationDatabase admin -p $MONGODB_ROOT_PASSWORD
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl create secret generic mongosecret --namespace ratingsapp --from-literal=MONGOCONNECTION="mongodb://vamsee:vamsee@ratings-mongodb.ratingsapp:27017/ratingsdb"
secret/mongosecret created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl describe secret mongosecret --namespace ratingsapp
Name:         mongosecret
Namespace:    ratingsapp
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
MONGOCONNECTION:  66 bytes
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ az acr list -o table
NAME     RESOURCE GROUP    LOCATION    SKU       LOGIN SERVER        CREATION DATE         ADMIN ENABLED
-------  ----------------  ----------  --------  ------------------  --------------------  ---------------
acr7839  aksworkshop       eastus      Standard  acr7839.azurecr.io  2021-01-02T21:16:56Z  False
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ code rating-api-deployment.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl apply \
>   --namespace ratingsapp \
>   -f rating-api-deployment.yaml
deployment.apps/ratings-api created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get pods \
>   --namespace ratingsapp \
>   -l app=ratings-api -w
NAME                           READY   STATUS              RESTARTS   AGE
ratings-api-744dd7fdcf-7hdtt   0/1     ContainerCreating   0          5s
ratings-api-744dd7fdcf-7hdtt   0/1     Running             0          20s
^Cvamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get pods --namespace ratingsapp
NAME                               READY   STATUS    RESTARTS   AGE
ratings-api-744dd7fdcf-7hdtt       1/1     Running   0          28s
ratings-mongodb-6b8d45bbb4-99pd6   1/1     Running   0          6m40s
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get deployment ratings-api --namespace ratingsapp
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ratings-api   1/1     1            1           46s
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ code ratings-api-service.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl apply --namespace ratingsapp -f ratings-api-service.yaml
service/ratings-api created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get service ratings-api --namespace ratingsapp
NAME          TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
ratings-api   ClusterIP   10.2.0.135   <none>        80/TCP    6s
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get endpoints ratings-api --namespace ratingsapp
NAME          ENDPOINTS          AGE
ratings-api   10.240.0.18:3000   26s
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ code ratings-web-deployment.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl apply \
>   --namespace ratingsapp \
>   -f ratings-web-deployment.yaml
deployment.apps/ratings-web created
vamsi@Azure:~/aks-workshop$ kubectl get pods --namespace ratingsapp -l app=ratings-web -w
NAME                          READY   STATUS              RESTARTS   AGE
ratings-web-c6d49ff5c-zwbq6   0/1     ContainerCreating   0          12s
ratings-web-c6d49ff5c-zwbq6   1/1     Running             0          30s
^Cvamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ code ratings-web-service.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl apply \
>   --namespace ratingsapp  \
>   -f ratings-web-service.yaml
service/ratings-web created
vamsi@Azure:~/aks-workshop$ kubectl get service ratings-web  --namespace ratingsapp
NAME          TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
ratings-web   ClusterIP   10.2.0.127   <none>        80/TCP    14s
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ Kubectl get namespace
bash: Kubectl: command not found
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get namespace
NAME              STATUS   AGE
default           Active   65m
kube-node-lease   Active   65m
kube-public       Active   65m
kube-system       Active   65m
ratingsapp        Active   31m
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl create namespace ingress
namespace/ingress created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get namespace
NAME              STATUS   AGE
default           Active   65m
ingress           Active   4s
kube-node-lease   Active   66m
kube-public       Active   66m
kube-system       Active   66m
ratingsapp        Active   31m
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm repo add stable https://charts.helm.sh/stable
"stable" already exists with the same configuration, skipping
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm install nginx-ingress stable/nginx-ingress \
>     --namespace ingress \
>     --set controller.replicaCount=2 \
>     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
>     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
WARNING: This chart is deprecated
NAME: nginx-ingress
LAST DEPLOYED: Sat Jan  2 21:48:12 2021
NAMESPACE: ingress
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
*******************************************************************************************************
* DEPRECATED, please use https://github.com/kubernetes/ingress-nginx/tree/master/charts/ingress-nginx *
*******************************************************************************************************


The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace ingress get services -o wide -w nginx-ingress-controller'

An example Ingress that makes use of the controller:

  apiVersion: extensions/v1beta1
  kind: Ingress
  metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: example
    namespace: foo
  spec:
    rules:
      - host: www.example.com
        http:
          paths:
            - backend:
                serviceName: exampleService
                servicePort: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
        - hosts:
            - www.example.com
          secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get service nginx-ingress-controller --namespace ingress -w
NAME                       TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)                      AGE
nginx-ingress-controller   LoadBalancer   10.2.0.181   52.146.82.159   80:30299/TCP,443:30214/TCP   6s
^Cvamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get service nginx-ingress-controller --namespace ingress
NAME                       TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)                      AGE
nginx-ingress-controller   LoadBalancer   10.2.0.181   52.146.82.159   80:30299/TCP,443:30214/TCP   112s
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ az network dns zone create \
>   --resource-group aksworkshop \
>   --name devopssource.com
{
  "etag": "00000002-0000-0000-1e4f-8e9f51e1d601",
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/dnszones/devopssource.com",
  "location": "global",
  "maxNumberOfRecordSets": 10000,
  "name": "devopssource.com",
  "nameServers": [
    "ns1-02.azure-dns.com.",
    "ns2-02.azure-dns.net.",
    "ns3-02.azure-dns.org.",
    "ns4-02.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "registrationVirtualNetworks": null,
  "resolutionVirtualNetworks": null,
  "resourceGroup": "aksworkshop",
  "tags": {},
  "type": "Microsoft.Network/dnszones",
  "zoneType": "Public"
}
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ az network dns record-set a add-record \
>    --resource-group aksworkshop \
>    --record-set-name @ \
>    --zone-name devopssource.com \
>    --ipv4-address 1.1.1.1
{
  "arecords": [
    {
      "ipv4Address": "1.1.1.1"
    }
  ],
  "etag": "144cce46-735c-4e64-9196-d328b309ae89",
  "fqdn": "devopssource.com.",
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/dnszones/devopssource.com/A/@",
  "metadata": null,
  "name": "@",
  "provisioningState": "Succeeded",
  "resourceGroup": "aksworkshop",
  "targetResource": {
    "id": null
  },
  "ttl": 3600,
  "type": "Microsoft.Network/dnszones/A"
}
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ PUBLIC_IP_ID=$(az network public-ip list --query "[?ipAddress=='52.146.82.159'].id" -o tsv)
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ az network dns record-set a update --name @ \
>   --resource-group aksworkshop \
>   --zone-name devopssource.com \
>   --target-resource $PUBLIC_IP_ID
{
  "etag": "4f09c4e4-0fc2-4b06-b1da-c12c5ae8f721",
  "fqdn": "devopssource.com.",
  "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/aksworkshop/providers/Microsoft.Network/dnszones/devopssource.com/A/@",
  "metadata": null,
  "name": "@",
  "provisioningState": "Succeeded",
  "resourceGroup": "aksworkshop",
  "targetResource": {
    "id": "/subscriptions/c70c6f54-be39-4053-a47e-8a6aec89da7a/resourceGroups/mc_aksworkshop_aksworkshop-2016_eastus/providers/Microsoft.Network/publicIPAddresses/kubernetes-a3a191284b93a483b9d90db636c691cc",
    "resourceGroup": "mc_aksworkshop_aksworkshop-2016_eastus"
  },
  "ttl": 3600,
  "type": "Microsoft.Network/dnszones/A"
}
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
+===============================================+
| Add Azure Nameservers in godaddy dns settings |
+===============================================+
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl create namespace cert-manager
namespace/cert-manager created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm repo add jetstack https://charts.jetstack.io
"jetstack" already exists with the same configuration, skipping
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "jetstack" chart repository
...Successfully got an update from the "bitnami" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.14/deploy/manifests/00-crds.yaml
Warning: apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
customresourcedefinition.apiextensions.k8s.io/certificaterequests.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/certificates.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/challenges.acme.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/clusterissuers.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/issuers.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/orders.acme.cert-manager.io created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ helm install cert-manager \
>   --namespace cert-manager \
>   --version v0.14.0 \
>   jetstack/cert-manager
W0102 21:58:04.202665     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:04.410559     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:04.617681     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:04.825956     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:05.033861     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:05.241457     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:05.450076     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:06.075264     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:06.281803     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:06.488886     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:06.695842     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:06.904406     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:07.111259     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:07.318414     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:07.526228     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 Role is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 Role
W0102 21:58:07.734384     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 Role is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 Role
W0102 21:58:07.941455     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 RoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 RoleBinding
W0102 21:58:08.148147     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 RoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 RoleBinding
W0102 21:58:09.395345     865 warnings.go:67] admissionregistration.k8s.io/v1beta1 MutatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 MutatingWebhookConfiguration
W0102 21:58:09.602936     865 warnings.go:67] admissionregistration.k8s.io/v1beta1 ValidatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 ValidatingWebhookConfiguration
W0102 21:58:10.704858     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:10.704942     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:10.704999     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:10.705062     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:10.705129     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:10.705171     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:10.705231     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
W0102 21:58:10.913059     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:10.913060     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:10.914367     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:10.914390     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:10.914491     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:10.914497     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:10.914369     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
W0102 21:58:11.130603     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 Role is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 Role
W0102 21:58:11.130627     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 Role is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 Role
W0102 21:58:11.339784     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 RoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 RoleBinding
W0102 21:58:11.339784     865 warnings.go:67] rbac.authorization.k8s.io/v1beta1 RoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 RoleBinding
W0102 21:58:11.998615     865 warnings.go:67] admissionregistration.k8s.io/v1beta1 MutatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 MutatingWebhookConfiguration
W0102 21:58:12.208998     865 warnings.go:67] admissionregistration.k8s.io/v1beta1 ValidatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 ValidatingWebhookConfiguration
NAME: cert-manager
LAST DEPLOYED: Sat Jan  2 21:57:50 2021
NAMESPACE: cert-manager
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
cert-manager has been deployed successfully!

In order to begin issuing certificates, you will need to set up a ClusterIssuer
or Issuer resource (for example, by creating a 'letsencrypt-staging' issuer).

More information on the different types of issuers and how to configure them
can be found in our documentation:

https://docs.cert-manager.io/en/latest/reference/issuers.html

For information on how to configure cert-manager to automatically provision
Certificates for Ingress resources, take a look at the `ingress-shim`
documentation:

https://docs.cert-manager.io/en/latest/reference/ingress-shim.html
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl get pods --namespace cert-manager
NAME                                      READY   STATUS    RESTARTS   AGE
cert-manager-56c84569c9-4wj8v             1/1     Running   0          28s
cert-manager-cainjector-97596cdd4-zgdnx   1/1     Running   0          28s
cert-manager-webhook-54bff44696-k2mcn     0/1     Running   0          28s
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ code cluster-issuer.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl apply \
>   --namespace ratingsapp \
>   -f cluster-issuer.yaml
clusterissuer.cert-manager.io/letsencrypt created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ code ratings-web-ingress.yaml
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl describe cert ratings-web-cert --namespace ratingsapp
Error from server (NotFound): certificates.cert-manager.io "ratings-web-cert" not found
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl apply \
>   --namespace ratingsapp \
>   -f ratings-web-ingress.yaml
Warning: networking.k8s.io/v1beta1 Ingress is deprecated in v1.19+, unavailable in v1.22+; use networking.k8s.io/v1 Ingress
ingress.networking.k8s.io/ratings-web-ingress created
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$ kubectl describe cert ratings-web-cert --namespace ratingsapp
Name:         ratings-web-cert
Namespace:    ratingsapp
Labels:       <none>
Annotations:  <none>
API Version:  cert-manager.io/v1alpha3
Kind:         Certificate
Metadata:
  Creation Timestamp:  2021-01-02T22:05:51Z
  Generation:          1
  Managed Fields:
    API Version:  cert-manager.io/v1alpha2
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:ownerReferences:
          .:
          k:{"uid":"c34966fa-3500-40ec-bba3-edf34c04218d"}:
            .:
            f:apiVersion:
            f:blockOwnerDeletion:
            f:controller:
            f:kind:
            f:name:
            f:uid:
      f:spec:
        .:
        f:dnsNames:
        f:issuerRef:
          .:
          f:group:
          f:kind:
          f:name:
        f:secretName:
      f:status:
        .:
        f:conditions:
    Manager:    controller
    Operation:  Update
    Time:       2021-01-02T22:05:51Z
  Owner References:
    API Version:           extensions/v1beta1
    Block Owner Deletion:  true
    Controller:            true
    Kind:                  Ingress
    Name:                  ratings-web-ingress
    UID:                   c34966fa-3500-40ec-bba3-edf34c04218d
  Resource Version:        13793
  Self Link:               /apis/cert-manager.io/v1alpha3/namespaces/ratingsapp/certificates/ratings-web-cert
  UID:                     23508580-2f27-44a6-b1da-1729c3646c41
Spec:
  Dns Names:
    frontend.52.146.82.159.nip.io
    devopssource.com
  Issuer Ref:
    Group:      cert-manager.io
    Kind:       ClusterIssuer
    Name:       letsencrypt
  Secret Name:  ratings-web-cert
Status:
  Conditions:
    Last Transition Time:  2021-01-02T22:05:51Z
    Message:               Waiting for CertificateRequest "ratings-web-cert-2254125980" to complete
    Reason:                InProgress
    Status:                False
    Type:                  Ready
Events:
  Type    Reason        Age   From          Message
  ----    ------        ----  ----          -------
  Normal  GeneratedKey  8s    cert-manager  Generated a new private key
  Normal  Requested     8s    cert-manager  Created new CertificateRequest resource "ratings-web-cert-2254125980"
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
vamsi@Azure:~/aks-workshop$
  