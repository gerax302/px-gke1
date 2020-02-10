# Personal Portworx Repo for GKE 

This repo contains examples for:
- SC
- SC and deployment
- Application Backup Strategy



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


