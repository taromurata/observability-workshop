---
title: Setup Horizontal Pod Autoscaler
linkTitle: Setup HPA
weight: 5
---

In Kubernetes, a HorizontalPodAutoscaler automatically updates a workload resource (such as a Deployment or StatefulSet), with the aim of automatically scaling the workload to match demand.

Horizontal scaling means that the response to increased load is to deploy more Pods. This is different from vertical scaling, which for Kubernetes would mean assigning more resources (for example: memory or CPU) to the Pods that are already running for the workload.

If the load decreases, and the number of Pods is above the configured minimum, the HorizontalPodAutoscaler instructs the workload resource (the Deployment, StatefulSet, or other similar resource) to scale back down.

## 1. Setup HPA

Create an autoscaling deployment for when the CPU usage for `php-apache` deployment goes above 50% with a minimum of 1 pod and a maximum of 4 pods.

``` text
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=4 -n apache
```

{{% alert title="Workshop Question" color="danger" %}}

How many additional **php-apache** pods are created?

{{% /alert %}}

## 2. Validate HPA

``` text
kubectl get hpa -n apache
```

{{% alert title="Workshop Question" color="danger" %}}

Which metrics in the Apache Dashboards have significantly increased?

{{% /alert %}}
