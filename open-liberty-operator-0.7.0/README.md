Follow instructions below to install Open Liberty Operator:

```bash
# Install Custom Resource Definitions (CRDs)
kubectl apply -f openliberty-app-crd.yaml

# Set operator namespace and the namespace to watch:
OPERATOR_NAMESPACE=default
WATCH_NAMESPACE='""'

# Install cluster-level role-based access
sed -i "s/OPEN_LIBERTY_OPERATOR_NAMESPACE/${OPERATOR_NAMESPACE}/" openliberty-app-cluster-rbac.yaml
kubectl apply -f openliberty-app-cluster-rbac.yaml

# Install the operator
sed -i "s/OPEN_LIBERTY_WATCH_NAMESPACE/${WATCH_NAMESPACE}/" openliberty-app-operator.yaml
kubectl apply -f openliberty-app-operator.yaml
```

You can also see detailed installation guide from [here](https://github.com/OpenLiberty/open-liberty-operator/tree/master/deploy/releases/0.7.0).