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

## サンプルコマンド

1. コマンドライン

    ```bash
    eksctl create cluster \
        --name sample \
        --version 1.19 \
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
