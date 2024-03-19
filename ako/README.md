# ako helm release install via gitops


## Pre-requisites
1. Tanzu Mission Control (TMC) access.
2. A TKGs environment (vSphere with Tanzu)
3. A TMC managed or provisioned Cluster with Continuous Delivery Enabled.
4.
5.
6.



## Setup Instructions
1. Select the Cluster you provisioned or manage from TMC, under Add-ons click Continuous Delivery --> Enable Continuous Delivery
2. Add the git Repository 
	  - under Add-ons click Sources --> Git repositories --> ADD GIT REPOSITORY
		  - Name: tanzu-packages-cd         
		  - Repo:  https://github.com/tkrausjr/tanzu-packages-cd.git
		  - Credentials: No credentials Needed
		  - Advanced:
			  - Branch: dependencies