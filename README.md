# Shadowsocks Helm Chart

![License](https://img.shields.io/github/license/predatorray/shadowsocks-helm-chart)

A Helm Chart for Shadowsocks.

## Prerequisite

A [Kubernetes](https://kubernetes.io/) cluster and  [Helm](https://helm.sh/) CLI installed on your laptop.

## Usage

### Add repository

```sh
helm repo add predatorray http://predatorray.github.io/charts
```

See [`helm repo`](https://helm.sh/docs/helm/helm_repo/) for more information.

### Install the chart

```sh
helm install $RELEASE_NAME predatorray/shadowsocks
```

Or, use the command below upgrade an existing release if it has been installed.

```sh
helm upgrade --install $RELEASE_NAME predatorray/shadowsocks
```

After that, all shadowsocks components will be ready on your K8S cluster.

## Configuration

| Parameter                                         | Description                                   | Default |
|---------------------------------------------------|-----------------------------------------------|---------|
| `replicaCount`                                    | Number of Shadowsocks pods                    | `1` |
| `image.repository`                                | Image repository of the Shadowsocks           | `shadowsocks/shadowsocks-libev` |
| `image.pullPolicy`                                | Image pull policy of the Shadowsocks          | `IfNotPresent` |
| `image.tag`                                       | Image tag of the Shadowsocks                  | `v3.3.5` |
| `imagePullSecrets`                                | Image pull secrets                            | |
| `fullnameOverride`                                | Override the fullname of K8S manifests        | `{{ .Release.Name }}` |
| `serviceAccount.create`                           | Create ServiceAccount                         | `true` |
| `serviceAccount.annotations`                      | ServiceAccount annotations                    | |
| `serviceAccount.name`                             | Name of ServiceAccount                        | The fullname of the release |
| `podAnnotations`                                  | Annotations of the Shadowsocks pods           | |
| `podSecurityContext`                              | Security context of the Shadowsocks pods      | |
| `securityContext`                                 | Security context of the Shadowsocks container | |
| `service.type`                                    | Kubernetes service type                       | `ClusterIP` |
| `service.port`                                    | Port where the service (shadowsocks) is exposed | `8388` |
| `service.annotations`                             | Annotations of the service                    | |
| `service.udpLoadBalancer.enabled`                 | If the service is a LoadBalancer, since we cannot create one with mix protocols, a seperate UDP-dedicated LoadBalancer will be created if enabled. | `false` |
| `service.udpLoadBalancer.serviceName`             | Service name of the UDP LoadBalancer          | `{{ .Release.Name }}-udp` |
| `service.udpLoadBalancer.annotations`             | Annotations of the UDP LoadBalancer           | |
| `shadowsocks.method`                              | Encryption method the Shadowsocks (See: `-m <encrypt_method>` in [the Usage](https://github.com/shadowsocks/shadowsocks-libev#usage) ) | `aes-256-gcm` |
| `shadowsocks.password.plainText`                  | Password of the Shadowsocks (See: `-k <password>` in [the Usage](https://github.com/shadowsocks/shadowsocks-libev#usage)) | `passw0rd` |
| `shadowsocks.password.existingSecret.secretName`  | Read password from Secret instead of plain text | |
| `shadowsocks.password.existingSecret.passwordKey` | Secret key name for the password              | `password` |
| `shadowsocks.dnsServers`                          | DNS servers of the Shadowsocks                | `["8.8.8.8", "8.8.4.4"]` |
| `shadowsocks.timeout`                             | Timeout of the Shadowsocks (See: `-t <timeout>` in [the Usage](https://github.com/shadowsocks/shadowsocks-libev#usage)) | 300 |
| `kcptun.enabled` |                                 Enabled Shadowsocks over Kcptun                | `false` |
| `kcptun.port`                                     | Port of the Kcptun                            | `29900` |
| `kcptun.crypt`                                    | The crypt of the Kcptun (See: `--crypt value` option in [the Usage](https://github.com/xtaci/kcptun#usage)) | |
| `kcptun.key.plainText`                            | Key (password) of the Kcptun | `it's a secret` (See: `--key value` option in [the Usage](https://github.com/xtaci/kcptun#usage) |
| `kcptun.key.existingSecret.secretName`            | Read key from Secret instead of plain text    | |
| `kcptun.key.existingSecret.passwordKey`           | Secret key name for the key                   | |
| `kcptun.image.repository`                         | Image repository of the Kcptun                | `xtaci/kcptun` |
| `kcptun.image.pullPolicy`                         | Image pull policy of the Kcptun               | `IfNotPresent` |
| `kcptun.image.tag`                                | Image tag of the Kcptun                       | `v20210103` |
| `resources`                                       | CPU/Memory resource requests/limits           | `{}` | `nodeSelector` | Node selector | `{}` |
| `tolerations`                                     | Tolerations                                   | `[]` |
| `affinity`                                        | Affinity                                      | `{}` |

## Examples

### Shadowsocks with LoadBalancer

```sh
helm upgrade --install shadowsocks predatorray/shadowsocks \
    --set service.type=LoadBalancer
```

### Shadowsocks over Kcptun with LoadBalancer

```sh
helm upgrade --install shadowsocks predatorray/shadowsocks \
    --set service.type=LoadBalancer \
    --set kcptun.enabled=true
```
