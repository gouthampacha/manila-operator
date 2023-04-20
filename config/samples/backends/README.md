# Manila Backend Configuration Samples

This directory includes a set of Manila Backend configuration samples that use
the `kustomize` configuration management tool available through the `oc
kustomize` command.

These samples are not meant to serve as deployment recommendations, just as
working examples to serve as reference.

For each backend there is a `backend.yaml` file containing an overlay for
the `OpenStackControlPlane` with just the backend specific information.

Backend pre-requirements will be listed in that same `backend.yaml` file.
These can range from having to replace the storage system's address and
credentials in a different yaml file, to having to create secrets.

Currently available samples are:

- CephFS Native
- CephFS via Clustered NFS
- Multibackend with CephFS Native and CephFS via Clustered NFS

## Ceph example

Once the OpenStack operators are running in your OpenShift cluster and
the secret `osp-secret` is present, one can deploy OpenStack with a
specific storage backend with single command.  For example for Ceph we can do:
`oc kustomize cephfs-native | oc apply -f -`.

The result of the `oc kustomize cephfs-native` command is a complete
`OpenStackControlPlane` manifest, and we can see its contents by redirecting it
to a file or just piping it to `less`: `oc kustomize cephfs-native | less`.

Creating the basic secret that our samples require can be done using the
`install_yamls` target called `input`.

A complete example when we already have CRC running would be:

```
$ cd install_yamls
$ make ceph
$ make crc_storage openstack input
$ cd ../manila-operator
$ oc kustomize config/samples/backends/cephfs-native | oc apply -f -
```
