# kubernetesコマンドメモ

```
# 簡単入門
kubectl run myapp --image=nginx:1.12 --replicas 3 --labels="app=myapp"
kubectl create service loadbalancer --tcp 80:80 myapp
kubectl delete deployments --all
kubectl delete svc --all
```

```
※ エイリアス登録しとくと良い
alias ku='kubectl'

# --output wideで詳細に知れる
kubectl get pods --output wide

# ディレクトリも指定できる
kubectl apply -f ./

# 指定したディクトリ以下のファイルを再帰的に適用
kubectl apply -f ./ -R

# ディレクトリ内で削除されたマニフェストを検知し、リソースを削除する
kubectl apply -f ./ --prune
kubectl apply -f ./perfect_guide/prune_sample --prune -l system=a

# 即時削除
kubectl delete -f [ファイル名] --grace-period 0 --force

# コンテナ内のファイルをローカルマシンにコピー(逆も可)
kubectl cp sample-pod1:/etc/hostname hostname
kubectl cp hostname sample-pod1:tmp/newfile
kubectl exec -it sample-pod1 ls /tmp

# localhost:8888宛の通信をPodの80/TCPポートに転送（Service,Deploymentに紐づくPodへのポートフォワーディングも可）
kubectl port-forward sample-pod1 8888:80
kubectl port-forward deployment/sample-deployment 8888:80

# コマンド実行時の処理内容を詳細に見れる（６でRequest,Responseの概要、８でそのBodyまで見れる）
kubectl -v=6 get pod
kubectl -v=8 get pod
```

### Context, Namespace

```
brew install kubectx (kubensも同時にインストールされる)
```

```
$ kubectx --help
  kubectx                       : list the contexts
  kubectx <NAME>                : switch to context <NAME>
  kubectx -                     : switch to the previous context
  kubectx -c, --current         : show the current context name
  kubectx <NEW_NAME>=<NAME>     : rename context <NAME> to <NEW_NAME>
  kubectx <NEW_NAME>=.          : rename current-context to <NEW_NAME>
  kubectx -d <NAME> [<NAME...>] : delete context <NAME> ('.' for current-context)
                                  (this command won't delete the user/cluster entry
                                  that is used by the context)
  kubectx -u, --unset           : unset the current context

  kubectx -h,--help             : show this message
```

```
$ kubens --help
  kubens                    : list the namespaces in the current context
  kubens <NAME>             : change the active namespace of current context
  kubens -                  : switch to the previous namespace in this context
  kubens -c, --current      : show the current namespace
  kubens -h,--help          : show this message
```

### [stern](https://github.com/wercker/stern)

* kubectl logsより高機能で、複数のPodのログを同時に見ることができ、色分けされていて視覚的にも見やすい
* 引数は部分一致で指定できる

### [kube-ps1](https://github.com/jonmosco/kube-ps1)

* プロンプトにContext,Namespaceを表示(kubeon,kubeoff)