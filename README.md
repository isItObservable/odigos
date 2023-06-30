# Is it Observable
<p align="center"><img src="/image/logo.png" width="40%" alt="Is It observable Logo" /></p>

## Episode : WHat is Odigo
This repository contains the files utilized during the tutorial presented in the dedicated IsItObservable episode related Odigos.
<p align="center"><img src="/image/odigos_logo.png" width="40%" alt="Odigos Logo" /></p>

This repository showcase the usage of Odigos  with :
* The hipster-shop
* The OpenTelemetry demo
* Dynatrace


We will send all Telemetry data produced by Odigos to Dynatrace.

## Prerequisite
The following tools need to be install on your machine :
- jq
- kubectl
- git
- gcloud ( if you are using GKE)
- Helm


## Deployment Steps in GCP

You will first need a Kubernetes cluster with 2 Nodes.
You can either deploy on Minikube or K3s or follow the instructions to create GKE cluster:
### 1.Create a Google Cloud Platform Project
```shell
PROJECT_ID="<your-project-id>"
gcloud services enable container.googleapis.com --project ${PROJECT_ID}
gcloud services enable monitoring.googleapis.com \
    cloudtrace.googleapis.com \
    clouddebugger.googleapis.com \
    cloudprofiler.googleapis.com \
    --project ${PROJECT_ID}
```
### 2.Create a GKE cluster
```shell
ZONE=europe-west3-a
NAME=isitobservable-odigos
gcloud container clusters create "${NAME}" --zone ${ZONE} --machine-type=e2-standard-8 --num-nodes=2
```


## Getting started
### Dynatrace Tenant
#### 1. Dynatrace Tenant - start a trial
If you don't have any Dyntrace tenant , then i suggest to create a trial using the following link : [Dynatrace Trial](https://bit.ly/3KxWDvY)
Once you have your Tenant save the Dynatrace tenant url in the variable `DT_TENANT_URL` (for example : https://dedededfrf.live.dynatrace.com)
```
DT_TENANT_URL=<YOUR TENANT Host>
```

#### 2. Create the Dynatrace API Tokens
Create a Dynatrace token with the following scope ( left menu Acces Token):
* ingest metrics
* ingest OpenTelemetry traces
* ingest logs
<p align="center"><img src="/image/data_ingest.png" width="40%" alt="data token" /></p>
Save the value of the token . We will use it later to store in a k8S secret

```
DATA_INGEST_TOKEN=<YOUR TOKEN VALUE>
```

### 2.Clone the Github Repository
```shell
https://github.com/isItObservable/odigos
cd odigos
```

### 3.Deploy most of the components
The application will deploy the entire environemnt: 
```shell
chmod 777 deployment.sh
./deployment.sh   --dthost "${DT_TENANT_URL}" --dttoken "${DATA_INGEST_TOKEN}" 
```
### 5.Configure Odigo
#### 1. From the ui: 
```shell
kubectl port-forward svc/odigos-ui 3000:3000 -n odigos-system
```
Select the namespace Hipster-shop and click on "everything in this namespace"
<p align="center"><img src="/image/odigos-io.png" width="40%" alt="odigos ui" /></p>


#### 2. From the cmdline

#### 1. Instrument the hipster-shop namespace
to let odigos instrument the hipster-shop we need to label the namespace :
```shell
kubectl label ns hipster-shop odigos-instrumentation=enabled
```

#### 2. Create the Dynatrace destination

to create the dynatrace destination , we first need to create the dynatrace secret in the odigos-system
```shell

```


