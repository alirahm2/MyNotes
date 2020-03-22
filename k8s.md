# Kubectl useful commands

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

### Scale replica set
`$ kb scale --replicas=0  replicaset [replicaset name]`
