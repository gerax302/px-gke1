# Enable credentials for Application Backups (namespaces) in Google Cloud Storage.

To activate the ServiceAccount as a secret we have 2 different ways:

#### 1. Define the SA directly in the Backup Location CRD.
   - Advantajes: 
     - Fastest way.
     - In just one yaml file (bl-explicit-demo.yaml) you declare both objects.
   - Disadvantage: 
     - Secret will be shown when describing stork objects (like `kubectl describe backuplocation.stork.libopenstorage.org -n kube-system`)  

#### 2. Define a Secret, then make reference to it in the Backup Location CRD.
   - Advantajes: 
     - Secret is secure, even when describe the objects. 
     - In future references, this can be automated with TF.
   - Disadvantage: 
     - Use 2 files (secret.yam) & (bl-ref-secret.yaml)
