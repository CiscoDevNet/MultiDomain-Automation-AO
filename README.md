# Multi-Domain Automation

## The Challenge: 
- Traditionally, networks contains distinct domains
- A single domain alone can’t serve the business needs of your entire enterprise.


## The Solution: 
 - Cisco multidomain integrations help ensure that all individual networking domains work together to help you achieve your digital transformation objectives
- Leveraging Cisco's open APIs and Cisco Action Orchestrator we can consistently automate configuration and deployment of your Network Infrastructure and Data Center


## The Story:
 - Big retail chain standing up a new store
 - Connection to data center for price list is necessary
 - Quickly deploy local network at the branch
 - Local compute is required. Grant local compute correct access to data center services

https://raw.githubusercontent.com/CiscoDevNet/MultiDomain-Automation-AO/master/img/md.png

## The Tech: 
Using Cisco Action Orchastrator:
 - SD-WAN - Initiate site to site connection using SD-WAN APIs
 - DC Networking - Using ACI APIs configure branch DC network 
 - DC Compute - Configure and virtualize baremetal UCS server using UCS manager APIs
 - DNAC - Initiate site creation, Network device Discovery and provision devices
 
## Setup and Configuration
 - Sign-up for an instance of [Cisco Action Orchestrator](https://engage2demand.cisco.com/lp_cisco_cloudcenter_suite_saas_free_trial_17572)
 - Import CAO automation JSON 
 
 #### Import Flow
 
 ![alt text](https://raw.githubusercontent.com/CiscoDevNet/MultiDomain-Automation-AO/master/img/import1.png "Step 1")
 
 ![alt text](https://raw.githubusercontent.com/CiscoDevNet/MultiDomain-Automation-AO/master/img/import2.png "Step 2")
 
 
 - Configure workflow **Targets** [How To](https://docs.cloudmgmt.cisco.com/display/ACTIONORCHESTRATOR/Overview+Targets) for each technology. `Hint: you can point to the DevNet Always-on Sandbox`
 - Validate and run flows. 

 