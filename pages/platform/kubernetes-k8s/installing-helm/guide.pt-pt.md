---
title: Installing Helm on OVHcloud Managed Kubernetes
slug: installing-helm
excerpt: 'Find out how to install Helm on OVHcloud Managed Kubernetes'
section: Tutorials
order: 4
routes:
    canonical: 'https://docs.ovh.com/gb/en/kubernetes/installing-helm/'
updated: 2023-01-11
---

**Last updated 11th January 2023**

<style>
 pre {
     font-size: 14px;
 }
 pre.console {
   background-color: #300A24; 
   color: #ccc;
   font-family: monospace;
   padding: 5px;
   margin-bottom: 5px;
 }
 pre.console code {
   border: solid 0px transparent;
   font-family: monospace !important;
 }
 .small {
     font-size: 0.75em;
 }
</style>

[Helm](https://docs.helm.sh/){.external} is a package manager for Kubernetes. It works with packages of pre-configured Kubernetes resources, called Helm charts. 

With Helm you can:

- find, deploy and manage software in Kubernetes [using a growing catalog of Helm charts in ArtifactHUB](https://artifacthub.io/){.external}
- create and share your own Helm charts

## Before you begin

This tutorial assumes that you already have a working OVHcloud Managed Kubernetes cluster, and some basic knowledge of how to operate it. If you want to know more on those topics, please look at the [OVHcloud Managed Kubernetes Service Quickstart](../deploying-hello-world/).

We are assuming that you have the `KUBECONFIG` environment variable pointing to your kubectl configuration file, as described in the Quickstarter. If that's not the case, you can use the `--kubeconfig [LOCATION_OF_CONFIG_FILE]` option in both `kubectl` and `helm` commands. 

## Helm concepts

Helm is built around three big concepts: charts, repositories and releases.

A *chart* is a Helm package. Inside the chart you have all the resource definitions necessary to run an application, tool, or service inside of a Kubernetes cluster. It's the Helm equivalent of a Debian pkg for linux, a Maven file for Java or a `package.json` for Node JS.

Charts are stored in *repositories*, where they can be shared. Repositories are the Helm equivalent of the NPM registry for Node JS or Maven Central for Java.

When a chart is installed in a Kubernetes cluster, the running instance is called a *release*. Multiple releases of a single chart can be installed at the same time in a cluster (think for example several instance of the WordPress chart for several different blogs instances running in the cluster). 


## Installing Helm

> [!warning]
> This guide supposes you're using Helm 3, the latest major version of Helm.
> The precedent version, Helm 2, is in *maintenance mode*, and considered deprecated.
> If you want to use Helm 2, please refer to the [official documentation](https://v2.helm.sh/)

Every release of Helm provides binary releases for a variety of OSes. These binary versions can be manually downloaded and installed. 

The simplest way to install Helm is grabbing the binary release for your platform on the [official release page](https://github.com/helm/helm/releases/latest){.external}. You then just need to unpack the client `helm` binary and add it to your PATH.

> [!primary]
> To use alternative installation procedures, like package managers (Homebrew, Snap etc.), please refer to the [official installation doc](https://docs.helm.sh/using_helm/#installing-helm){.external}.

To check if the `helm` CLI is correctly installed locally, you can display its version:

```bash
helm version
```

You can show the installed version:

<pre class="console"><code>$ helm version
version.BuildInfo{Version:"v3.10.3", GitCommit:"835b7334cfe2e5e27870ab3ed4135f136eecc704", GitTreeState:"clean", GoVersion:"go1.19.4"}
</code></pre>


## Initialize a Helm Chart Repository

> [!warning]
> As with Helm 2, the official Helm `stable` repository is currently deprecated. 
> The Helm community is currently transitioning to a hub model, with a [Helm Hub](https://hub.helm.sh/), where charts can be searched using `helm search hub <keyword>`
> As most charts from the Helm *stable* repository have been transferred to the [Bitnami repository](https://github.com/bitnami/charts/) we are using it in the tutorial.

Once you have Helm ready, you can add a chart repository. The easiest way to begin with Helm is to add the [Bitnami repository](https://github.com/bitnami/charts/):

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Once the repository added, run `helm repo update` to make sure we get the latest list of charts.

<pre class="console"><code>$ helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories

$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈ Happy Helming!⎈
</code></pre>


## Installing an example chart

Let's validate your Helm installation by installing an example chart, the official Redis one, with no persistence:

```bash
helm install test-redis bitnami/redis --set master.persistence.enabled=false
```


This will install the required elements and initialize the services. And at the end, it will give you the connection parameters for your new Redis database:


<pre class="console"><code>$ helm install test-redis bitnami/redis --set master.persistence.enabled=false

helm list -A
NAME: test-redis
LAST DEPLOYED: Wed Jan 11 12:02:08 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: redis
CHART VERSION: 17.4.2
APP VERSION: 7.0.7

** Please be patient while the chart is being deployed **

Redis&reg; can be accessed on the following DNS names from within your cluster:

    test-redis-master.default.svc.cluster.local for read/write operations (port 6379)
    test-redis-replicas.default.svc.cluster.local for read-only operations (port 6379)



To get your password run:

    export REDIS_PASSWORD=$(kubectl get secret --namespace default test-redis -o jsonpath="{.data.redis-password}" | base64 -d)

To connect to your Redis&reg; server:

1. Run a Redis&reg; pod that you can use as a client:

   kubectl run --namespace default redis-client --restart='Never'  --env REDIS_PASSWORD=$REDIS_PASSWORD  --image docker.io/bitnami/redis:7.0.7-debian-11-r7 --command -- sleep infinity

   Use the following command to attach to the pod:

   kubectl exec --tty -i redis-client \
   --namespace default -- bash

2. Connect using the Redis&reg; CLI:
   REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h test-redis-master
   REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h test-redis-replicas

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace default svc/test-redis-master 6379:6379 &
    REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h 127.0.0.1 -p 6379
</code></pre>

## Verifying your Redis

After installing the chart, follow the instructions on your console to test your Redis deployment and delete it when your tests are finished.

<pre class="console"><code>$ export REDIS_PASSWORD=$(kubectl get secret --namespace default test-redis -o jsonpath="{.data.redis-password}" | base64 -d)

$ kubectl run --namespace default redis-client --restart='Never'  --env REDIS_PASSWORD=$REDIS_PASSWORD  --image docker.io/bitnami/redis:7.0.7-debian-11-r7 --command -- sleep infinity
pod/redis-client created

$ echo $REDIS_PASSWORD
y9WgZhdMHm

$ kubectl exec --tty -i redis-client --namespace default -- bash

I have no name!@redis-client:/$ REDISCLI_AUTH="e3BUbYfAKv" redis-cli -h test-redis-master
test-redis-master:6379> ping
PONG
test-redis-master:6379> exit
I have no name!@redis-client:/$

I have no name!@redis-client:/$ exit
exit

$ kubectl delete pod redis-client
pod "redis-client" deleted
</code></pre>

## Cleaning up

To clean up your cluster, simply delete your Redis installation. You can use `helm list` to get the Redis release, in the current namespace, and then use `helm delete [REDIS_RELEASE]` to uninstall it.

<pre class="console"><code>$ helm list
NAME      	NAMESPACE	REVISION	UPDATED                            	STATUS  	CHART       	APP VERSION
test-redis	default  	1       	2023-01-11 12:02:08.08556 +0100 CET	deployed	redis-17.4.2	7.0.7

$ helm uninstall test-redis
release "test-redis" uninstalled
</code></pre>

## Go further

- If you need training or technical assistance to implement our solutions, contact your sales representative or click on [this link](https://www.ovhcloud.com/pt/professional-services/) to get a quote and ask our Professional Services experts for assisting you on your specific use case of your project.

- Join our community of users on <https://community.ovh.com/en/>.