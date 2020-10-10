master = kx00

taint-controlplane:
	#kubectl taint node kx00 CriticalAddonsOnly=true:NoExecute
	kubectl taint nodes $(master) node-role.kubernetes.io/master=true:NoSchedule --overwrite
	#kubectl label node $(master) kubernetes.io/role=master node-role.kubernetes.io/master= --overwrite


create: taint-controlplane


delete:
