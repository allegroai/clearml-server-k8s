# ClearML Server for Kubernetes Clusters 

##  Auto-Magical Experiment Manager & Version Control for AI

[![GitHub license](https://img.shields.io/badge/license-SSPL-green.svg)](https://img.shields.io/badge/license-SSPL-green.svg)
[![GitHub version](https://img.shields.io/github/release-pre/allegroai/clearml-server.svg)](https://img.shields.io/github/release-pre/allegroai/clearml-server.svg)
[![PyPI status](https://img.shields.io/badge/status-beta-yellow.svg)](https://img.shields.io/badge/status-beta-yellow.svg)

## Introduction

The **clearml-server** is the backend service infrastructure for [ClearML](https://github.com/allegroai/clearml).
It allows multiple users to collaborate and manage their experiments.
By default, **ClearML** is set up to work with the **ClearML** demo server, which is open to anyone and resets periodically. 
In order to host your own server, you will need to install **clearml-server** and point **ClearML** to it.

**clearml-server** contains the following components:

* The **ClearML** Web-App, a single-page UI for experiment management and browsing
* RESTful API for:
    * Documenting and logging experiment information, statistics and results
    * Querying experiments history, logs and results
* Locally-hosted file server for storing images and models making them easily accessible using the Web-App

Use this repository to deploy **clearml-server** on Kubernetes clusters.

## Provided in this repository

### [Basic Helm Chart](https://github.com/allegroai/clearml-server-k8s/tree/master/clearml-server-chart)
### [Cloud-ready Helm Chart (advanced)](https://github.com/allegroai/clearml-server-k8s/tree/master/clearml-server-cloud-ready)

## Who We Are

ClearML is supported by the team behind *allegro.ai*,
where we build deep learning pipelines and infrastructure for enterprise companies.

We built ClearML to track and control the glorious but messy process of training production-grade deep learning models.
We are committed to vigorously supporting and expanding the capabilities of ClearML.

We promise to always be backwardly compatible, making sure all your logs, data and pipelines 
will always upgrade with you.

## License

Apache License, Version 2.0 (see the [LICENSE](https://www.apache.org/licenses/LICENSE-2.0.html) for more information)

## Documentation, Community & Support

More information in the [official documentation](https://allegro.ai/clearml/docs) and [on YouTube](https://www.youtube.com/c/ClearML).

For examples and use cases, check the [examples folder](https://github.com/allegroai/clearml/tree/master/examples) and [corresponding documentation](https://allegro.ai/clearml/docs/rst/examples/index.html).

If you have any questions: post on our [Slack Channel](https://join.slack.com/t/clearml/shared_invite/zt-c0t13pty-aVUZZW1TSSSg2vyIGVPBhg), or tag your questions on [stackoverflow](https://stackoverflow.com/questions/tagged/clearml) with '**[clearml](https://stackoverflow.com/questions/tagged/clearml)**' tag (*previously [trains](https://stackoverflow.com/questions/tagged/trains) tag*).

For feature requests or bug reports, please use [GitHub issues](https://github.com/allegroai/clearml/issues).

Additionally, you can always find us at *clearml@allegro.ai*

## Contributing

**PRs are always welcomed** :heart: See more details in the ClearML [Guidelines for Contributing](https://github.com/allegroai/clearml/blob/master/docs/contributing.md).


_May the force (and the goddess of learning rates) be with you!_
