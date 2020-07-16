# CSI-migration Full Sync testing

Volume created with in-tree storage class by in-tree plugin
<pre>
root@kube-master:~# kubectl get pvc -A
NAMESPACE   NAME               STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
default     vcppvc-with-ds-1   Bound    pvc-6587c2cb-7625-4484-8404-c883ca3f90d0   1Gi        RWO            vcpsc          54s
default     vcppvc-with-ds-2   Bound    pvc-2226d604-94d7-4358-af43-495388018f1c   1Mi        RWO            vcpsc          50s
</pre>

Enable CSI migration and deploy the CSI driver
<pre>
root@kube-master:~# kubectl describe pv pvc-6587c2cb-7625-4484-8404-c883ca3f90d0
Name:            pvc-6587c2cb-7625-4484-8404-c883ca3f90d0
Labels:          silver=yes
Annotations:     kubernetes.io/createdby: vsphere-volume-dynamic-provisioner
                 pv.kubernetes.io/bound-by-controller: yes
                 pv.kubernetes.io/migrated-to: csi.vsphere.vmware.com
                 pv.kubernetes.io/provisioned-by: kubernetes.io/vsphere-volume
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    vcpsc
Status:          Bound
Claim:           default/vcppvc-with-ds-2
Reclaim Policy:  Delete
Access Modes:    RWO
VolumeMode:      Filesystem
Capacity:        1Gi
Node Affinity:   <none>
Message:         
Source:
    Type:               vSphereVolume (a Persistent Disk resource in vSphere)
    VolumePath:         [vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e0c39595-532b-48f1-a219-1ec3c09e0b95.vmdk
    FSType:             ext4
    StoragePolicyName:  vSAN Default Storage Policy
Events:                 <none>
</pre>

Observe CRD creation:
<pre>
root@kube-master:~# kubectl describe cnsvspherevolumemigrations.cns.vmware.com 42618de1-3b93-4089-aefb-7cda81d6a774
Name:         42618de1-3b93-4089-aefb-7cda81d6a774
Namespace:    
Labels:       <none>
Annotations:  <none>
API Version:  cns.vmware.com/v1alpha1
Kind:         CnsVSphereVolumeMigration
Metadata:
  Creation Timestamp:  2020-07-09T07:24:10Z
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
    Time:            2020-07-09T07:24:10Z
  Resource Version:  5046594
  Self Link:         /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7ec993d6-7ab2-4a0a-972c-d47936f30531
  UID:               30007f3c-5d77-4ea5-8fc3-7221941b2691
Spec:
  Volumeid:    42618de1-3b93-4089-aefb-7cda81d6a774
  Volumepath:  [vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e0c39595-532b-48f1-a219-1ec3c09e0b95.vmdk
Events:        <none>
</pre>

Wait for Full Sync to be triggered and watch volume metadata getting updated in CNS UI:
[CNS UI](https://github.com/chethanv28/CSI-migration-syncer-logs/blob/master/Full-sync.png)
