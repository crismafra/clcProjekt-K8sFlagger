# CLC - Canary Deployment with Flagger

## 0. Introduction
This repository contains the demo and documentation for the project in the context of the Cloud Computing course. 
## 1. Team members
- **Hanreich Martin**, S2010595006
- **Mafra de Sa Cristina**, S2010595013
- **Prandner Sarah**, S2010595018

## 2. Description
### 2.1 Goal
The goal is to create an easy-to-follow tutorial for the canary deployment strategy assisted by the Kubernetes operator *Flagger*. 
Focus is on the encountered obstacles and remaining questions when attempting working with flagger while following the official *Flagger docs*. 
In order to create this guide a sample application is deployed using *Flagger*. Furthermore, three more features of flagger are shown: a demo of an automated rollback, monitoring of the rollout and alerting in case of events. 
In combination with *Flagger* *NGINX* is used for traffic routing while the analysis and minitoring is done via *Prometheus* and *Microsoft Teams* is used for receiving notifications and alerts regarding the deployment status.

### 2.2 Used Tools
#### Flagger
Flagger is a progressive delivery tool for Kubernetes. It helps with the deployment of an application by providing services which facilitate the rollout process and its automation. Specifically, Flagger supports the *Canary Release*, *Blue/Green* and *Blue/Green Mirroring* deployment strategies as well as *A/B-Testing*. Moreover, *Flagger* works in conjunction with other tools to enable for example effective monitoring and alerting. 
These include Service Meshes like *Istio* or *Linkerd*, Ingress Controller like *Skipper* and monitoring via *DataDog*.

However, for this project following tools are used:
### NGINX
More specifically the NGINX Ingress Controller is used. It therefore is responsible to traffic routing and
### Prometheus
    tba
### Helm
    tba
### Microsoft Teams
####

More details on the website: [Flagger](https://flagger.app/)
## 3. Architecture
Following diagram shows how the components work together for the canary rollout:
[![N|Solid](https://raw.githubusercontent.com/fluxcd/flagger/main/docs/diagrams/flagger-nginx-overview.png)](https://raw.githubusercontent.com/fluxcd/flagger/main/docs/diagrams/flagger-nginx-overview.png)

# 4. Milestones and Responsibilities
- *1 - Get required tools running: Flagger, NGINX, Helm*
    **Resonsible:** Hanreich Martin, **Working:** All
    **Date:** 5.1.2022
- *2 - Perform the canary deployment*
    **Resonsible**: Cristina Mafra, **Working:** All
    **Date:** 12.1.2022
- *3 - Demo automatic rollback*
    **Resonsible**: Hanreich Martin, **Working:** Hanreich Martin
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
