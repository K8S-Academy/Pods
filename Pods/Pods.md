# Kubernetes Pods

## Overview

A Pod is the smallest deployable and manageable unit in Kubernetes. It is the fundamental building block of the Kubernetes execution model and the object around which most other Kubernetes features are designed. Rather than deploying and operating individual containers directly, Kubernetes always works with Pods.

A Pod represents a group of one or more containers that run together in a shared execution context. These containers may share networking, storage, and parts of the runtime environment, depending on how the Pod is defined. In practice, a Pod acts as an application-specific logical host for one or more tightly related containers.

Although a Pod can contain multiple containers, many Pods contain only a single container. That does not make the Pod abstraction unnecessary. Even in the single-container case, Kubernetes still uses the Pod as the unit of scheduling, networking, lifecycle management, and policy enforcement.

One of the most important characteristics of a Pod is locality. If a Pod contains multiple containers, all of them are always co-located and co-scheduled on the same worker node. A Pod never spans multiple nodes. This property is central to understanding why Pods exist and how they should be designed.

> **Production note:** The YAML examples in this document use raw `Pod` manifests to explain Pod behavior clearly. In real production environments, application Pods are usually created and managed through higher-level controllers such as `Deployment`, `StatefulSet`, `Job`, or `CronJob`.

### Example: A Basic Single-Container Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app.kubernetes.io/name: nginx
    app.kubernetes.io/component: web
spec:
  containers:
    - name: nginx
      image: nginx:1.27.4-alpine
      imagePullPolicy: IfNotPresent
      ports:
        - name: http
          containerPort: 80
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "250m"
          memory: "256Mi"
```

**Why this matters:** Even a simple Pod benefits from production-ready basics such as pinned image tags, consistent labels, named ports, and explicit resource requests and limits. These small details make workloads easier to operate, monitor, and schedule correctly.

---
