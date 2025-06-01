# Kubernetics Basics

The Purpose of this repository was to present working examples of a basic Kubernetes applications for learning purposes. 

In this guide I will walk you through creating your first namespace, your first service account, your first pod execution, your first deployment execution, and lastly configuring a service to create a canary deployment, then expanding said canary deployment. 

##### For instructions on how to deploy this repos resources; scroll to the "Deploying this Repository" section.

---
## What is a Namespace? 

>
- https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

*"In Kubernetes, namespaces provide a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces."*

In essence, a Kubernetes cluster functions like its own virtual private cloud (VPC), built on the communication and coordination of the node or nodes that make up the cluster. One of these nodes takes on the role of the **control plane**, which acts as the central command center. The control plane hosts the key components—such as the API server, scheduler, controller manager, and etcd—that manage the cluster’s behavior, resources, and application lifecycle.

To deploy applications on a cluster in an organized and scalable way, we use **namespaces**. From a cloud perspective, a namespace is similar to a **VPC** or a logically isolated environment—like a sandbox—that separates workloads, permissions, and resources. This helps teams manage multiple projects or environments (e.g., dev, test, prod) within the same cluster.

Think of it this way: you wouldn’t spin up a virtual machine without first defining the network it will run on. The same logic applies to Kubernetes—resources need a defined **context**, and namespaces provide that boundary.

*Run the below to view all namespaces cluster wide*
```shell
kubectl get namespaces -A 
```

---
## What is a Service Account? 

>
- https://kubernetes.io/docs/concepts/security/service-accounts/

*"A service account is a type of non-human account that, in Kubernetes, provides a distinct identity in a Kubernetes cluster. Application Pods, system components, and entities inside and outside the cluster can use a specific ServiceAccount's credentials to identify as that ServiceAccount."*

Similar to service accounts and IAM identities in cloud providers, **Kubernetes service accounts** give us a way to uniquely identify and assign permissions to applications, workloads, and users within a cluster.

Just like in AWS or GCP, we can deploy applications with restricted permissions—limiting what resources they can create, modify, or view—within their namespace or across the cluster. We can also define user access boundaries and create service identities for internal or external agents, such as Jenkins, CI/CD tools like Harness, or external APIs, to securely perform operations inside the cluster.

Just as in the IAM space of any typical cloud provider, it’s critical to be **intentional and restrictive** when assigning roles and permissions—especially in production environments. Overprovisioning access in a Kubernetes cluster can introduce serious security risks, so follow the principle of least privilege wherever possible.

*Run the below to view all service accounts cluster wide*
```shell
kubectl get sa -A
```

---
## What are Pods/Deployments? 

>
- https://kubernetes.io/docs/concepts/workloads/pods/

*"Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.*

*A Pod (as in a pod of whales or pea pod) is a group of one or more [containers](https://kubernetes.io/docs/concepts/containers/), with shared storage and network resources, and a specification for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host."*

- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

*"A Deployment manages a set of Pods to run an application workload, usually one that doesn't maintain state."*

Based on the definitions above, for clarity and practicality: **Pods** are the smallest logical units we can deploy in Kubernetes, designed to host one or more application containers. In short, they serve as the **runtime environments** for the Docker images we want to deploy.

From the perspective of the container, each pod behaves like its own **individual machine** hosting an application. You can configure various settings at the pod level to control how all the containers inside it operate. While it’s common to have a single container per pod, you’ll later come to understand the value of **multi-container pods**—where secondary containers perform sidecar roles like logging, monitoring, or providing helper functionality for the main application.

A **Deployment** is a Kubernetes object used to manage a group of pods of the same type. It functions similarly to a **managed instance group** or **autoscaling group** in cloud providers, offering automated scaling, self-healing, and rollout strategies to maintain application availability.

*Use the below to view all active pods within the cluster*
```shell
kubectl get pods -A 
```

---
## What is a service? 

>
- https://kubernetes.io/docs/concepts/services-networking/service/

*"In Kubernetes, a Service is a method for exposing a network application that is running as one or more [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) in your cluster."*

A **Service** in Kubernetes is used to expose one or more applications running in your cluster—either internally to other resources or externally to the outside world. Think of it like a **virtual router** or **gateway** that allows communication to and from your pods, regardless of where they’re running or how many replicas exist.

Just like you'd configure a firewall or load balancer in a cloud provider to direct traffic to a specific VM or group of instances, a Kubernetes Service groups pods using labels and routes traffic to them on specified ports. This abstraction makes it possible to expose **a single application**, **multiple components of an application**, or even **entire backend services** under one accessible endpoint.

Services can be set up for **internal cluster use only** (like APIs talking to databases), or made **externally accessible** through a `LoadBalancer` or `NodePort`, depending on your use case. You can define different **protocols** (like TCP or UDP), and set up **target ports** that differ from the incoming port—useful when the container listens on a different port than the one clients connect to. 

In short, a Service acts as the **network layer glue** that binds your pods to the rest of the environment—whether it's another workload in the cluster, or a user hitting your app from a browser across the internet.

---
## Deploying this Repository 

Follow the below steps to provision your namespace, service account, applications, and service.
Read the attached notes on the manifests for additional information on resource specifications

```shell
# Create a namespace
kubectl create ns my-first-ns
# Create a service account
kubectl create sa -n my-first-ns my-app-id
# Deploy your first pod
kubectl apply -f basic-pod.yaml
# Deploy your first deployment
kubectl apply -f basic-deployment.yaml
# Deploy your first service
kubectl apply -f basic-service.yaml
# View your active services to access your application
kubectl get svc -n my-first-ns
```

In our zoom meeting we will do a basic cover of imperative commands 