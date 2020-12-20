name ?= hi

create:
	helm install $(name) .

update:
	helm upgrade $(name) .

delete:
	helm delete $(name)
	kubectl delete pvc data-$(name)-postgres-0

status:
	helm status $(name)

client:
	kubectl wait --timeout=300s --for=condition=available deployment -l app.kubernetes.io/name=todos-api,app.kubernetes.io/instance=$(name)
	$(eval port = $(shell kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services $(name)-todos-api))
	$(eval host = $(shell kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}"))
	http http://$(host):$(port)/health_check
	http http://$(host):$(port)/todos
