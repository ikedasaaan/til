


### cluster一覧

```
$ kubectl get-clusters
```

# Context

### context一覧

```
kubectl config get-contexts
```


### context一覧

```
kubectl config use-context <context name>
```


### namespace を　切り替え

```
$ kubectl config set-context $(kubectl config current-context) --namespace=mynamespace
```

### Namespace を　確認する

```
$ kubectl config view | grep namespace:
```


### Deployment状態確認

```
kubectl describe deployment
```