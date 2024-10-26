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

## クラスタ管理コマンド

1. コマンドで直接作成

    ```shell
    eksctl create cluster \
        --name sample \
        --version 1.30 \
        --region ap-northeast-1 \
        --dry-run
    # eksctl delete cluster --name sample
    ```

2. コンフィグから作成

    ```bash
    eksctl create cluster -f config.yaml
    # eksctl delete cluster -f config.yaml
    ```

## 参照

1. [eksctl - The official CLI for Amazon EKS](https://github.com/weaveworks/eksctl)
2. [Create a kubeconfig for Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html#create-kubeconfig-manually)
3. [Kubernetes リソースを表示する](https://docs.aws.amazon.com/ja_jp/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions)
4. [Image: eksctl/eksctl](https://gallery.ecr.aws/eksctl/eksctl)
