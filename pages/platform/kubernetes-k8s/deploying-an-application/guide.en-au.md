---
title: Deploying an application
slug: deploying-an-application
excerpt: 'Find out how to deploy a "Hello World" application on an OVHcloud Managed Kubernetes cluster'
section: Getting started
updated: 2023-06-06
---

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
   font-size: 0.75em;
   color: #ccc;
 }
 .small {
     font-size: 0.75em;
 }
</style>

**Last updated 6th June, 2023.**

## Objective

OVHcloud Managed Kubernetes service provides you with Kubernetes clusters without the hassle of installation or operation. This guide will explain how to deploy a simple *Hello World* application on a OVHcloud Managed Kubernetes cluster.

## Requirements

- an OVHcloud Managed Kubernetes cluster
- at least one node on the cluster (see the [ordering a node](../managing-nodes/) guide for details) 
- a well configured  `kubectl` (see the [configuring kubectl](../configuring-kubectl/) guide for details) 

> [!warning]
> When a __LoadBalancer__ Service resource is created inside a Managed Kubernetes cluster, an associated Public Cloud Load Balancer is automatically created, allowing public access to your K8S application.
> The Public Cloud Load Balancer service is hourly charged and will appear in your Public Cloud project. For more information, please refer to the following documentation: [Network Load Balancer price](https://www.ovhcloud.com/en-au/public-cloud/prices/#network).

## Instructions

### Step 1 - Deploy your first application

The following command will deploy a simple application (nginx image) using a [Kubernetes Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) and a [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/).

Create a new namespace:

```bash
kubectl create ns hello-app
```

Create a `hello.yml` file for our `ovhplatform/hello` Docker image:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world
  labels:
    app: hello-world
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    app: hello-world
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world-deployment
  labels:
    app: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: ovhplatform/hello
        ports:
        - containerPort: 80
```

And apply the file:

```bash
kubectl apply -f hello.yml -n hello-app
```

After applying the YAML file, a new `hello-world` service and the corresponding `hello-world-deployment` deployment are created in the `hello-app` namespace:

<pre class="console"><code>$ kubectl apply -f hello.yml -n hello-app
service/hello-world created
deployment.apps/hello-world-deployment created
</code></pre>

> [!primary]
> The application you have just deployed is a simple Nginx server with a single static *Hello World* page. 
> Basically it just deploys the Docker image [`ovhplatform/hello`](https://hub.docker.com/r/ovhplatform/hello/){.external}.

### Step 2 - List the pods

You have just deployed a `hello-world` service in a pod in your worker node. Let's verify that everything is correct by listing the pods.

```bash
kubectl get pods -n hello-app -l app=hello-world
```

You should see your newly created pod:

<pre class="console"><code>$ kubectl get pods -n hello-app -l app=hello-world
NAME                                           READY     STATUS    RESTARTS   AGE
hello-world-deployment-d98c6464b-7jqvg         1/1       Running   0          47s
</code></pre>

> [!primary]
> In this tutorial, we chose to create a special namespace to isolate our application.
>
> But if you want to use the `default` namespace instead, it's the default Kubernetes namespace so you don't need to specify it in your kubectl commands.

### Step 3 - List the deployments

You can also verify that the deployment is active:

```bash
kubectl get deploy -n hello-app -l app=hello-world
```

And you will see the `hello-service-deployment`:

<pre class="console"><code>$ kubectl get deploy -n hello-app -l app=hello-world
NAME                          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-world-deployment        1         1         1            1           1m
</code></pre>

### Step 4 - List the services

Now, you will use `kubectl` to view your service:

```bash
kubectl get services -n hello-app -l app=hello-world
```

You should see your newly created service:

<pre class="console"><code>$ kubectl get services -n hello-app -l app=hello-world
NAME          TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)        AGE
hello-world   LoadBalancer   10.3.234.211   51.178.69.47   80:31885/TCP   6m54s
</code></pre>

> [!primary]
> In case you get `<pending>` under `EXTERNAL-IP`, it may take a minute or two to provision the LoadBalancer, so try again in a few moments.

### Step 5 - Test your service

Retrieve the URL of the `hello-world` application:

<pre class="console"><code>$ export SERVICE_URL=$(kubectl get svc hello-world -n hello-app -o jsonpath='{.status.loadBalancer.ingress[].ip}')

$ echo "http://$SERVICE_URL/"
http://135.125.83.30/
</code></pre>

Copy/paste the URL in your browser to see your new running `hello-world` application:

![Testing your service](images/deploying_an_application-01.png){.thumbnail}


You can also test the newly created service, in command line, with curl:

```bash
curl $SERVICE_URL
```

You should see your newly created service:

```bash
curl $SERVICE_URL
<!doctype html>

<html>
<head>
<title>OVH K8S</title>
</head>
<style>
.title {
font-size: 3em;
padding: 2em;
text-align: center;
}
</style>
<body>
<div class="title">
<p>Hello from Kubernetes!</p>
<img src="./ovh.svg"/>
</div>
</body>
</html>
```

> [!primary]
> If you have an error message "Failed to connect to 135.125.83.30 port 80: Connection refused", it's normal. The service is starting, so you have to wait a few seconds in order to test it again.

### Step 6 - Clean up

At the end, you can clean up by deleting the service and the deployment.

Let's delete the namespace (deleting the namespace will delete all resources created within it, including the deployment and the service):

```bash
kubectl delete ns hello-app
```

If you list the services, the deployments and the pods, you will see that `hello-world` doesn't exist anymore:

<pre class="console"><code>$ kubectl delete service hello-world -n hello-app
namespace "hello-app" deleted

$ kubectl get services -l app=hello-world -n hello-app
No resources found in hello-app namespace.

$ kubectl get deployments -n hello-app -l app=hello-world
No resources found in hello-app namespace.

$ kubectl get pods -n hello-app -l app=hello-world
No resources found in hello-app namespace.
</code></pre>

## Go further

To learn more about using your Kubernetes cluster the practical way, we invite you to look at our [OVHcloud Managed Kubernetes doc site](../).

- If you need training or technical assistance to implement our solutions, contact your sales representative or click on [this link](https://www.ovhcloud.com/en-au/professional-services/) to get a quote and ask our Professional Services experts for assisting you on your specific use case of your project.

- Join our [community of users](https://community.ovh.com/en/).
