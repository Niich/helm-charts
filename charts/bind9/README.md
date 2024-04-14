# niich notes

### Running on port 53

To get DNS on port 53 the easiest way is to use a LoadBalancer. nodeport will work if you want to use a high number port but windows doesn't like that (linux works fine).

Chart was modified to copy the config-secret to a directory called "working" in an "emptydir" volume mount. This is because bind needs to write to the directory that the zone files are in so it can create and manage jnl cache files. 

Bind also expects to be able to write the updates back to the zone file periodically and if it is a secret it is read only. This setup means that changes will not truly be persistent but since we are managing the DNS entries in k8s using externalDNS and octoDNS they should get recreated automatically but depending on the quantity of records this could cause a problem on pod boot.

### TODO: Add Createing custom Zone with RFC2136 support



# Original README
# bind9

![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 9.18](https://img.shields.io/badge/AppVersion-9.18-informational?style=flat-square)

A Helm chart for Bind9 using the official ISC docker image

## TL;DR
```console
$ helm repo add johanneskastl-bind9-isc https://johanneskastl.github.io/bind9-isc-helm-chart/
$ helm repo update
$ helm install bind9 johanneskastl-bind9-isc/bind9
```

## Installing the Chart
To install the chart with the release name `bind9`:
```console
helm install bind9 johanneskastl-bind9-isc/bind9
```

## Uninstalling the Chart
To uninstall the `bind9` deployment:
```console
helm uninstall bind9
```
The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

Read through the [values.yaml](./values.yaml) file. It has several commented out suggested values.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,
```console
helm install bind9 \
  --namespace bind9 \
  --set mount_configuration_from_secret.secretName=my-named-configuration-secret
    johanneskastl-bind9-isc/bind9
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart.
For example,
```console
helm install bind9 johanneskastl-bind9-isc/bind9 -f values.yaml
```

