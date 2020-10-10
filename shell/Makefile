deployent_name = vtse-shell
shell = /bin/bash

create:
	kubectl create -f shell.yaml

shell:
	$(eval pod = $(shell kubectl get pods \
		-l app=$(deployent_name) \
		-ojsonpath='{.items[0].metadata.name}' \
	))
	kubectl exec -it $(pod) -- /usr/bin/sudo -u vtse -i
	#kubectl exec -it $(pod) -- $(shell)

delete:
	-kubectl delete -f shell.yaml

status:
	kubectl get pods \
		-l app=$(deployent_name) \
		-owide
