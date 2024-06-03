## Deployment, Replicaset
- Replicaset
  - Podの親
  - 任意のPodを複製しそれらを束ねる
- Deployment
  - Replicasetの親
  - Replicasetを束ねる
```bash
Deployment
└── ReplicaSet # v1
    ├── Pod
    ├── Pod
    └── Pod
└── ReplicaSet # v2
    ├── Pod
    ├── Pod
    └── Pod
```
- Deploymentを利用するメリット
  - RollingUpdateによる無停止デプロイが可能になる。
  - 上図のように1個目のReplicasetにv1、2つ目のReplicasetにv2モジュールを立ち上げるようにできる
- maxUnavailable, maxSurge
  - どちらも％で指定する
  - maxUnavailable
    - デプロイ中に最大何%のPodが失われてもいいか
  - maxSurge
    - デプロイ中に最大何%のPodが増えてもいいか

## 実践
Replicasetの作成
```bash
$ kubectl apply -f chapter-06/replicaset.yaml 
replicaset.apps/httpserver created
$ kubectl get pod                
NAME               READY   STATUS    RESTARTS   AGE
httpserver-297sj   1/1     Running   0          40s
httpserver-dsxgj   1/1     Running   0          40s
httpserver-vsvcq   1/1     Running   0          40s
```

Replicasetの直接確認
```bash
$ kubectl get replicaset      
NAME         DESIRED   CURRENT   READY   AGE
httpserver   3         3         3       13s
```

Replicaset削除
```bash
$ kubectl delete replicaset httpserver
replicaset.apps "httpserver" deleted
$ kubectl get pod                     
No resources found in default namespace.
```

## 参考：Rolling UpdateとB/G Deployの違い
[改めてECSのデプロイ方法を整理する](https://tech.nri-net.com/entry/aws_ecs_deploy)