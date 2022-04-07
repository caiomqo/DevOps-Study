# Deploy NeuVector with Helm

## List all Helm Repositories

helm repo list

## Add NeuVector Helm Repo

helm repo add neuvector https://neuvector.github.io/neuvector-helm/
helm search repo neuvector/core

## Install Helm Chart while modifying valules.yaml

helm install caio-nv \
--namespace neuvector \
neuvector/core \
--set registry=docker.io \
--set tag=5.0.0-preview.3 \
--set manager.image.repository=neuvector/manager.preview \
--set controller.image.repository=neuvector/controller.preview \
--set enforcer.image.repository=neuvector/enforcer.preview \
--set cve.scanner.image.repository=neuvector/scanner.preview \
--set cve.scanner.image.tag=latest \
--set cve.updater.image.repository=neuvector/updater.preview \
--set cve.updater.image.tag=latest

## List all installed Helm Charts in all Namespaces

helm list -A

## Uninstall Helm Chart

helm uninstall caio-nv neuvector/core --namespace neuvector

# If Using Local Helm Chart

## Clone Git Repo

## Modify values.yaml

## Deploy Local Chart

helm install caio-nv core/ --values core/values.yaml --namespace neuvector 