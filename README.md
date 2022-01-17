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
Prometheus is an open-source systems monitoring and alerting toolkit. Prometheus collects and stores its metrics as time series data, i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels. Prometheus scrapes metrics from instrumented jobs, either directly or via an intermediary push gateway for short-lived jobs. It stores all scraped samples locally and runs rules over this data to either aggregate and record new time series from existing data or generate alerts.<img width="471" alt="image" src="https://user-images.githubusercontent.com/84008097/149830685-38f5cf99-7052-4633-8c90-f284478795ef.png">


More details on the website: [Prometheus](https://prometheus.io/docs/introduction/overview/)

### Helm
Helm is a tool used to manage Kubernetes applications that simplify their deployment through a collection of files, known as *Charts*. The *Charts* describe a related set of Kubernetes resources and assist in defining, installing and updating applications regardless of their complexity.

More details on the website: [Helm](https://helm.sh/)

### Microsoft Teams
With the help of Prometheus alerting through AlertManager we will be able to send alert messages via Teams. For that we have to configure incoming Webhooks in Teams. This will then generate a webhook URL which will be used to post messages to our channel. To send the third-party tool for Microsoft Teams, an alertmanager.yaml file will be needed. So that our AlertManager is ready for sending alerts using webhook, a Secret object - which is used by Prometheus operator’s AlertManager and then applied to Kubernetes - must be created. Last but not least a default Microsoft Teams Message card template has to be defined and a Kubernetes manifest with Helm passing “Incoming Webhook URL” of Microsoft Teams must be generated. This will be finally deployed to Kubernetes and makes it possible to receive alerts in our Microsoft Teams Channel. 

More details on the website: [msteams](https://github.com/prometheus-msteams/prometheus-msteams)

A deep look into Flagger and the tools: [Flagger](https://flagger.app/)

## 3. Architecture
Following diagram shows how the previous described components work together for the canary rollout:
[![N|Solid](https://raw.githubusercontent.com/fluxcd/flagger/main/docs/diagrams/flagger-nginx-overview.png)](https://raw.githubusercontent.com/fluxcd/flagger/main/docs/diagrams/flagger-nginx-overview.png)

## 3. Microsoft Teams Alert Step by Step Guide

#### Set up Microsoft Teams
Go to the channel, where you want to receive alerts. Click on the ... on the right side of the channel name and select Connectors from the dropdown list.

<img width="471" alt="pic1" src="https://user-images.githubusercontent.com/84008097/149829568-bc9f5594-3435-44ef-821b-620b7c3e2040.png">

A window will then open. Add "Incoming Webhook".

![pic2](https://user-images.githubusercontent.com/84008097/149829888-4bcb8b2c-aa22-433b-8def-109e6c08013c.png)

Now the connector "Incoming Webhook" opens where you have to select "Add" one more time.

![pic3](https://user-images.githubusercontent.com/84008097/149829956-c4d52672-bb21-405b-b3cd-31f3957d6115.png)

To configure the connector now you have to select the pre-selected channel again by Clicking on the ... on the right side of Channel name via the connector. Again select Connectors from the dropdown list. Next click on the "Configure" button next to “Incoming Webhook”. 

![pic4](https://user-images.githubusercontent.com/84008097/149830023-9269bc5c-ab12-4f81-89bf-420e3129590d.png)

After that, you need to specify a name as well as an image for the IncomingWebhook connection. When you have completed these steps, a URL will be created. Copy it to the clipboard and click on "Done". You will need this URL when you switch to the service that will send data to your groups.

![pic5](https://user-images.githubusercontent.com/84008097/149830061-431ed6d4-585f-45d0-8d4f-a4139c1f0a77.png)
![pic6](https://user-images.githubusercontent.com/84008097/149830164-716f14ec-04fa-478f-aec6-a17b7947f16f.png)


#### Configure Alertmanager via Prometheus
To send the third-party tool for Microsoft Teams, generate a alertmanager.yaml file to configure Alertmanager.

<img width="471" alt="pic7" src="https://user-images.githubusercontent.com/84008097/149830223-d341f974-81bb-4e61-bdeb-5beb097077c0.png">

<img width="471" alt="pic8" src="https://user-images.githubusercontent.com/84008097/149830614-1076cdb7-79f3-4368-8d06-df809e55be1f.png">

```
global:
  resolve_timeout: 5m
receivers:
- name: prometheus-msteams
  webhook_configs:
  - url: "http://prometheus-msteams:2000/alertmanager"
    send_resolved: true
route:
  group_by:
  - job
  group_interval: 5m
  group_wait: 30s
  receiver: prometheus-msteams
  repeat_interval: 12h
  routes:
  - match:
      alertname: Watchdog
    receiver: prometheus-msteams
```

The above file uses webhook as the recipient and also specifies the name of prometheus-msteams. Next, encode the alertmanager.yaml with base64.

```
cat alertmanager.yaml | base64
Z2xvYmFsOgogIHJlc29sdmVfdGltZW91dDogNW0KcmVjZWl2ZXJzOgotIG5hbWU6IHByb21ldGhldXMtbXN0ZWFtcwogIHdlYmhvb2tfY29uZmlnczoKICAtIHVybDogImh0dHA6Ly9wcm9tZXRoZXVzLW1zdGVhbXM6MjAwMC9hbGVydG1hbmFnZXIiCiAgICBzZW5kX3Jlc29sdmVkOiB0cnVlCnJvdXRlOgogIGdyb3VwX2J5OgogIC0gam9iCiAgZ3JvdXBfaW50ZXJ2YWw6IDVtCiAgZ3JvdXBfd2FpdDogMzBzCiAgcmVjZWl2ZXI6IHByb21ldGhldXMtbXN0ZWFtcwogIHJlcGVhdF9pbnRlcnZhbDogMTJoCiAgcm91dGVzOgogIC0gbWF0Y2g6CiAgICAgIGFsZXJ0bmFtZTogV2F0Y2hkb2cKICAgIHJlY2VpdmVyOiBwcm9tZXRoZXVzLW1zdGVhbXMK
```


A Secret object is then created. This is used by Alertmanager, and applies to Kubernetes.

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: alertmanager-main
  namespace: monitoring
type: Opaque
data:
  alertmanager.yaml: Z2xvYmFsOgogIHJlc29sdmVfdGltZW91dDogNW0KcmVjZWl2ZXJzOgotIG5hbWU6IHByb21ldGhldXMtbXN0ZWFtcwogIHdlYmhvb2tfY29uZmlnczoKICAtIHVybDogImh0dHA6Ly9wcm9tZXRoZXVzLW1zdGVhbXM6MjAwMC9hbGVydG1hbmFnZXIiCiAgICBzZW5kX3Jlc29sdmVkOiB0cnVlCnJvdXRlOgogIGdyb3VwX2J5OgogIC0gam9iCiAgZ3JvdXBfaW50ZXJ2YWw6IDVtCiAgZ3JvdXBfd2FpdDogMzBzCiAgcmVjZWl2ZXI6IHByb21ldGhldXMtbXN0ZWFtcwogIHJlcGVhdF9pbnRlcnZhbDogMTJoCiAgcm91dGVzOgogIC0gbWF0Y2g6CiAgICAgIGFsZXJ0bmFtZTogV2F0Y2hkb2cKICAgIHJlY2VpdmVyOiBwcm9tZXRoZXVzLW1zdGVhbXMK
EOF
```

Now Alertmanager is ready for sending alerts via webhook.

##### Set externalLabels for multiple clusters
In the case of multiple Kubernetes cluster, you have to idenfy which cluster is generating alerts. Add cluster: <CLUSTER_NAME> in externalLabels to Prometheus CRD so that alerts have information of the cluster.

```
cat <<EOF | kubectl apply -f -
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  labels:
    prometheus: k8s
  name: k8s
  namespace: monitoring
spec:
  alerting:
    alertmanagers:
    - name: alertmanager-main
      namespace: monitoring
      port: web
  baseImage: quay.io/prometheus/prometheus
  nodeSelector:
    beta.kubernetes.io/os: linux
  replicas: 2
  resources:
    requests:
      memory: 400Mi
  ruleSelector:
    matchLabels:
      prometheus: k8s
      role: alert-rules
  securityContext:
    fsGroup: 2000
    runAsNonRoot: true
    runAsUser: 1000
  serviceAccountName: prometheus-k8s
  serviceMonitorNamespaceSelector: {}
  serviceMonitorSelector: {}
  version: v2.7.2
  externalLabels:
    cluster: test
```

#### Set up Prometheus - MS-teams
This step sets up prometheus-msteams, which sends alerts generated by Prometheus to Microsoft Teams.

Clone the following repository.
```
git clone https://github.com/bzon/prometheus-msteams
cd prometheus-msteams/chart
```

Modify the default Microsoft Teams Message card template

```
vim prometheus-msteams/card.tmpl
{{ define "teams.card" }}
{
  "@type": "MessageCard",
  "@context": "http://schema.org/extensions",
  "themeColor": "{{- if eq .Status "resolved" -}}2DC72D
                 {{- else if eq .Status "firing" -}}
                    {{- if eq .CommonLabels.severity "critical" -}}8C1A1A
                    {{- else if eq .CommonLabels.severity "warning" -}}FFA500
                    {{- else -}}808080{{- end -}}
                 {{- else -}}808080{{- end -}}",
  "summary": "{{- if eq .CommonAnnotations.summary "" -}}
                  {{- if eq .CommonAnnotations.message "" -}}
                    {{- js .CommonLabels.cluster | reReplaceAll "_" " " | reReplaceAll `\\'` "'" -}}
                  {{- else -}}
                    {{- js .CommonAnnotations.message | reReplaceAll "_" " " | reReplaceAll `\\'` "'" -}}
                  {{- end -}}
              {{- else -}}
                  {{- js .CommonAnnotations.summary | reReplaceAll "_" " " | reReplaceAll `\\'` "'" -}}
              {{- end -}}",
  "title": "Prometheus Alert ({{ .Status }})",
  "sections": [ {{$externalUrl := .ExternalURL}}
  {{- range $index, $alert := .Alerts }}{{- if $index }},{{- end }}
    {
      "activityTitle": "[{{ js $alert.Annotations.description |  reReplaceAll "_" " " | reReplaceAll `\\'` "'" }}]({{ $externalUrl }})",
      "facts": [
        {{- range $key, $value := $alert.Annotations }}
        {
          "name": "{{ $key }}",
          "value": "{{ js $value | reReplaceAll "_" " " | reReplaceAll `\\'` "'" }}"
        },
        {{- end -}}
        {{$c := counter}}{{ range $key, $value := $alert.Labels }}{{if call $c}},{{ end }}
        {
          "name": "{{ $key }}",
          "value": "{{ js $value | reReplaceAll "_" " " | reReplaceAll `\\'` "'" }}"
        }
        {{- end }}
      ],
      "markdown": true
    }
    {{- end }}
  ]
}
{{ end }}
```

Deploy Prometheus - MS-Teams to Kubernetes 
In this step of using “helm template” command a Kubernetes manifest with Helm passing “Incoming Webhook URL” of Microsoft Teams will be generated and to Kubernetes deployed.

```
helm template prometheus-msteams/ --name prometheus-msteams --set connectors.alertmanager=https://outlook.office.com/webhook/xxxx/xxxx | kubectl apply -f -
configmap/prometheus-msteams-config created
configmap/prometheus-msteams-card-template created
service/prometheus-msteams created
deployment.apps/prometheus-msteams created
```

Validate incoming alerts via Microsoft Teams
Finally, you will receive incoming alerts in MS Teams in the defined template format.

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
