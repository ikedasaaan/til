
kubectl apply -f install/kubernetes/helm/helm-service-account.yaml

helm init --service-account tiller

kubectl create namespace istio-system

helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system

helm install install/kubernetes/helm/istio --name istio --namespace istio-system


# default inject設定

kubectl label namespace kntn-api istio-injection=enabled