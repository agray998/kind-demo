
# A demonstration of a local k8s cluster using (*k*)ubernetes (*in*) (*d*)ocker

## Pre-reqs
To use kind, we need go and docker installed. Go may be readily installed via apt:
```shell
sudo apt update && sudo apt install -y golang # the apt package will usually have an older version of go than the most current version; this is not an issue for our purposes
```
To install docker, and add yourself to the docker group:
```shell
curl https://get.docker.com | sudo bash
sudo usermod -aG docker ${USER}
sudo reboot
```

## Configuring cluster
To get started, we need a kind cluster to use for the demonstration. Kind is a go application, and can be installed on any machine with go installed via:  
```shell
GO111MODULE="on" go get sigs.k8s.io/kind@v0.16.0 # (go < 1.17) 
```
or  
```shell
go install sigs.k8s.io/kind@v0.16.0 # (go >= 1.17) 
```
Next, we define the configuration for the cluster, using the following *kind-config.yml*:  
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: demo-kube-cluster
nodes:
- role: control-plane
- role: worker
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
- role: worker
```
This defines a cluster with one control and two worker nodes, with one worker node having its' port 80 forwarded to port 80 on the VM, allowing external access to the cluster to test the running application(s).
To create this cluster, simply run:  
```shell
kind create cluster --config=kind-config.yml
```
Once the cluster is created, it can be managed through kubectl. If kubectl is not already installed, install it following the appropriate installation steps for your system. On Ubuntu kubectl may be installed as a snap, via:
```shell
sudo snap install kubectl --classic
```
