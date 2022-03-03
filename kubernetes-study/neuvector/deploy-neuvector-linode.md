# Official NeuVector Documentation
https://open-docs.neuvector.com/

## Download kubeconfig from Linode Cluster Page
    linode.com

## Change kubectl cluster
    export KUBECONFIG=~/.kube/neuvector-kubeconfig.yaml

## Useful commands
    kubectl get nodes
    kubectl cluster-info

## Running a single container (pod)
    kubectl run nameofpod --image=<image in docker hub>


# Running neuvector on linode

## Create namespace
kubectl create namespace neuvector

## Create custom CRDs
    kubectl apply -f https://raw.githubusercontent.com/neuvector/manifests/main/kubernetes/5.0.0/crd-k8s-1.19.yaml
    kubectl apply -f https://raw.githubusercontent.com/neuvector/manifests/main/kubernetes/5.0.0/waf-crd-k8s-1.19.yaml
    kubectl apply -f https://raw.githubusercontent.com/neuvector/manifests/main/kubernetes/5.0.0/admission-crd-k8s-1.19.yaml

## Add read permissions to access k8s
    kubectl create clusterrole neuvector-binding-app --verb=get,list,watch,update --resource=nodes,pods,services,namespaces
    kubectl create clusterrole neuvector-binding-rbac --verb=get,list,watch --resource=rolebindings.rbac.authorization.k8s.io,roles.rbac.authorization.k8s.io,clusterrolebindings.rbac.authorization.k8s.io,clusterroles.rbac.authorization.k8s.io
    kubectl create clusterrolebinding neuvector-binding-app --clusterrole=neuvector-binding-app --serviceaccount=neuvector:default
    kubectl create clusterrolebinding neuvector-binding-rbac --clusterrole=neuvector-binding-rbac --serviceaccount=neuvector:default
    kubectl create clusterrole neuvector-binding-admission --verb=get,list,watch,create,update,delete --resource=validatingwebhookconfigurations,mutatingwebhookconfigurations
    kubectl create clusterrolebinding neuvector-binding-admission --clusterrole=neuvector-binding-admission --serviceaccount=neuvector:default
    kubectl create clusterrole neuvector-binding-customresourcedefinition --verb=watch,create,get,update --resource=customresourcedefinitions
    kubectl create clusterrolebinding  neuvector-binding-customresourcedefinition --clusterrole=neuvector-binding-customresourcedefinition --serviceaccount=neuvector:default
    kubectl create clusterrole neuvector-binding-nvsecurityrules --verb=list,delete --resource=nvsecurityrules,nvclustersecurityrules
    kubectl create clusterrolebinding neuvector-binding-nvsecurityrules --clusterrole=neuvector-binding-nvsecurityrules --serviceaccount=neuvector:default
    kubectl create clusterrolebinding neuvector-binding-view --clusterrole=view --serviceaccount=neuvector:default
    kubectl create rolebinding neuvector-admin --clusterrole=admin --serviceaccount=neuvector:default -n neuvector
    kubectl create clusterrole neuvector-binding-nvwafsecurityrules --verb=list,delete --resource=nvwafsecurityrules
    kubectl create clusterrolebinding neuvector-binding-nvwafsecurityrules --clusterrole=neuvector-binding-nvwafsecurityrules --serviceaccount=neuvector:default
    kubectl create clusterrole neuvector-binding-nvadmissioncontrolsecurityrules --verb=list,delete --resource=nvadmissioncontrolsecurityrules
    kubectl create clusterrolebinding neuvector-binding-nvadmissioncontrolsecurityrules --clusterrole=neuvector-binding-nvadmissioncontrolsecurityrules --serviceaccount=neuvector:default

## Delete old bindings (if applicable)
    kubectl delete clusterrolebinding neuvector-binding
    kubectl delete clusterrole neuvector-binding

## Check if neuvector service account is added
    kubectl get clusterrolebinding  | grep neuvector
    kubectl get rolebinding -n neuvector | grep neuvector

## Create services and pods (Docker Runtime)
    kubectl apply -f https://raw.githubusercontent.com/neuvector/manifests/main/kubernetes/5.0.0/neuvector-docker-k8s.yaml

## Check if services are running
    kubectl get svc -n neuvector



# Deploying demo application

## Create demo namespace 
    kubectl create namespace demo

## Deploy yaml files
    kubectl apply -f redis.yaml
    kubectl apply -f nodejs.yaml
    kubectl apply -f nginx.yaml

## Check services
    kubectl get svc -n demo

## From here can open 
    https://<public-ip>:<mapped-port>