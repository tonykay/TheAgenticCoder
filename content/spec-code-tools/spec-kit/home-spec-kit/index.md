---
title: "Getting Started with Spec"
date: "2025-10-12"
linktitle: spec-start
slug: "getting-started-with-spec"
thumbnail: "images/terminal-refactor-80x12.png"
categories:
  - "Spec Coding"
tags:
  - Spec Coding
  - Refactor
---


# Test Page Spec 

lorem ipsum dolor sit amet consectetur adipiscing elit

> Some initial notes I have about this and about that
> ...
> yada yada yada
>

## sec

### foo `me`
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