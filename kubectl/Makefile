.PHONY: a-deployment
a-deployment:
	kubectl apply -f manifest/simple-deployment.yaml --record
	@sleep 30
	kubectl get pod,replicaset,deployment --selector app=echo
	kubectl rollout history deployment echo

.PHONY: d-deployment
d-deployment:
	kubectl delete -f manifest/simple-deployment.yaml

.PHONY: a-ingress
a-ingress:
	kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.16.2/deploy/mandatory.yaml
	kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.16.2/deploy/provider/cloud-generic.yaml
	kubectl apply -f manifest/simple-service.yaml
	kubectl apply -f manifest/simple-ingress.yaml
	@sleep 60
	kubectl get ingress
	curl http://localhost -H 'Host: ch05.gihyo.local'

.PHONY: d-ingress
d-ingress:
	kubectl delete -f manifest/simple-ingress.yaml
	kubectl delete -f manifest/simple-service.yaml
	kubectl delete -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.16.2/deploy/provider/cloud-generic.yaml
	kubectl delete -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.16.2/deploy/mandatory.yaml
	kubectl get ingress

.PHONY: a-job
a-job:
	kubectl apply -f manifest/simple-job.yaml
	@sleep 30
	kubectl logs -l app=pingpong
	kubectl get pod -l app=pingpong

.PHONY: d-job
d-job:
	kubectl delete -f manifest/simple-job.yaml

.PHONY: a-cronjob
a-cronjob:
	kubectl apply -f manifest/simple-cronjob.yaml
	@sleep 30
	kubectl logs -l app=pingpong
	kubectl get pod -l app=pingpong

.PHONY: d-cronjob
d-cronjob:
	kubectl delete -f manifest/simple-cronjob.yaml

.PHONY: a-r-u
a-r-u:
	kubectl apply -f deploy/echo-version.yaml
	@sleep 10
	kubectl apply -f deploy/update-checker.yaml
	@sleep 10
	kubectl get pod,service,deployment

.PHONY: p-r-u
p-r-u:
	kubectl patch deployment echo-version -p '{"spec":{"template":{"spec":{"containers":[{"name":"echo-version","image":"gihyodocker/echo-version:0.2.0"}]}}}}'
	@sleep 10
	kubectl logs -f update-checker

.PHONY: d-r-u
d-r-u:
	kubectl delete -f deploy/update-checker.yaml
	kubectl delete -f deploy/echo-version.yaml
	@sleep 10
	kubectl get pod,service,deployment

.PHONY: a-bg-d
a-bg-d:
	kubectl apply -f deploy/echo-version-blue.yaml
	@sleep 5
	kubectl apply -f deploy/echo-version-green.yaml
	@sleep 5
	kubectl apply -f deploy/echo-version-bluegreen-service.yaml
	@sleep 5
	kubectl apply -f deploy/update-checker.yaml
	@sleep 5
	kubectl get pod,service,deployment

.PHONY: p-bg-d
p-bg-d:
	kubectl patch service echo-version -p '{"spec":{"selector":{"color":"green"}}}'
	@sleep 10
	kubectl logs -f update-checker

.PHONY: d-bg-d
d-bg-d:
	kubectl delete -f deploy/update-checker.yaml
	kubectl delete -f deploy/echo-version-bluegreen-service.yaml
	kubectl delete -f deploy/echo-version-green.yaml
	kubectl delete -f deploy/echo-version-blue.yaml
	@sleep 10
	kubectl get pod,service,deployment

.PHONY: a-log
a-log:
	kubectl apply -f logging/elasticsearch.yaml
	@sleep 30
	kubectl apply -f logging/kibana.yaml
	@sleep 30
	kubectl apply -f logging/fluentd-daemonset.yaml
	@sleep 10
	kubectl apply -f logging/echo.yaml
	@sleep 10
	kubectl get pod,service,deployment
	curl http://localhost:30080

.PHONY: d-log
d-log:
	kubectl delete -f logging/echo.yaml
	kubectl delete -f logging/fluentd-daemonset.yaml
	kubectl delete -f logging/kibana.yaml
	kubectl delete -f logging/elasticsearch.yaml
	@sleep 10
	kubectl get pod,service,deployment

.PHONY: a-affinity
a-affinity:
	kubectl apply -f manifest/pod-affinity-deployment.yaml
	@sleep 10
	kubectl get pod

.PHONY: d-affinity
d-affinity:
	kubectl delete -f manifest/pod-affinity-deployment.yaml
	@sleep 10
	kubectl get pod
