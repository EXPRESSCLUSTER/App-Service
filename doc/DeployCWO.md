# Deploy Cluster WebUI Offline on App Service

## Index
- [Prerequisite](#prerequisite)
- [Create a Container Registry on Azure](#create-a-container-registry-on-azure)
- [Build Cluster WebUI Offline on Linux Machine](#build-cluster-webui-offline-on-linux-machine)
- [Deploy Cluster WebUI Offline](#deploy-cluster-webui-offline)
- [Link](#link)

## Prerequisite
- Azure account.
- Linux machine to get a Docker image.

## Create a Container Registry on Azure
1. Click **Container registries** icon and click **Create**.
1. Set the following parameters and create the registry.
   - Resource group: your resource group
   - Registry name: your registry name
   - Location: select your location
1. Click the registry name click **Access keys** to check the following parameters.
   - Registry name
   - Username
   - password

## Build Cluster WebUI Offline on Linux Machine
1. Download the Cluster WebUI Offline binary file.
   - https://www.support.nec.co.jp/View.aspx?id=3140107045
1. Download [Dockerfile](https://github.com/EXPRESSCLUSTER/App-Service/tree/main/Dockerfile/cwo/4.2.2) and build the container image.
   - CLUSTERPRO X 4.2 for Windows
     ```sh
     docker build -t cwo-420-win-jp:4.2.1-200401-1 .
     ```
   - CLUSTERPRO X 4.2 for Linux
     ```sh
     docker build -t cwo-420-lin-jp:4.2.1-200401-1 .
     ```
1. Change the tag as below.
   ```sh
   docker tag cwo-420-win-jp:4.2.1-200401-1 <your registry name>.azurecr.io/cwo-420-win-jp:4.2.1-200401-1
   ```
1. Login to your registry.
   ```sh
   docker login <your registry name>.azurecr.io --username <your user name>
   Password: <enter your password>
   ```
1. Push the Cluster WebUI Offline container image.
   ```sh
   docker push <your registry name>.azurecr.io/clpwitnessd:4.2.1-200401-1ss
   ```

## Deploy Cluster WebUI Offline
1. Click **App Services** icon and click **Create**.
1. Set the following parameters and create the container.
   - Basics
     - Resource group: your resource group
     - Name: your application name (E.g. cwo-420-win-jp)
     - Publish: Docker Container
     - Operating System: Linux
     - Region: your region
   - Docker
    - Options: Single Container
    - Image Source: Azure Container Registry
    - Image: select image
    - Tag: select tag
1. Stop the container and change port number with Azure Cloud Shell.
1. Access to your container as below.
   - https://cwo-420-win-jp.azurewebsites.net

## Link
- Migrate custom software to Azure App Service using a custom container
  - https://docs.microsoft.com/en-us/azure/app-service/tutorial-custom-container?pivots=container-linux