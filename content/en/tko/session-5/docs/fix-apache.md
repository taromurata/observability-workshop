---
title: Fix PHP/Apache Issue
linkTitle: Fix PHP/Apache Issue
weight: 3
---
## 1. Kubernetes Resources

Especially in Production Kubernetes Cluster CPU and Memory are considered precious resources.And  the Cluster operators will normally require you to specify in the deployment the amount of CPU and Memory your Pod or service will require, so they can have the cluster automatically manage on which Node(s) your solution will be placed.

You do this by placing a  Resource section in the deployment of you application/Pod

**Example:**

``` yaml
resources:
      limits:           # Maximum amount of CPU & memory for peek use
        cpu: "1"        # Maximum of 1 core of CPU allowed at for peek use
        memory: "16Mi"  # Maximum allowed 16 Megabyte of memory
      requests:         # Request are the expected amount of CPU & memory for normal use
        cpu: "0.5"      # Requesting half of Core of a CPU
        memory: "10Mi"  # Requesting 10 Megabyte of memory
```

More information can be found here : [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)

If your application or pod will go over the limits set in your deployment, Kubernetes will kill and restart your Pod to protect the other applications on the Cluster.

An other scenario that you will run into is when there is not enough Memory or CPU on a Node. In that case, the cluster will try to reschedule your pod(s) on a different node with more space.

If that fails, or if there is not enough space when you deploy your application, the Cluster will put your workload/deployment in schedule mode until there are enough room on any of the available nodes to deploy the pods according their limits.

## 2. Fix PHP/Apache Deployment

{{% alert title="Workshop Question" color="danger" %}}

Before we start, let's check the current status of the PHP/Apache deployment. Which Auto-Detect detector has fired?

{{% /alert %}}

To fix the PHP/Apache deployment, edit the deployment and reduce the CPU resources further:

```bash
kubectl edit deployment php-apache -n apache
```

Find the resources section and reduce the CPU limits to **1** and the CPU requests to **0.5** e.g.

``` yaml
resources:
  limits:
    cpu: "1"
    memory: "16Mi"
  requests:
    cpu: "0.5"
    memory: "10Mi"
```

Save the above changes. The deployment will be updated and the pods will be restarted. You can validate the changes have been applied by running the following command:

## 3. Validate the changes

``` bash
kubectl describe deployment php-apache -n apache
```

Validate the pod is now running in Splunk Observability Cloud.

{{% alert title="Workshop Question" color="danger" %}}

Is the **Apache Web Servers** dashboard showing any data?

**Tip:** Don't forget to use filters and timeframes to narrow down your data.

{{% /alert %}}
