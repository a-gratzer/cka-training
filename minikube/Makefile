.POSIX:
.PHONY: *

.EXPORT_ALL_VARIABLES:

# ###############################################
PROFILE:=minikube
KUBERNETES_VERSION:=v1.27.3
NODES:=1
DISK_SIZE:=10g
CPUS:=3
MEMORY:=8g
CNI:=calico
# ###############################################

# #############################
# mini
mini_start:
	minikube start --kubernetes-version=$(KUBERNETES_VERSION) \
		--profile $(PROFILE) \
		--nodes=$(NODES) \
		--disk-size=$(DISK_SIZE) \
		--cpus=$(CPUS) \
		--memory=$(MEMORY) \
		--cni=$(CNI)

mini_status:
	minikube status

mini_config:
	minikube config view

mini_dashboard:
	minikube dashboard

mini_stop:
	minikube stop

mini_delete_profile:
	minikube delete $(PROFILE)

mini_delete_all:
	minikube delete --all

mini_profile_list:
	minikube profile list

mini_ssh:
	minikube ssh -n minikube

mini_metrics:
	minikube addons enable metric-server

# #############################
# kubectl

k_version:
	kubectl version

k_cluster_info:
	kubectl cluster-info

k_api_resources:
	kubectl api-resources