.PHONY: setup
setup:
	kubectl config use-context docker-desktop
	@sleep 3
	helm init
	@sleep 10
	kubectl -n kube-system get service,deployment,pod --selector app=helm
	helm version

.PHONY: i-redmine
i-redmine:
	helm install -f value/redmine.yaml --name redmine stable/redmine --version 4.0.0
	@sleep 30
	helm ls
	kubectl get service,deployment --selector release=redmine

.PHONY: d-redmine
d-redmine:
	helm del --purge redmine

.PHONY: p-chart
p-chart:
	helm package echo
	helm search echo

.PHONY: helm-serve
helm-serve:
	helm serve &

.PHONY: i-chart
i-chart:
	helm install -f value/echo.yaml --name echo local/echo
	@sleep 30
	kubectl get deployment,service,ingress --selector app=echo
	curl http://localhost -H 'Host: ch07-echo.gihyo.local'

.PHONY: d-chart
d-chart:
	helm del --purge echo
