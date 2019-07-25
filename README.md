# TRAINS Server for Kubernetes Clusters 

##  Auto-Magical Experiment Manager & Version Control for AI

[![GitHub license](https://img.shields.io/badge/license-SSPL-green.svg)](https://img.shields.io/badge/license-SSPL-green.svg)
[![GitHub version](https://img.shields.io/github/release-pre/allegroai/trains-server.svg)](https://img.shields.io/github/release-pre/allegroai/trains-server.svg)
[![PyPI status](https://img.shields.io/badge/status-beta-yellow.svg)](https://img.shields.io/badge/status-beta-yellow.svg)

## Introduction

The **trains-server** is the backend service infrastructure for [TRAINS](https://github.com/allegroai/trains).
It allows multiple users to collaborate and manage their experiments.
By default, **TRAINS** is set up to work with the **TRAINS** demo server, which is open to anyone and resets periodically. 
In order to host your own server, you will need to install **trains-server** and point **TRAINS** to it.

**trains-server** contains the following components:

* The **TRAINS** Web-App, a single-page UI for experiment management and browsing
* RESTful API for:
    * Documenting and logging experiment information, statistics and results
    * Querying experiments history, logs and results
* Locally-hosted file server for storing images and models making them easily accessible using the Web-App

Use this repository to deploy **trains-server** on Kubernetes clusters.

## Prerequisites

* a Kubernetes cluster
* `kubectl` is installed and configured (see [Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) in the Kubernetes documentation)
* a node labeled `app: trains`

## Deploying trains-server in Kubernetes Clusters

1. Create the `trains` namespace (all deployment is in this namespace):

        kubectl apply -f trains-namespace.yaml 

1. Clone the `trains-k8s` repository and change to the new `trains-k8s` directory:

        git clone https://github.com/allegroai/trains-k8s.git && cd trains-k8s

1. Deploy the **trains-server** services on your Kubernetes cluster:

        kubectl apply -f services.yaml

1. Create all the deployments:

        kubectl apply -f elasticsearch-deployment.yaml,mongo-deployment.yaml,apiserver-deployment.yaml,fileserver-deployment.yaml,webserver-deployment.yaml

## Port Mapping

After **trains-server** is deployed, the services expose the following node ports:

* API server on `30008`
* Web server on `30080`
* File server on `30081`

## Accessing trains-server

Access **trains-server** by creating a load balancer and domain name with records pointing to it.
**TRAINS** translates domain names using the following rules:

* record to access the **TRAINS** Web-App:

    `*app.<your domain name>.*` pointing to your node on port `30080`

    For example, `trainsapp.mydomainname.com` points to your node on port `30080`.

* record to access the **TRAINS** API:

    `*api.<your domain name>.*` pointing to your node on port `30008`

    For example, `trainsapi.mydomainname.com` points to your node on port `30008`.

* record to access the **TRAINS** file server:

    `*files.<your domain name>.*`  pointing to your node on port `30081`

    For example, `trainsfiles.mydomainname.com` points to your node on port `30081`.

## Additional Configuration for trains-server

You can also configure **trains-server** for:
 
* fixed users (users with credentials)
* non-responsive experiment watchdog settings
 
For detailed instructions, see the [Optional Configuration](https://github.com/allegroai/trains-server#optional-configuration) section in the **trains-server** repository README file.
