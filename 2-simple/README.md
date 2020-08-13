# Deploy a simple Java EE application running on Open Liberty server to an AKS cluster

## Prerequisites

1. If you haven't created an AKS cluster and installed Open Liberty Operator on it, follow instructions [here](../open-liberty-operator-0.7.0/README.md) to complete these steps.
2. Follow tutorial [Connect to the cluster](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough#connect-to-the-cluster) to connect to the AKS cluster.
3. If you haven't created namespace `open-liberty-demo`, follow the instructions below to create it:

   ```bash
   kubectl create namespace open-liberty-demo
   ```

4. If you haven't pushed image `javaee-cafe-simple:1.0.0` to repositories of your personal DockerHub account, follow the relative instructions [here](https://github.com/Azure-Samples/open-liberty-on-aro/blob/master/guides/howto-deploy-java-openliberty-app.md#deploy-application-on-aro-4-cluster) to complete this step.

## Deployment

1. Follow instructions below to deploy the containerized application to the AKS cluster:

   ```bash
   # Change directory to 2-simple of your local clone
   cd <local-repo>/2-simple

   # Create environment variables which will be passed to OpenLibertyApplication "javaee-cafe-simple"
   # Note: replace "<Your_DockerHub_Account>" with your valid Docker Hub account name
   export Your_DockerHub_Account=<Your_DockerHub_Account>

   # Create OpenLibertyApplication "javaee-cafe-simple"
   envsubst < openlibertyapplication.yaml | kubectl create -f -
   ```
