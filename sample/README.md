# Sample

## クラスタ作成

1. コマンドライン

    ```bash
    eksctl create cluster \
        --name sample \
        --version 1.30 \
        --fargate \
        --nodegroup-name sample \
        --managed \
        --enable-ssm \
        --full-ecr-access \
        --alb-ingress-access
    # eksctl delete cluster --name sample
    ```

2. コンフィグファイル

    ```bash
    eksctl create cluster -f config-file/eks-sample-cluster.yaml
    # eksctl delete cluster -f config-file/eks-sample-cluster.yaml
    ```

## クラスタ削除

1. コマンドラインのパラメータ指定

    ```bash
    eksctl delete cluster --name sample
    ```

2. コンフィグファイルのパラメータ指定

    ```bash
    eksctl delete cluster -f eks-sample-cluster.yaml
    ```

## EKSクラスタへの認証情報追加

### ロールの権限追加

1. テストロールの作成

    ```bash
    testRoleArn=$(aws iam create-role --role-name test-role --assume-role-policy-document file://policy/test-role-trust-policy.json | jq -cr '.Role.Arn')
    # aws iam delete-role --role-name test-role
    ```

2. ロールのEKS権限の付与

    ```bash
    eksctl create iamidentitymapping \
        --cluster sample \
        --region=ap-northeast-1 \
        --arn ${testRoleArn} \
        --group eks-console-dashboard-full-access-group \
        --no-duplicate-arns
    ```

### ユーザーの権限追加

1. テストユーザの作成

    ```bash
    testUserArn=$(aws iam create-user --user-name test-user | jq -cr .User.Arn)
    # aws iam delete-user --user-name test-user
    ```

2. ユーザのEKS権限の付与

    ```bash
    eksctl create iamidentitymapping \
        --cluster sample \
        --region=ap-northeast-1 \
        --arn ${testUserArn} \
        --group eks-console-dashboard-restricted-access-group \
        --no-duplicate-arns
    ```

### 権限の確認

```bash
eksctl get iamidentitymapping --cluster sample --region=ap-northeast-1
```

## コンフィグ認証

> ~/.kube/config
