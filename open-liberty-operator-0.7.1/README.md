# Install Open Liberty Operator

1. Follow tutorial [Quickstart: Deploy an Azure Kubernetes Service (AKS) cluster using the Azure portal](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal) to create an AKS cluster.

2. Follow instructions below to install Open Liberty Operator V0.7.1:

   ```bash
   # Install Custom Resource Definitions (CRDs)
   kubectl apply -f https://raw.githubusercontent.com/OpenLiberty/open-liberty-operator/master/deploy/releases/0.7.1/openliberty-app-crd.yaml

   # Set operator namespace and the namespace to watch:
   OPERATOR_NAMESPACE=default
   WATCH_NAMESPACE='""'

   # Install cluster-level role-based access
   curl -L https://raw.githubusercontent.com/OpenLiberty/open-liberty-operator/master/deploy/releases/0.7.1/openliberty-app-cluster-rbac.yaml \
       | sed -e "s/OPEN_LIBERTY_OPERATOR_NAMESPACE/${OPERATOR_NAMESPACE}/" \
       | kubectl apply -f -

   # Install the operator
   curl -L https://raw.githubusercontent.com/OpenLiberty/open-liberty-operator/master/deploy/releases/0.7.1/openliberty-app-operator.yaml \
       | sed -e "s/OPEN_LIBERTY_WATCH_NAMESPACE/${WATCH_NAMESPACE}/" \
       | kubectl apply -n ${OPERATOR_NAMESPACE} -f -
   ```

   You can also see detailed installation guide from [here](https://github.com/OpenLiberty/open-liberty-operator/tree/master/deploy/releases/0.7.1).
