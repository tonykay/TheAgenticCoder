---
title: "News - Under Construction"
date: "2025-10-31"
linktitle: news-under-construction
slug: news-under-construction
thumbnail: "../../images/under-construction.png"
weight: 10
categories:
  - News
  - Resources
tags:
  - Spec Coding
  - News
---


# Coming Soon - news items!

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
