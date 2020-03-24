# Kubectl useful commands

### Load local image in Minikube local registry
[link 1](https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-with-minikube)

### Deploy a file 
` $ kb create -f [file name] `

### Replace a new deployment
` $ kb replace -f [file name] `

### Set the list of all configurations
` $ kb get all `

### Get cluster info
`$ kb get cluster-info`

### Get current context(connected cluster)
`$ kubectl config current-context`

### switch current context(connected cluster)
`$ kb config use-context [context name]`

### Get all ReplicaSets
`$ kb get replicasets`

### get deployment info
`$ kb get deployment`

### Get all namespaces
`$ kubectl get pods --all-namespaces `

### Set current namespace
`$ kubectl config set-context --current --namespace=my-namespace`

### Get pods in different namespace
`$ kb get pods --namespace=[namespace name]`

### Create deployment in different namespace than default 
`$ kb create -f [file name] --namespace=[namespace name]`

### Create namespace
`$ kb create namespace [namespace name]`





















