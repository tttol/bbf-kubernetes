## Podの詳細を確認する
- `kubectl  get pod`でPodの状態を一覧表示（`docker ps`に近い）
- `kubectl get pod -o yaml`で稼働中のPodの詳細をyaml形式で表示可能
- 出力形式については以下
```
    -o, --output='':
	Output format. One of: (json, yaml, name, go-template, go-template-file, template, templatefile, jsonpath,
	jsonpath-as-json, jsonpath-file, custom-columns, custom-columns-file, wide). See custom columns
	[https://kubernetes.io/docs/reference/kubectl/#custom-columns], golang template
	[http://golang.org/pkg/text/template/#pkg-overview] and jsonpath template
	[https://kubernetes.io/docs/reference/kubectl/jsonpath/].
```
- Podの詳細確認コマンドとして`kebectl logs [Pod名]` `kubectl describe pod [Pod名]`などもある

## コンテナをデバッグする
- アプリコンテナはアプリに関する最小限のリソースしか搭載されてない事が多い
- デバッグ用にcurlイメージをpullしてアプリをターゲットに起動して、そこからアプリのデバッグを行う
- アプリコンテナと同じネットワーク・同じボリュームにローカルからアクセス可能
```bash
kubectl debug \
--stdin --tty myapp \
--image=curlimages/curl:8.4.0 \
--target=hello-server \
-n default \
-- sh

Targeting container "hello-server". If you don't see processes from this container it may be because the container runtime doesn't support this feature.
Defaulting debug container name to debugger-p9qc2.
If you don't see a command prompt, try pressing enter.
~ $ curl localhost:8080
Hello, world!~ $ 

```

- もしくは、別Podでcurlを立ち上げてそこからcurする。る
- この場合、`curl localhost:8080`ではなく`curl {myappのIP}:8080`になる
- なのでIPを`kubectl get pod myapp -o wide`で事前に確認しておく必要がある
```bash
$ kubectl get pod myapp -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE                 NOMINATED NODE   READINESS GATES
myapp   1/1     Running   0          3d    10.244.0.5   kind-control-plane   <none>           <none>
$ kubectl exec -it curlpod -- /bin/sh
~ $ curl 10.244.0.5:8080
Hello, world!~ $
```

- ポートフォワードして別portでmyappを起動することも可能
```bash
kubectl port-forward myapp 8000:8080
Forwarding from 127.0.0.1:8000 -> 8080
Forwarding from [::1]:8000 -> 8080
```

```bash
# 別ターミナルから実行
curl localhost:8080
```

