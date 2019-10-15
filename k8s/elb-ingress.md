## Worker Node用のIAMロール取得

 kubectl -n kube-system describe configmap aws-auth
 
node-role-permission-policy.json

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "*"
    }
  ]
}
````

### ポリシーを作成

```
aws iam create-policy \
  --policy-name sts-assumerole-policy \
  --policy-document file://node-role-permission-policy.json

```

上記結果のARNをメモ


### IAMロール名を取得

```
  aws iam list-roles --query "Roles[?contains(RoleName,'NodeInstanceRole')].[RoleName][]" --output text
```

### 設定

```
    aws iam attach-role-policy --role-name <IAMロール名> --policy-arn <ARN名>
```




 