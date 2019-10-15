
### 前提

* kubectlが利用できる
* kubectlがインストール先のClsterに向いている


### 構築

cluster-admin role の場合

```
cat <<EoF > rbac.yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
EoF
```

```
helm init --service-account tiller --tiller-namespace kube-system
```

```
helm list --tiller-namespace kube-system
```

