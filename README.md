# Personal Portworx Repo for GKE 

This repo contains examples for:
- SC
- SC and deployment
- Application Backup Strategy

In GCP you need to have an IAM & Service Account for portworx with the following role permissions:
- Editor
- Storage Object Admin
- Storage Object Creator
- Storage Object Viewer

## ETCD Notes 

**From documentation in PX Central:**
You can restrict the nodes that will run the key-value store by labelling your nodes with the label px/metadata-node=true.
Only the nodes with the label will participate in the kvdb cluster.
This allows you to use nodes with dedicated hardware for the key-value store.
Portworx will create and manage an internal key-value store (kvdb) cluster.

For example: kubectl label nodes node1 node2 node3 px/metadata-node=true
If you have 3 nodes, will create an etcd instance by node.

**Observations**
After create GKE Test Env, you will need to label your nodes.
Choose to label just one, for testing purposes.


## Application Backup Strategy Notes

1. Storage Class
2. Scheduled Policy
3. Credentials / Secret
4. Backup Location -->(3)
5. Application Backup --> (4)
6. Application Backup Schedule -->(2)(4)
7. Application Restore

Recommended to have credentials and AB (Application Backup) in **kube-system** namespace

## Portworx

```bash
PX_POD=$(kubectl get pods -l name=portworx -n kube-system -o jsonpath='{.items[0].metadata.name}')
kubectl exec $PX_POD -n kube-system -- /opt/pwx/bin/pxctl status -—color 

kubectl exec -it $PX_POD -n kube-system bash 
alias p=/opt/pwx/bin/pxctl

kubectl get pods -l name=portworx -n kube-system -o wide


PX_POD=$(kubectl get pods -l name=portworx -n kube-system -o jsonpath='{.items[0].metadata.name}')
kubectl exec $PX_POD -n kube-system -- /opt/pwx/bin/pxctl volume list
```

## Stork

#### Get Stork

The following instructions are to you can get the binarie from the pod to your workstation.
```bash
LINUX
STORK_POD=$(kubectl get pods -n kube-system -l name=stork -o jsonpath='{.items[0].metadata.name}') &&
kubectl cp -n kube-system $STORK_POD:/storkctl/linux/storkctl ./storkctl
sudo mv storkctl /usr/local/bin &&
sudo chmod +x /usr/local/bin/storkctl


OSX
STORK_POD=$(kubectl get pods -n kube-system -l name=stork -o jsonpath='{.items[0].metadata.name}') &&
kubectl cp -n kube-system $STORK_POD:/storkctl/darwin/storkctl ./storkctl
sudo mv storkctl /usr/local/bin &&
sudo chmod +x /usr/local/bin/storkctl
```



#### Storkctl (stork cli)
```bash
storkctl version
storkctl get schedulepolicy
storkctl get volumesnapshotschedules
storkctl get volumesnapshots

storkctl get backuplocation -n kube-system
kubectl describe backuplocation.stork.libopenstorage.org -n kube-system

storkctl get applicationbackups -n kube-system
kubectl describe applicationbackup.stork.libopenstorage.org -n kube-system

storkctl get applicationbackupschedule -n kube-system
kubectl describe applicationbackupschedule.stork.libopenstorage.org -n kube-system
```

### Wipe PX Resources
`curl -fsL https://install.portworx.com/px-wipe | bash` 

Here you can find more information:
https://docs.portworx.com/portworx-install-with-kubernetes/operate-and-maintain-on-kubernetes/uninstall/uninstall/#delete-wipe-px-cluster-configuration 