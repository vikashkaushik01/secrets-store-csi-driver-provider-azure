#Install the Secrets Store CSI Driver
## secrets-store-csi-driver-provider-azure
kubectl apply -f csi_driver_yamls/rbac-secretproviderclass.yaml
kubectl apply -f csi_driver_yamls/csidriver.yaml
kubectl apply -f csi_driver_yamls/secrets-store.csi.x-k8s.io_secretproviderclasses.yaml
kubectl apply -f csi_driver_yamls/secrets-store.csi.x-k8s.io_secretproviderclasspodstatuses.yaml
kubectl apply -f csi_driver_yamls/secrets-store-csi-driver.yaml

## If using the driver to sync secrets-store content as Kubernetes Secrets, csi_driver_yamls the additional RBAC permissions
## required to enable this feature
kubectl apply -f csi_driver_yamls/rbac-secretprovidersyncing.yaml

## If using the secret rotation feature, csi_driver_yamls the additional RBAC permissions
## required to enable this feature
kubectl apply -f csi_driver_yamls/rbac-secretproviderrotation.yaml

## If using the CSI Driver token requests feature (https://kubernetes-csi.github.io/docs/token-requests.html) to use
## pod/workload identity to request a token and use with providers
kubectl apply -f csi_driver_yamls/rbac-secretprovidertokenrequest.yaml

## [OPTIONAL] To csi_driver_yamls driver on windows nodes
kubectl apply -f csi_driver_yamls/secrets-store-csi-driver-windows.yaml

To validate the installer is running as expected, run the following commands:


kubectl get po --namespace=kube-system
You should see the Secrets Store CSI driver pods running on each agent node:


csi-secrets-store-qp9r8         3/3     Running   0          4m
csi-secrets-store-zrjt2         3/3     Running   0          4m
You should see the following CRDs deployed:


kubectl get crd
NAME
secretproviderclasses.secrets-store.csi.x-k8s.io
secretproviderclasspodstatuses.secrets-store.csi.x-k8s.io

-----------------------------------------------------------------------------------------


# Install the Azure Key Vault provider

## For linux nodes

kubectl apply -f https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/deployment/provider-azure-installer.yaml

## For windows nodes

kubectl apply -f https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/deployment/provider-azure-installer-windows.yaml


To validate the provider's installer is running as expected, run the following commands:

kubectl get pods -l app=csi-secrets-store-provider-azure
You should see the provider pods running on each agent node:

NAME                                     READY   STATUS    RESTARTS   AGE
csi-secrets-store-provider-azure-4ngf4   1/1     Running   0          8s
csi-secrets-store-provider-azure-bxr5k   1/1     Running   0          8s
