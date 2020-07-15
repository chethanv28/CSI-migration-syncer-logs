
# CSI-migration Syncer testing

Volume created with in-tree storage class by in-tree plugin
<pre>
root@kube-master:~# kubectl get pvc -A
NAMESPACE   NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
default     vcppvc-with-ds-1  Bound    pvc-716f36d3-e574-4cb3-b4df-d703b0764f1c   1Gi        RWO            vcpsc          3m41s
</pre>

Enable CSI migration and deploy the CSI driver
<pre>
root@kube-master:~# kubectl describe pvc vcppvc-with-ds-1
Name:          vcppvc-with-ds-1
Namespace:     default
StorageClass:  vcpsc
Status:        Bound
Volume:        pvc-716f36d3-e574-4cb3-b4df-d703b0764f1c
Labels:        gold=yes
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
               pv.kubernetes.io/migrated-to: csi.vsphere.vmware.com
               volume.beta.kubernetes.io/storage-provisioner: kubernetes.io/vsphere-volume
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      1Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Mounted By:    <none>
Events:        <none>
</pre>


Observe CRD creation:
<pre>
root@kube-master:~# kubectl get cnsvspherevolumemigrations.cns.vmware.com -A
NAME                                   AGE
73216d3e-5c23-44e2-b700-10629b2f2753   4m43s
root@kube-master:~# kubectl describe cnsvspherevolumemigrations.cns.vmware.com
Name:         73216d3e-5c23-44e2-b700-10629b2f2753
Namespace:    
Labels:       <none>
Annotations:  <none>
API Version:  cns.vmware.com/v1alpha1
Kind:         CnsVSphereVolumeMigration
Metadata:
  Creation Timestamp:  2020-07-15T02:17:54Z
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
    Time:            2020-07-15T02:17:54Z
  Resource Version:  6582124
  Self Link:         /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/73216d3e-5c23-44e2-b700-10629b2f2753
  UID:               b24be2f6-21e3-4914-b161-a7e703a109b9
Spec:
  Volumeid:    73216d3e-5c23-44e2-b700-10629b2f2753
  Volumepath:  [vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-716f36d3-e574-4cb3-b4df-d703b0764f1c.vmdk
Events:        <none>
</pre>

Create a pod using the migrated volume:
<pre>
root@kube-master:~# kubectl create -f csi-yamls/pod.yaml 
pod/example-vanilla-block-pod created



2020-07-15T02:17:44.281Z	DEBUG	syncer/metadatasyncer.go:649	PodUpdated: Pod example-vanilla-block-pod calling updatePodMetadata	{"TraceId": "a6eb406a-16d6-4ff3-96a5-472e9b3fabd8"}
...
...
2020-07-15T02:18:00.157Z	INFO	volume/manager.go:531	UpdateVolumeMetadata: Volume metadata updated successfully. volumeID: "73216d3e-5c23-44e2-b700-10629b2f2753"

</pre>


CNS UI:
* Pod Name Updated
[CNS UI](https://github.com/chethanv28/CSI-migration-syncer-logs/blob/master/POD_Updated.png)

