# A demonstration of a local k8s cluster using (*k*)ubernetes (*in*) (*d*)ocker

## Pre-reqs
To use kind, we need go and docker installed...

## Configuring cluster
To get started, we need a kind cluster to use for the demonstration. Kind is a go application, and can be installed on any machine with go installed via:  
`GO111MODULE="on" go get sigs.k8s.io/kind@v0.16.0` (go < 1.17)  
or  
`go install sigs.k8s.io/kind@v0.16.0` (go >= 1.17)  
Next, we define the configuration for the cluster, using the following *kind-config.yml*:  
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
- role: worker
```
This will create a cluster with one control and two worker nodes, with one worker node having its' port 80 forwarded to port 80 on the VM, allowing external access to the cluster to test the running application(s).
