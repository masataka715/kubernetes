# kubernetesコマンドメモ

```
# kubernetesのインストール
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
```

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
kubectl get replicasets -o wide

# ReplicaSetのPodの増減履歴が見れる（Events部分）
kubectl describe rs rs1

# --recordで履歴を保持できる
kubectl apply -f deployment.yaml --record
# annotationsに履歴やrevisionの番号情報などが保持されている
kubectl get replicasets -o yaml | head

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

# Deployment更新の一時停止・停止解除（即時適用したくない時）
kubectl rollout pause deployment dm1
kubectl rollout resume deployment dm1
kubectl rollout status deployment dm1

# 永続化領域の確認
kubectl get persistentvolumes
# 永続化領域の解放
kubectl delete persistentvolumeclaims www-ss1-{0..4}
```

### Context, Namespace

```
brew install kubectx (kubensも同時にインストールされる)
```

```
  kubectx                       : list the contexts
  kubectx <NAME>                : switch to context <NAME>
  kubectx -                     : switch to the previous context
  kubectx -c, --current         : show the current context name
```

```
  kubens                    : list the namespaces in the current context
  kubens <NAME>             : change the active namespace of current context
  kubens -                  : switch to the previous namespace in this context
  kubens -c, --current      : show the current namespace
```

### [stern](https://github.com/wercker/stern)

* kubectl logsより高機能で、複数のPodのログを同時に見ることができ、色分けされていて視覚的にも見やすい
* 引数は部分一致で指定できる

### [kube-ps1](https://github.com/jonmosco/kube-ps1)

* プロンプトにContext,Namespaceを表示(kubeon,kubeoff)

### GKE

```
# 設定確認
gcloud config list

# GKEで利用可能なKubernetesのversion確認
gcloud container get-server-config --zone asia-northeast1-a

# クラスタの作成
gcloud container clusters create test --cluster-version=1.14.10-gke.17 --machine-type=n1-standard-1 --num-nodes=3

# 作成したクラスタの認証情報をkubectlに渡す
gcloud container clusters get-credentials test
※コンテキストが２つできる（docker-for-desktop, gke~）

# クラスタ削除
gcloud container clusters delete test

#クラスタ確認
gcloud container clusters list
```