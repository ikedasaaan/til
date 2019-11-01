# namespace

```
## 一覧取得
kubectl get namespace


## 現在のnamespace
kubectl config view | grep namespace


## default変更
kubectl config set-context $(kubectl config current-context) --namespace=kube-system


## 各種コマンドでの指定

kubectl --namespace=kube-system get pod
```
