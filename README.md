# tanzu-packages-cd

A sample repository for deploying Tanzu packages automatically in TMC via CD feature and flux integration. This repo contains Kustomizations for Cert-manager and Contour along with all their dependencies such as Service Accounts and Namespaces.

## Pre-requisites
1. Tanzu Mission Control (TMC) access.
2. A TKGs environment (vSphere with Tanzu)
3. A TMC managed or provisioned Cluster.


## Setup Instructions
1. Select the Cluster you provisioned or manage from TMC, under Add-ons click Continuous Delivery --> Enable Continuous Delivery
2. Add the git Repository 
  - under Add-ons click Sources --> ADD GIT REPOSITORY
	  - Name: tanzu-packages-cd          #
	  - Repo:  https://github.com/tkrausjr/tanzu-packages-cd.git
	  - Credentials: No credentials Needed
	  - Advanced:
		  - Branch: Main

3. Add two Kustomizations (One for pre-requisites and one for both Tanzu packages(Conour & Cert-manager))
  - prerequisites  
    - Add-Ons --> Continuous Delivery --> Installed kustomizations --> Add Kustomization
      - [ ] Name:   prereq-tanzu-packages
      - [ ] Repo:  Select from Drop Down:  tanzu-packages-cd  
      - [ ] Path:  /pre-reqs
      - [ ] Advanced:  NA
        - [ ] SAVE
 - Contour & Cert-Manager Kustomization 
    -Add-Ons --> Continuous Delivery --> Installed kustomizations --> Add Kustomization
      - [ ] Name: cert-manager-contour
      - [ ] Repo:  Select from Drop Down:  tanzu-packages-cd  
      - [ ] Path:  /development
      - [ ] Advanced:  
        - [ ] Target Namespace:  packages
          - [ ] SAVE
  
4.
5.
6.
