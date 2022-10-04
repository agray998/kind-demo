
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

## Configuring Cluster
To get started, we need a kind cluster to use for the demonstration. Kind is a go application, and can be installed on any machine with go installed via:  
```shell
GO111MODULE="on" go get sigs.k8s.io/kind@v0.16.0 # (go < 1.17) 
```
or  
```shell
go install sigs.k8s.io/kind@v0.16.0 # (go >= 1.17) 
```
Once kind is installed, it will need to be added to PATH:
```shell
export PATH=$PATH:$(go env GOPATH)/bin
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
  - containerPort: 32080
    hostPort: 80
- role: worker
```
This defines a cluster with one control and two worker nodes, with one worker node having its' port 32080 forwarded to port 80 on the VM, allowing external access to the cluster to test the running application(s).
To create this cluster, simply run:  
```shell
kind create cluster --config=kind-config.yml
```
Once the cluster is created, it can be managed through kubectl. If kubectl is not already installed, install it following the appropriate installation steps for your system. On Ubuntu kubectl may be installed as a snap, via:
```shell
sudo snap install kubectl --classic
```

## Deploying Example
NOTE: Hosting the kind cluster takes up a fair amount of disk space, if you find your machine does not have sufficient space to pull the images for the example, you can mount an additional data disk, and configure docker to store images there.
To demonstrate the cluster in action we can deploy the provided example, for now a simple one pod per service deployment of [this application](https://github.com/agray998/QA-DevOps-Practical-Project):
```shell
cd prac-proj
declare -a services=(db name unit effect frontend)
for service in ${services[@]}; do kubectl apply -f ${service}.yaml; done
```
The application should now be available at port 80 on the virtual machine hosting the cluster.
