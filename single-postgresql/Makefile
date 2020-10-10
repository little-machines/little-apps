# https://severalnines.com/database-blog/using-kubernetes-deploy-postgresql

cluster_node = kx00
postgres_image = postgres:10-alpine
pg_user = postgres
pg_password = p0stgres
pg_database = db

all: create

postgres-deployment.yaml: postgres-deployment.yaml.tmpl
	sed -e 's/{image}/$(postgres_image)/g' $< \
		> $@

postgres-configmap.yaml: postgres-configmap.yaml.tmpl
	sed -e 's/{pg_database}/$(pg_database)/g' $< | \
		sed -e 's/{pg_user}/$(pg_user)/g' | \
		sed -e 's/{pg_password}/$(pg_password)/g' \
		> $@

create: delete postgres-configmap.yaml postgres-deployment.yaml
	kubectl create -f postgres-configmap.yaml
	kubectl create -f postgres-storage.yaml
	kubectl create -f postgres-deployment.yaml
	kubectl create -f postgres-service.yaml

delete:
	-kubectl delete -f postgres-service.yaml
	-kubectl delete -f postgres-deployment.yaml
	-kubectl delete -f postgres-storage.yaml
	-kubectl delete -f postgres-configmap.yaml
	rm -f postgres-deployment.yaml
	rm -f postgres-configmap.yaml

storage-status:
	kubectl get pv postgres-pv-volume -o wide
	kubectl get pvc postgres-pv-claim -o wide

service-status:
	kubectl get service postgres -o wide

deployment-status:
	kubectl get deployment postgres -o wide

status: storage-status deployment-status service-status

client:
	kubectl wait --for=condition=ready pod -l app=postgres
	$(eval nodeport = $(shell kubectl get service postgres \
		-o jsonpath='{.spec.ports[0].nodePort}') \
	)
	docker run --rm \
		-e PGHOST=$(cluster_node) \
		-e PGPORT=$(nodeport) \
		-e PGUSER=$(pg_user) \
		-e PGPASSWORD=$(pg_password) \
		-e PGDATABASE=$(pg_database) \
		-it $(postgres_image) psql
