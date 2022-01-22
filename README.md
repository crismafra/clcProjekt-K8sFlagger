# CLC - Canary Deployment with Flagger

## 0. Introduction
This repository contains the demo and documentation for the student project which is done in the context of the *Cloud Computing (CLC)* course. 

## 1. Team members
- **Hanreich Martin**, S2010595006
- **Mafra de Sa Cristina**, S2010595013
- **Prandner Sarah**, S2010595018

## 2. Description
### 2.1 Goal
The goal is to create an easy-to-follow tutorial for the canary deployment strategy assisted by the Kubernetes operator *Flagger*. 
Focus is on the encountered obstacles and remaining questions when attempting working with flagger while following the official *Flagger docs*. 
In order to create this guide a sample application is deployed using *Flagger*. Furthermore, three more aspects when using flagger are shown: a demo of an automated rollback, monitoring of the rollout and alerting in case of events. 
In combination with *Flagger* *NGINX* is used for traffic routing while the analysis and monitoring is done via *Prometheus* and *Microsoft Teams* is used for receiving notifications and alerts regarding the deployment status.

### 2.2 Used Tools
#### Flagger
Flagger is a progressive delivery tool for Kubernetes. It helps with the deployment of an application by providing services which facilitate the rollout process and its automation. Specifically, Flagger supports the *Canary Release*, *Blue/Green* and *Blue/Green Mirroring* deployment strategies as well as *A/B-Testing*. Moreover, *Flagger* works in conjunction with other tools to enable for example effective monitoring and alerting. 
These include Service Meshes like *Istio* or *Linkerd*, Ingress Controller like *Skipper* and monitoring via *DataDog*.

However, for this project following tools are used:
### NGINX
More specifically the NGINX Ingress Controller is used. It is responsible for traffic routing and therefore handling external requests from the users.

See: [NGINX Ingress Controller](https://docs.nginx.com/nginx-ingress-controller/)]

### Prometheus
Prometheus is an open-source systems monitoring and alerting toolkit. Prometheus collects and stores its metrics as time series data, i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels. Prometheus scrapes metrics from instrumented jobs, either directly or via an intermediary push gateway for short-lived jobs. It stores all scraped samples locally and runs rules over this data to either aggregate and record new time series from existing data or generate alerts.

More details on the website: [Prometheus](https://prometheus.io/docs/introduction/overview/)

### Helm
Helm is a tool used to manage Kubernetes applications that simplify their deployment through a collection of files, known as *Charts*. The *Charts* describe a related set of Kubernetes resources and assist in defining, installing and updating applications regardless of their complexity.

More details on the website: [Helm](https://helm.sh/)

### Microsoft Teams

With the help of Prometheus alerting through AlertManager we will be able to send alert messages via Teams.
For that we have to configure Incoming Webhooks in Teams. This will then generate a webhook URL which will be used to post messages to our channel.  To send the third-party tool for Microsoft Teams, a alertmanager.yaml file will be needed.
So that our Alertmanager is ready for sending alerts using webhook, a Secret object - which is used by Prometheus operator’s Alertmanager and then applied to Kubernetes - must be created. Last but not least a default Microsoft Teams Message card template has to be defined and a Kubernetes manifest with Helm passing “Incoming Webhook URL” of Microsoft Teams must be generated. This will be finally deployed to Kubernetes and makes it possible to receive alerts in our Microsoft Teams Channel. 

More details on the website: [msteams](https://github.com/prometheus-msteams/prometheus-msteams)

A deep look into Flagger and the tools: [Flagger](https://flagger.app/)

## 3. Architecture
Following diagram shows how the previous described components work together for the canary rollout:
[![N|Solid](https://raw.githubusercontent.com/fluxcd/flagger/main/docs/diagrams/flagger-nginx-overview.png)](https://raw.githubusercontent.com/fluxcd/flagger/main/docs/diagrams/flagger-nginx-overview.png)

# 4. Milestones and Responsibilities
- *1 - Get required tools running: Flagger, NGINX, Helm*  
    **Responsible:** Hanreich Martin, **Working:** All  
    **Date:** 5.1.2022  
- *2 - Perform the canary deployment*  
    **Responsible**: Cristina Mafra, **Working:** All  
    **Date:** 12.1.2022  
- *3 - Demo automatic rollback*  
    **Responsible**: Hanreich Martin, **Working:** Hanreich Martin  
    **Date:** 19.1.2022  
- *4 - Demo monitoring with Prometheus*  
    **Responsible**: Cristina Mafra, **Working:** Cristina Mafra  
    **Date:** 19.1.2022  
- *5 - Demo alerting via Microsoft Teams*  
    **Responsible**: Sarah Prandner, **Working:** Sarah Prandner  
    **Date:** 19.1.2022  
- *6 - Finalize tutorial*  
    **Responsible**: Sarah Prandner, **Working:** All  
    **Date:** 26.1.2022  
