.PHONY: container-exec
container-exec:
	kubectl exec -it simple-echo sh -c nginx

.PHONY: container-logs
container-logs:
	kubectl logs -f simple-echo -c echo

.PHONY: record
deployment-info:
	kubectl apply -f simple-deployment.yaml --record

.PHONY: show-revision
show-revision:
	kubectl rollout history deployment echo

.PHONY: roll-back
roll-back:
	kubectl rollout undo deployment echo

.PHONY: deployment-info
deployment-info:
	kubectl get pod,replicaset,deployment --selector app=echo

