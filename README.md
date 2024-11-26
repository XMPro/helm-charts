# XMPRO Stream Host Helm Chart

This Helm chart deploys XMPRO Stream Host on a Kubernetes cluster.

## Prerequisites

- Kubernetes 1.16+
- Helm 3.0+
- kubectl configured to communicate with your cluster
- Access to XMPRO Azure Container Registry (xmpro.azurecr.io)

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install my-release .
```

## Configuration

The following table lists the configurable parameters of the XMPRO Stream Host chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `sh.replicaCount` | Number of Stream Host replicas | `1` |
| `sh.image.repository` | Stream Host image repository | `xmpro.azurecr.io/stream-host` |
| `sh.image.tag` | Stream Host image tag | `latest` |
| `sh.image.pullPolicy` | Image pull policy | `Always` |
| `sh.environment.KUBE_DEPLOYMENT` | Enable Kubernetes deployment mode | `"true"` |
| `sh.environment.xm__xmpro__Gateway__Name` | XMPRO Gateway Name | `""` |
| `sh.environment.xm__xmpro__Gateway__ServerUrl` | XMPRO Gateway Server URL | `""` |
| `sh.environment.xm__xmpro__Gateway__CollectionId` | XMPRO Gateway Collection ID | `""` |
| `sh.environment.xm__xmpro__Gateway__Secret` | XMPRO Gateway Secret | `""` |
| `sh.podAnnotations` | Additional pod annotations | `{}` |
| `sh.labels` | Additional labels for the deployment | `{}` |
| `sh.selectorLabels` | Additional selector labels | `{}` |

### Example values.yaml

```yaml
sh:
  podAnnotations: {}
  replicaCount: 1
  
  image:
    repository: xmpro.azurecr.io/stream-host
    pullPolicy: Always
    tag: "latest"
  
  environment:
    KUBE_DEPLOYMENT: "true"
    xm__xmpro__Gateway__Name: "exampleprefix"
    xm__xmpro__Gateway__ServerUrl: "https://ds.example.2svzxmpro.com/"
    xm__xmpro__Gateway__CollectionId: "exampleid"
    xm__xmpro__Gateway__Secret: "examplesecret"
  
  labels: {}
  selectorLabels: {}
```

## Required Configuration

The following environment variables must be configured in your values.yaml:

- `xm__xmpro__Gateway__Name`: The prefix used to identify your stream host
- `xm__xmpro__Gateway__ServerUrl`: The URL of your XMPRO Data Stream Designer instance
- `xm__xmpro__Gateway__CollectionId`: Your XMPRO collection ID
- `xm__xmpro__Gateway__Secret`: Your XMPRO gateway secret

## Image Pull Secrets

If you need to authenticate to pull images from xmpro.azurecr.io, you'll need to create an image pull secret:

```bash
kubectl create secret docker-registry xmpro-registry-secret \
  --docker-server=xmpro.azurecr.io \
  --docker-username=<username> \
  --docker-password=<password>
```

Then reference it in your values.yaml:

```yaml
imagePullSecrets:
  - name: xmpro-registry-secret
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm uninstall my-release
```

## Security Considerations

- Ensure your Gateway Secret is stored securely and not committed to version control
- Consider using Kubernetes Secrets for sensitive environment variables
- Use network policies to restrict access to the Stream Host pods if required

## Troubleshooting

Common issues:

1. Image Pull Errors
   - Verify your image pull secrets are configured correctly
   - Ensure you have access to xmpro.azurecr.io

2. Connection Issues
   - Verify the `xm__xmpro__Gateway__ServerUrl` is accessible from your cluster
   - Check that the Gateway Secret is correct

## Support

For support with the XMPRO Stream Host, please contact XMPRO support or refer to the XMPRO documentation.
