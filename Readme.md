# DSA Deploy for AKS Nodes

##  Requirements

Tested on AKS cluster with 1.15.10 k8s version. Agent Nodes configuration:

~~~sh
OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
Ubuntu 16.04.6 LTS   4.15.0-1071-azure   docker://3.0.10+azure
~~~

About Linux Kernel support: [https://help.deepsecurity.trendmicro.com/agent-linux-kernel-support.html](https://help.deepsecurity.trendmicro.com/agent-linux-kernel-support.html)

## Usage

### Build your on container image:

~~~
docker build -t <your registry path>/<container_name>:<tag> .
docker push <your registry path>/<container_name>:<tag>
~~~


### Configure the image on daemonsset.yaml file

Change the line 20 (image section) on the k8s/daemonset.yaml to your registry. 

~~~yaml
- image: <your container build>
~~~

### Configure your DSM Activation infos on configmap.yaml file

Configure to use your on Deep Security Manager activation configuration (k8s/configmap.yaml line 88):

~~~yaml
/opt/ds_agent/dsa_control -a $ACTIVATIONURL <your tenant activation configuration>
~~~

## Support

Official support from Trend Micro is not available. Individual contributors may be Trend Micro employees, but are not official support.

Original repo: [https://github.com/patnaikshekhar/AKSNodeInstaller](https://github.com/patnaikshekhar/AKSNodeInstaller)


# AKS Node Installer

Many customers want the ability to run arbitrary software on their AKS worker nodes such as Malware scanners, policy enforcers etc. This script which is heavily inspired by [Kured](https://github.com/weaveworks/kured) lets you do that. The script (in the container) is meant to run as a DaemonSet so that new nodes can be bootstrapped.

## Installation

Before installing update the script in k8s/sampleconfigmap.yaml which is run to install the software that you want in the cluster.

```sh
git clone https://github.com/patnaikshekhar/AKSNodeInstaller
cd AKSNodeInstaller
# Update script in ./k8s/sampleconfigmap.yaml to use your installation instructions
kubectl apply -f ./k8s
```

## Explanation
This [blog article](https://medium.com/@patnaikshekhar/initialize-your-aks-nodes-with-daemonsets-679fa81fd20e) explains the code.

## Demo
[![Youtube Demo](https://img.youtube.com/vi/vAIW4ZSP44I/0.jpg)](https://www.youtube.com/watch?v=vAIW4ZSP44I)
