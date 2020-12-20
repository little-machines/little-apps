manifest_dir = ./base
name = event-processing
env = dev

create:
	kustomize build overlays/$(env) | kubectl apply -f -

delete:
	kustomize build overlays/$(env) | kubectl delete -f -

status:
	kubectl get all \
		-l app.kubernetes.io/name=$(name),app.kubernetes.io/environment=$(env) \
		-o wide

convert:
	kompose convert --out=$(manifest_dir)
