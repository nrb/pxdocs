---
title: Resize a Portworx PVC
weight: 4
linkTitle: Resize PVCs
keywords: portworx, storage class, container, Kubernetes, storage, Docker, k8s, flexvol, pv, persistent disk,StatefulSets
description: Step-by-step tutorial on how to resize a Portworx volume with Kubernetes
series: k8s-vol
---

This document describes how to dynamically resize a volume (PVC) using Kubernetes and Portworx.

## Pre-requisites

* Resize support for PVC is in Kubernetes 1.11 and above. If you have an older version, use [pxctl volume update](/reference/cli/updating-volumes) to update the volume size.
* The StorageClass must have `allowVolumeExpansion: true`.
* The PVC must be in use by a Pod.

## Example

To resize a Portworx PVC, you can simply edit the PVC spec and update the size. Let's take an example of resizing a mysql PVC.

1. Download the [MySQL StorageClass spec](/samples/k8s/mssql/mssql_sc.yml?raw=true) and apply it. Note that the StorageClass has `allowVolumeExpansion: true`
2. Download the [MySQL PVC spec](/samples/k8s/mssql/mssql_pvc.yml?raw=true) and apply it. We will start with a 5GB volume.
3. Download the [MySQL Deployment spec](/samples/k8s/mssql/mssql_deployment.yml?raw=true) and apply it. Wail till the pod becomes 1/1 and then proceed to next step.
4. Run `kubectl edit pvc mssql-data` and change the size in the "spec" to 10Gi.

After you save the spec, `kubectl describe pvc mssql-data` should have an entry like below that confirms the volume resize.

```
Normal  VolumeResizeSuccessful  5s    volume_expand                ExpandVolume succeeded for volume default/mssql-data
```
