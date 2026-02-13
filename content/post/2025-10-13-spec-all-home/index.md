---
title: "Spec - Under Construction"
date: "2025-11-01"
linktitle: spec-under-construction
slug: "spec-under-construction"
thumbnail: "../../images/under-construction.png"
weight: 10
categories:
  - Spec Coding
tags:
  - Spec Coding
  - Vibe Coding
---


# Coming Soon

Placeholder pages for SpecCoding.Com, a home for everything Spec, Spec Driven Development, 

> Some initial notes I have about this and about that
> ...
> yada yada yada
>

Now is the winter of our discount tent

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    app.kubernetes.io/component: background-controller
    app.kubernetes.io/instance: kyverno
    app.kubernetes.io/part-of: kyverno
  name: kyverno:update-deployments
rules:
- apiGroups:
  - apps
  resources:
  - deployments
  verbs:
  - update
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    app.kubernetes.io/component: cleanup-controller
    app.kubernetes.io/instance: kyverno
    app.kubernetes.io/part-of: kyverno
  name: kyverno:clean-violating-resources
rules:
- apiGroups:
  - apps
  resources:
  - deployments
  verbs:
  - get
  - watch
  - list
  - delete
```
