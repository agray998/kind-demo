# A demonstration of a local k8s cluster using *k*ubernetes *in* *d*ocker

## Pre-reqs
To use kind, we need go and docker installed...

## Configuring cluster
To get started, we need a kind cluster to use for the demonstration. Kind is a go application, and can be installed on any machine with go installed via:  
`GO111MODULE="on" go get sigs.k8s.io/kind@v0.16.0` (go < 1.17)  
or  
`go install sigs.k8s.io/kind@v0.16.0` (go >= 1.17)
