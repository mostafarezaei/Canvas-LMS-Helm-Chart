# Canvas-LMS-Helm-Chart
A simple helm chart to deploy Canvas LMS on kubernetes.

## Prerequisites

* Kubernetes >= 1.20

## Installing the Chart

Add the canvas repository to Helm:

```sh
helm repo add canvas-repo https://github.com/mostafarezaei/Canvas-LMS-Helm-Chart
```

Install the Canvas Helm:

```sh
helm upgrade -i canvas canvas-repo/canvas \
--namespace canvas
```

For Canvas persistent storage you can create a [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims)
and use `--set persistentVolumeClaim.claimName=<PVC-CLAIM-NAME>`.

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: canvas-pvc
  namespace: canvas
  labels:
    app.kubernetes.io/name: canvas
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
EOF
```

```
helm upgrade -i canvas canvas-repo/canvas \
--namespace canvas \
--set persistentVolumeClaim.claimName=canvas-pvc
```

The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `canvas` deployment:

```console
helm delete --purge canvas
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the chart and their default values.

Parameter | Description | Default
--- | --- | ---
`image.repository` | image repository | ``
`image.tag` | image tag | `<VERSION>`
`image.pullPolicy` | image pull policy | `IfNotPresent`
`resources.requests/cpu` | pod CPU request | `100m`
`resources.requests/memory` | pod memory request | `256Mi`
`resources.limits/cpu` | pod CPU limit | `2000m`
`resources.limits/memory` | pod memory limit | `2Gi`
`affinity` | node/pod affinities | None
`nodeSelector` | node labels for pod assignment | `{}`
`tolerations` | list of node taints to tolerate | `[]`
`rbac.create` | if `true`, create and use RBAC resources | `true`
`rbac.pspEnabled` | If `true`, create and use a restricted pod security policy | `false`
`serviceAccount.create` | If `true`, create a new service account | `true`
`serviceAccount.name` | Service account to be used | None
`persistentVolumeClaim.claimName` |  Specify an existing volume claim to be used for Canvas data | None

## Logging in

You can log into Canvas at http://localhost:8080 with the email address "root@sample.com" and the password "rootpass".
