manifest_dir = ./base
name = buffalo-todos-api
env = dev

create:
	kustomize build overlays/$(env) | kubectl apply -f -

delete:
	kustomize build overlays/$(env) | kubectl delete -f -

status:
	kubectl get all \
		-l app.kubernetes.io/name=$(name),app.kubernetes.io/environment=$(env) \
		-o wide

client:
	$(eval labels = app.kubernetes.io/environment=$(env),app.kubernetes.io/name=buffalo-todos-api,io.kompose.service=web)
	kubectl wait --timeout=300s --for=condition=available deployment -l $(labels)
	kubectl wait --timeout=300s --for=condition=Initialized pod -l $(labels)
	$(eval ip = $(shell kubectl get service -l $(labels) \
		-o jsonpath='{.items[0].status.loadBalancer.ingress[0].ip}'))
	$(eval port = $(shell kubectl get service -l $(labels) \
		-o jsonpath='{.items[0].spec.ports[0].port}'))
	http http://$(ip):$(port)/health_check
	http http://$(ip):$(port)/todos

convert:
	kompose convert --file=docker-compose.yml --out=$(manifest_dir)
	kompose convert --file=docker-compose.db.yml --out=$(manifest_dir)
