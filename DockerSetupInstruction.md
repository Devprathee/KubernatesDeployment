
# Steps to build docker image.

##	Visual Studio Code
1.	[Downdload and install  visual studio code](https://code.visualstudio.com/download) 
 
1.	Install Extensions C#, Dockers and Remote – WSL

1.	Add Dockers file to workspace using command

1.	Run Switch Daemon incase if its didn't switch automatically

	'C:\Program Files\Docker\Docker\’ DockerCli.exe –SwitchDaemon

1.	Build docker image using command

	docker  build --rm -t imagename:version . --build-arg ASPNETCORE_ENVIRONMENT=Development

	OR

	docker build --rm --pull -f "C:\MyWorkSpace\Dockerfile" --label "com.microsoft.created-by=visual-studio-code" -t "imagename:verion" "C:\MyWorkSpace\Projects\MVC-branch\SRC\Dev\BCOWebApp" --build-arg ASPNETCORE_ENVIRONMENT=Development

1.	Dockers run using command
1.	Run command in new terminal

	docker run --rm -d -p 8009:5001/tcp imagename:verion
---
##	Install Docker for desktop
	[Link For Docker Desktop](https://docs.docker.com/docker-for-windows/install)
---
##	Install sdk .net core latest version
---

# Azure Cli Commands To Login

## Azure Container Registry Creation
1. Azure Login Command

 	 az login

1. Azure Container Registry Login
	
	az acr login -n acrName --expose-token

1. Login to Docker

docker login -u username -p password acrName.azurecr.io

username =clientid
password=client secrrent id

	
1. We need to tag local docker container to server registry before pushing into server else push won't work.

	docker tag imagename:version acrname.azurecr.io/repositoryname/imagename:version

1. Push Docker Image using Docker Command 

	docker push acrname.azurecr.io/repositoryname/imagename:version


1. Build and Push Image together

	az acr build --image imagename:version --registry acrname -f Dockerfile  
	

## Deployment to AKS 

[Documentation Link to deploy to AKS](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough)
1.To Install the kubelogin and kubectl using below command 

		az aks install-cli

		or Download KubeLogin and kubectl seperately from [Here](https://github.com/Azure/kubelogin/releases)

1. Login to azure 

az login --service-principal --username ********************* --password **************** --tenant ***********

az account set --subscription ********************

az aks get-credentials --resource-group resourcegroupname --name namespaceName

kubelogin convert-kubeconfig -l spn

1. Setting kube config path and user service principle details into service config. 


set KUBECONFIG=C:\Users\username\.kube\config

Set AAD_SERVICE_PRINCIPAL_CLIENT_ID=****************************

set AAD_SERVICE_PRINCIPAL_CLIENT_SECRET=******************************



1. cmd to Deploy into pods using static deployment file

	kubectl apply -f C:\MyWorkSpace\WebApp\bcowebapp-deployment-script.yaml

1. cmd to get the pods details 
   
	kubectl get pods -n namespaceName

1. cmd to get pod informatio used pod name

	kubectl describe pod podName -n namespace

1. cmd to get details of pods ,internal ips and service running 
	
	kubectl get service -n namespace --watch 

1. cmd set the namespace for the current context to avoid mentioning in all the commands

	kubectl config set-context --current --namespace=namespaceName

1. cmd to go to kubernetes daskboard

	kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard --user=clusteruser

1. Delete  user binding already available delete using below 
	
	kubectl delete clusterrolebinding kubernetes-dashboard

1. cmd to get logs based on the pod names

	kubectl log podname

1. cmd to open kubernetes dashboard and to authenticte user config file generated in kubernates local folder
	az aks browse --resource-group resoucegroupname --name youraks




