# ClearML Server for Kubernetes Clusters Using Helm

##  Auto-Magical Experiment Manager & Version Control for AI

[![GitHub license](https://img.shields.io/badge/license-SSPL-green.svg)](https://img.shields.io/badge/license-SSPL-green.svg)
[![GitHub version](https://img.shields.io/github/release-pre/allegroai/clearml-server.svg)](https://img.shields.io/github/release-pre/allegroai/clearml-server.svg)
[![PyPI status](https://img.shields.io/badge/status-beta-yellow.svg)](https://img.shields.io/badge/status-beta-yellow.svg)

## Introduction

The **clearml-server** is the backend service infrastructure for [ClearML](https://github.com/allegroai/clearml).
It allows multiple users to collaborate and manage their experiments.
By default, *ClearML is set up to work with the ClearML Demo Server, which is open to anyone and resets periodically. 
In order to host your own server, you will need to install **clearml-server** and point ClearML to it.

**clearml-server** contains the following components:

* The ClearML Web-App, a single-page UI for experiment management and browsing
* RESTful API for:
    * Documenting and logging experiment information, statistics and results
    * Querying experiments history, logs and results
* Locally-hosted file server for storing images and models making them easily accessible using the Web-App

Use this repository to add **clearml-server** to your Helm and then deploy **clearml-server** on Kubernetes clusters using Helm.
 
## Prerequisites

* a Kubernetes cluster
* `kubectl` is installed and configured (see [Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) in the Kubernetes documentation)
* `helm` installed (see [Installing Helm](https://helm.sh/docs/using_helm/#installing-helm) in the Helm documentation)
* one node labeled `app: clearml`

    **Important**: ClearML Server deployment uses node storage. If more than one node is labeled as `app: clearml` and you redeploy or update later, then ClearML Server may not locate all of your data. 
    
## Modify the default values required by Elastic in Docker configuration file (see [Notes for production use and defaults](https://www.elastic.co/guide/en/elasticsearch/reference/master/docker.html#_notes_for_production_use_and_defaults))
1 - Connect to node you labeled as `app=clearml`

Edit `/etc/docker/daemon.json` (if it exists) or create it (if it does not exist).

Add or modify the `defaults-ulimits` section as shown below. Be sure the `defaults-ulimits` section contains the `nofile` and `memlock` sub-sections and values shown.

**Note**: Your configuration file may contain other sections. If so, confirm that the sections are separated by commas (valid JSON format). For more information about Docker configuration files, see [Daemon configuration file](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file), in the Docker documentation.

The clearml-server required defaults values are (json):

    {
        "default-ulimits": {
            "nofile": {
                "name": "nofile",
                "hard": 65536,
                "soft": 1024
            },
            "memlock":
            {
                "name": "memlock",
                "soft": -1,
                "hard": -1
            }
        }
    }

2 - Set the Maximum Number of Memory Map Areas

Elastic requires that the vm.max_map_count kernel setting, which is the maximum number of memory map areas a process can use, is set to at least 262144.

For CentOS 7, Ubuntu 16.04, Mint 18.3, Ubuntu 18.04 and Mint 19.x, we tested the following commands to set vm.max_map_count:

    echo "vm.max_map_count=262144" > /tmp/99-clearml.conf
    sudo mv /tmp/99-clearml.conf /etc/sysctl.d/99-clearml.conf
    sudo sysctl -w vm.max_map_count=262144

For information about setting this parameter on other systems, see the [elastic documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker-cli-run-prod-mode).

3 - Restart docker:

    sudo service docker restart
    
## Deploying ClearML Server in Kubernetes Clusters Using Helm 
 
1. Add the **clearml-server** repository to your Helm:

        helm repo add allegroai https://allegroai.github.io/clearml-server-helm/

1. Confirm the **clearml-server** repository is now in Helm:

        helm search clearml

    The helm search results must include `allegroai/clearml-server-chart`.

1. Install `clearml-server-chart` on your cluster:

        helm install allegroai/clearml-server-chart --namespace=clearml --name clearml-server

    A  clearml `namespace` is created in your cluster and **clearml-server** is deployed in it.
   
        
## Updating ClearML Server application using Helm

1. If you are upgrading from Trains Server version 0.15 or older to ClearML Server, a data migration is required before you upgrade.
Stay tuned, we'll update the required steps for the upgrade soon!

1. If you are upgrading from Trains Server to ClearML Server, follow these steps first:

    1. Log in to the node labeled as `app=trains`
    1. Rename /opt/trains and its subdirectories to /opt/clearml the following way:
    
           sudo mv /opt/trains /opt/clearml

    1. Label the node as `app=clearml`
    1. Follow the [Deploying ClearML Server](##-Deploying-ClearML-Server-in-Kubernetes-Clusters-Using-Helm) instructions to deploy Clearml

1. Update using new or updated values.yaml
        
        helm upgrade clearml-server allegroai/clearml-server-chart -f new-values.yaml
        
1. If there are no breaking changes, you can update your deployment to match repository version:

        helm upgrade clearml-server allegroai/clearml-server-chart
   
   **Important**: 
        
    * If you previously deployed a **clearml-server**, you may encounter errors. If so, you must first delete old deployment using the following command:
    
            helm delete --purge clearml-server
            
        After running the `helm delete` command, you can run the `helm install` command.
        
## Port Mapping

After **clearml-server** is deployed, the services expose the following node ports:

* API server on `30008`
* Web server on `30080`
* File server on `30081`

## Accessing ClearML Server

Access **clearml-server** by creating a load balancer and domain name with records pointing to the load balancer.

Once you have a load balancer and domain name set up, follow these steps to configure access to clearml-server on your k8s cluster:

1. Create domain records

   * Create 3 records to be used for Web-App, File server and API access using the following rules: 
     * `app.<your domain name>` 
     * `files.<your domain name>`
     * `api.<your domain name>`
     
     (*for example, `app.clearml.mydomainname.com`, `files.clearml.mydomainname.com` and `api.clearml.mydomainname.com`*)
2. Point the records you created to the load balancer
3. Configure the load balancer to redirect traffic coming from the records you created:
     * `app.<your domain name>` should be redirected to k8s cluster nodes on port `30080`
     * `files.<your domain name>` should be redirected to k8s cluster nodes on port `30081`
     * `api.<your domain name>` should be redirected to k8s cluster nodes on port `30008`

## Additional Configuration for ClearML Server

You can also configure the **clearml-server** for:
 
* fixed users (users with credentials)
* non-responsive experiment watchdog settings
 
For detailed instructions, see the [Optional Configuration](https://github.com/allegroai/clearml-server#optional-configuration) section in the **clearml-server** repository README file.
