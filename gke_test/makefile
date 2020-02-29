.PHONY: create-cluster
create-cluster: #クラスタバージョンはすぐ新しくなるので注意
	gcloud container clusters create test --cluster-version=1.14.10-gke.17 --machine-type=n1-standard-1 --num-nodes=3

# 作成したクラスタの認証情報をkubectlに渡す
.PHONY: give-kubectl-credentials
give-kubectl-credentials:
	gcloud container clusters get-credentials test