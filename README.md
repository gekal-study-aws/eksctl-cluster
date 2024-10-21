# EKS操作

## コマンド一覧

| コマンド            | 説明                                                     |
| ------------------- | -------------------------------------------------------- |
| eksctl associate    | Associate resources with a cluster                       |
| eksctl completion   | Generates shell completion scripts for bash, zsh or fish |
| eksctl create       | Create resource(s)                                       |
| eksctl delete       | Delete resource(s)                                       |
| eksctl disassociate | Disassociate resources from a cluster                    |
| eksctl drain        | Drain resource(s)                                        |
| eksctl enable       | Enable features in a cluster                             |
| eksctl generate     | Generate gitops manifests                                |
| eksctl get          | Get resource(s)                                          |
| eksctl help         | Help about any command                                   |
| eksctl scale        | Scale resources(s)                                       |
| eksctl set          | Set values                                               |
| eksctl unset        | Unset values                                             |
| eksctl update       | Update resource(s)                                       |
| eksctl upgrade      | Upgrade resource(s)                                      |
| eksctl utils        | Various utils                                            |
| eksctl version      | Output the version of eksctl                             |

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
    eksctl create cluster -f eks-sample-cluster.yaml
    # eksctl delete cluster -f eks-sample-cluster.yaml
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
    testRoleArn=$(aws iam create-role --role-name test-role --assume-role-policy-document file://test-role-trust-policy.json | jq -cr '.Role.Arn')
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

1. aws-iam-authenticator token

    > SSOに対応してない(2021/03/08)

    ```yaml
    args:
    - token
    - -i
    - sample
    command: aws-iam-authenticator
    ```

2. aws eks get-token

    ```yaml
    args:
    - "eks"
    - "get-token"
    - "--cluster-name"
    - sample
    command: aws
    ```

## 参照

1. [eksctl - The official CLI for Amazon EKS](https://github.com/weaveworks/eksctl)
2. [Create a kubeconfig for Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html#create-kubeconfig-manually)
3. [Kubernetes リソースを表示する](https://docs.aws.amazon.com/ja_jp/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions)
