# tanzu-packages-cd

A sample repository for deploying Tanzu packages automatically in TMC via CD feature and flux integration. This repo contains Kustomizations for Cert-manager and Contour along with all their dependencies such as Service Accounts and Namespaces.

## Pre-requisites
1. Tanzu Mission Control (TMC) access.
2. A TKGs environment (vSphere with Tanzu)
3. A TMC managed or provisioned Cluster.


## Setup Instructions
1. Select the Cluster you provisioned or manage from TMC, under Add-ons click Continuous Delivery --> Enable Continuous Delivery
2. Add the git Repository 
	  - under Add-ons click Sources --> Git repositories --> ADD GIT REPOSITORY
		  - Name: tanzu-packages-cd         
		  - Repo:  https://github.com/tkrausjr/tanzu-packages-cd.git
		  - Credentials: No credentials Needed
		  - Advanced:
			  - Branch: dependencies

3. Add two Kustomizations (One for pre-requisites and one for both Tanzu packages(Conour & Cert-manager))
	  - prerequisites  
		    - Add-Ons --> Continuous Delivery --> Installed kustomizations --> Add Kustomization
		      - Name:  package-prereqs
		      - Repo:  Select from Drop Down:  tanzu-packages-cd  
		      - Path:  /pre-reqs
		      - Advanced:  NA
		      	- Target Namespace: <Blank>
			- Prune: Enabled
			- SAVE
	  - Contour & Cert-Manager Kustomization 
		    -Add-Ons --> Continuous Delivery --> Installed kustomizations --> Add Kustomization
		      - Name:  development
		      - Repo:  Select from Drop Down:  tanzu-packages-cd  
		      - Path:  /development
		      - Advanced:  
			- Target Namespace:  packages
			- Prune: Enabled
			  - SAVE
  
  ## Validation
  ``` bash

k get kustomizations -A
	NAMESPACE                            NAME              AGE     READY   STATUS
	tanzu-continuousdelivery-resources   development       5m41s   True    Applied revision:dependencies/69619bbf4b51676376ce8420c2ca70b1bd641e44
	tanzu-continuousdelivery-resources   package-prereqs   6m30s   True    Applied revision: dependencies/69619bbf4b51676376ce8420c2ca70b1bd641e44

tanzu package installed list -n packages

  NAME          PACKAGE-NAME                   PACKAGE-VERSION        STATUS
  cert-manager  cert-manager.tanzu.vmware.com  1.10.1+vmware.1-tkg.2  Reconcile succeeded
  contour       contour.tanzu.vmware.com       1.22.3+vmware.1-tkg.1  Reconcile succeeded

k get all -n cert-manager
	NAME                                           READY   STATUS    RESTARTS   AGE
	pod/cert-manager-69f486fbd5-t9kbd              1/1     Running   0          5m3s
	pod/cert-manager-cainjector-66769494cd-zfmdx   1/1     Running   0          5m3s
	pod/cert-manager-webhook-5679fc4f7f-qzl9c      1/1     Running   0          5m3s

	NAME                           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
	service/cert-manager           ClusterIP   10.96.1.86     <none>        9402/TCP   5m3s
	service/cert-manager-webhook   ClusterIP   10.96.88.102   <none>        443/TCP    5m3s

	NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
	deployment.apps/cert-manager              1/1     1            1           5m3s
	deployment.apps/cert-manager-cainjector   1/1     1            1           5m3s
	deployment.apps/cert-manager-webhook      1/1     1            1           5m3s

	NAME                                                 DESIRED   CURRENT   READY   AGE
	replicaset.apps/cert-manager-69f486fbd5              1         1         1       5m3s
	replicaset.apps/cert-manager-cainjector-66769494cd   1         1         1       5m3s
	replicaset.apps/cert-manager-webhook-5679fc4f7f      1         1         1       5m3s
  
k get all -n tanzu-system-ingress
	NAME                           READY   STATUS    RESTARTS   AGE
	pod/contour-7b96f4d5df-d587r   1/1     Running   0          2m51s
	pod/contour-7b96f4d5df-rt5ll   1/1     Running   0          2m52s
	pod/envoy-7rczh                2/2     Running   0          2m52s
	pod/envoy-gdlpn                2/2     Running   0          2m52s

	NAME              TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE
	service/contour   ClusterIP      10.96.74.122   <none>           8001/TCP                     2m52s
	service/envoy     LoadBalancer   10.96.78.234   192.168.101.24   80:30008/TCP,443:30098/TCP   2m52s

	NAME                   DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
	daemonset.apps/envoy   2         2         2       2            2           <none>          2m52s

	NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
	deployment.apps/contour   2/2     2            2           2m52s

	NAME                                 DESIRED   CURRENT   READY   AGE
	replicaset.apps/contour-7b96f4d5df   2         2         2       2m52s
  
  
  
  ```
  
  

