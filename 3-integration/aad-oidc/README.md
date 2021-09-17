# Integrate your Liberty application with Azure Active Directory OpenID Connect

## Prerequisites

1. If you haven't created an AKS cluster and installed Open Liberty Operator on it, follow instructions [here](../../open-liberty-operator-0.7.1/README.md) to complete these steps.
2. Follow tutorial [Connect to the cluster](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough#connect-to-the-cluster) to connect to the AKS cluster.
3. If you haven't created namespace `open-liberty-demo`, follow the instructions below to create it:

   ```bash
   kubectl create namespace open-liberty-demo
   ```

4. Follow this [section](https://docs.microsoft.com/azure/aks/ingress-own-tls#create-an-ingress-controller) if you want to **Create an ingress controller** on the AKS cluster. Please replace **ingress-basic** with **open-liberty-demo** for the namespace used.
5. Set up Azure Active Directory by completing this [section](https://github.com/Azure-Samples/open-liberty-on-aro/blob/master/guides/howto-integrate-aad-oidc.md#set-up-azure-active-directory).

## Deployment

1. Follow instructions below to build and deploy the containerized application to the AKS cluster:

   ```bash
   # Change directory to "<path-to-repo>/3-integration/aad-oidc"
   cd <path-to-repo>/3-integration/aad-oidc

   # Build war package
   mvn clean package

   # Note: replace "<Your_DockerHub_Account>" with your valid Docker Hub account name
   export Your_DockerHub_Account=<Your_DockerHub_Account>

   # Build and tag application image
   # Note:
   # - replace "${Docker_File}" with "Dockerfile" to build application image with Open Liberty base image
   # - replace "${Docker_File}" with "Dockerfile-wlp" to build application image with WebSphere Liberty base image
   docker build -t javaee-cafe-aad-oidc-ingress:1.0.0 --pull --file=${Docker_File} .

   # Create a new tag with registry info that refers to source image
   docker tag javaee-cafe-aad-oidc-ingress:1.0.0 docker.io/${Your_DockerHub_Account}/javaee-cafe-aad-oidc-ingress:1.0.0

   # Log in to the container image registry
   docker login

   # Push image to the container image registry
   docker push docker.io/${Your_DockerHub_Account}/javaee-cafe-aad-oidc-ingress:1.0.0

   # Note: replace "<client ID>", "<client secret>", "<tenant ID>", "<group ID>", "<ingress controller ip address>", and "<domain name>" with the ones you noted down before
   export CLIENT_ID=<client ID>
   export CLIENT_SECRET=<client secret>
   export TENANT_ID=<tenant ID>
   export ADMIN_GROUP_ID=<group ID>
   export DOMAIN_NAME=<domain name>

   # Create secret "aad-oidc-secret"
   envsubst < aad-oidc-secret.yaml | kubectl create -f -

   # Create TLS private key and certificate, which is also used as CA certificate for testing purpose
   openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 \
       -keyout tls.key \
       -out tls.crt \
       -subj "/CN=${DOMAIN_NAME}/O=liberty-demo-tls"

   # Create environment variables which will be passed to secret "tls-crt-secret"
   export CA_CRT=$(cat tls.crt | base64 -w 0)
   export DEST_CA_CRT=$(cat tls.crt | base64 -w 0)
   export TLS_CRT=$(cat tls.crt | base64 -w 0)
   export TLS_KEY=$(cat tls.key | base64 -w 0)

   # Create secret "tls-crt-secret"
   envsubst < tls-crt-secret.yaml | kubectl create -f -

   # Note: change "USE_INGRESS_CONTROLLER" to false if ingress controller is not installed
   USE_INGRESS_CONTROLLER=true

   # Create OpenLibertyApplication "javaee-cafe-aad-ldap"
   if [ "$USE_INGRESS_CONTROLLER" = true ]; then
       export INGRESS_IP_ADDRESS=<ingress controller ip address>

       # Create OpenLibertyApplication "javaee-cafe-aad-oidc-ingress"
       envsubst < openlibertyapplication-ingress.yaml | kubectl create -f -

       # Create ingress "aad-oidc-ingress"
       envsubst < aad-oidc-ingress.yaml | kubectl create -f -

       # Test the ingress configuration
       # curl -v -k --resolve ${DOMAIN_NAME}:443:${INGRESS_IP_ADDRESS} https://${DOMAIN_NAME}
   else
       envsubst < openlibertyapplication.yaml | kubectl create -f -
   fi
   ```
