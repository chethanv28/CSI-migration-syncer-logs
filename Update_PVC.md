# CSI-migration Syncer testing

Volume created with in-tree storage class by in-tree plugin
<pre>
root@kube-master:~# kubectl get pvc -A
NAMESPACE   NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
default     block-pvc-vcppvc-4   Bound    pvc-d8698e54-e752-4a2c-9676-0b648f153d7e   1Gi        RWO            vcpsc          3m41s
</pre>

Enable CSI migration and deploy the CSI driver
<pre>
root@kube-master:~# kubectl describe pvc block-pvc-vcppvc-4
Name:          block-pvc-vcppvc-4
Namespace:     default
StorageClass:  vcpsc
Status:        Bound
Volume:        pvc-d8698e54-e752-4a2c-9676-0b648f153d7e
Labels:        <none>
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
               pv.kubernetes.io/migrated-to: csi.vsphere.vmware.com       
               volume.beta.kubernetes.io/storage-provisioner: kubernetes.io/vsphere-volume
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      1Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Mounted By:    <none>
Events:
  Type    Reason                 Age    From                         Message
  ----    ------                 ----   ----                         -------
  Normal  ProvisioningSucceeded  2m46s  persistentvolume-controller  Successfully provisioned volume pvc-d8698e54-e752-4a2c-9676-0b648f153d7e using kubernetes.io/vsphere-volume
</pre>

Label the PVC
<pre>
kubectl label pvc block-pvc-vcppvc-4 gold=yes
</pre>


Observe CRD creation:
<pre>
root@kube-master:~# kubectl get cnsvspherevolumemigrations.cns.vmware.com -A
NAME                                   AGE
191c6d51-ed59-4340-9841-638c09f642b7   5m40s
 kubectl describe cnsvspherevolumemigrations.cns.vmware.com 191c6d51-ed59-4340-9841-638c09f642b7
Name:         191c6d51-ed59-4340-9841-638c09f642b7
Namespace:    
Labels:       <none>
Annotations:  <none>
API Version:  cns.vmware.com/v1alpha1
Kind:         CnsVSphereVolumeMigration
Metadata:
  Creation Timestamp:  2020-07-09T17:04:45Z
  Generation:          1
  Managed Fields:
    API Version:  cns.vmware.com/v1alpha1
    Fields Type:  FieldsV1
    fieldsV1:
      f:spec:
        .:
        f:volumeid:
        f:volumepath:
    Manager:         vsphere-syncer
    Operation:       Update
    Time:            2020-07-09T17:04:45Z
  Resource Version:  5152522
  Self Link:         /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/191c6d51-ed59-4340-9841-638c09f642b7
  UID:               79d148cd-6299-4881-80ef-1e10ee85b5b1
Spec:
  Volumeid:    191c6d51-ed59-4340-9841-638c09f642b7
  Volumepath:  [vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8698e54-e752-4a2c-9676-0b648f153d7e.vmdk
Events:        <none>
</pre>


CNS UI:
* Label Update
[CNS UI](https://github.com/chethanv28/CSI-migration-syncer-logs/blob/master/PVC_Updated_On_CNS_UI.png)
* Volume Name
[CNS UI](https://github.com/chethanv28/CSI-migration-syncer-logs/blob/master/VolumeID_For_PVC_Update.png)
