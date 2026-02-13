---
title: "Getting Started with Frameworkless"
date: "2025-10-12"
linktitle: spec-frameworkless-home
slug: "getting-started-with-frameworkless"
thumbnail: "../../images/under-construction.png"
#thumbnail: dummy-image.png
weight: 20
categories:
  #  - Spec Coding
  - Frameworkless
tags:
  - Spec Coding
  - Refactor
---


# Frameworkless

lorem ipsum dolor sit amet consectetur adipiscing elit

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
