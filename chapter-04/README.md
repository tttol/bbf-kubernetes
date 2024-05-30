## 座学
- リソースの詳細はマニフェストファイル(yaml)で記述する
- コンテナを起動する最小構成はPod
- 1つのPodに複数のコンテナをいれることができる

## 実践
クラスター作成
```bash
$ kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.30.0) 🖼 
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-kind"
You can now use your cluster with:
```
クラスター確認
```bash
$ kubectl get nodes
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   91s   v1.30.0
```
Pod作成
```bash
$ kubectl apply --filename myapp.yaml --namespace default
pod/myapp created
```
Pod確認
```bash
$ kubectl get pod
NAME    READY   STATUS    RESTARTS   AGE
myapp   1/1     Running   0          13m
```