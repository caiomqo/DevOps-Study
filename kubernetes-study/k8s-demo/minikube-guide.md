# Kubernetes and Minikube

## First Commands

    minikube start
    minikube status
    minikube ip

## Deployment

    kubectl apply -f mongo-config.yaml 
    kubectl apply -f mongo-secret.yaml 
    kubectl apply -f mongo.yaml 
    kubectl apply -f webapp.yaml 

## Listing

    kubectl get all -> shows all components in the cluster
    kubectl get configmap -> shows the configmap
    kubectl get secret ->  shows the secret

    kubectl --help -> shows documentation for kubectl

## Get more Details

    kubectl describe service webapp-service
    kubectl describe service mongo-service

    kubectl describe pod webapp-deployment-dcffd6bcc-dgcl8
    kubectl describe pod mongo-deployment-7875498c-8m7jb

    kubectl logs webapp-deployment-dcffd6bcc-dgcl8
    kubectl logs mongo-deployment-7875498c-8m7jb

## Get the Services

    kubectl get svc

## Get the Nodes

    kubectl get node
    kubectl get node -o wide

## Get the Pods

    kubectl get pod

