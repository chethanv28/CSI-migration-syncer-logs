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


Full sync logs to show volumes getting registered from VMS
<pre>
2020-07-16T06:53:37.968Z	DEBUG	syncer/util.go:22	FullSync: Getting all PVs in Bound, Available or Released state	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
2020-07-16T06:53:37.968Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2226d604-94d7-4358-af43-495388018f1c.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
2020-07-16T06:53:37.986Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2226d604-94d7-4358-af43-495388018f1c.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2226d604-94d7-4358-af43-495388018f1c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
2020-07-16T06:53:37.986Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2226d604-94d7-4358-af43-495388018f1c.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0006d10a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "1353df2e-c731-11ea-8ac9-22f8ca2df1e3",
 VolumeType: (string) (len=5) "BLOCK",
 Datastores: ([]types.ManagedObjectReference) <nil>,
 Metadata: (types.CnsVolumeMetadata) {
  DynamicData: (types.DynamicData) {
  },
  ContainerCluster: (types.CnsContainerCluster) {
   DynamicData: (types.DynamicData) {
   },
   ClusterType: (string) (len=10) "KUBERNETES",
   ClusterId: (string) (len=15) "demo-cluster-id",
   VSphereUser: (string) (len=27) "Administrator@vsphere.local",
   ClusterFlavor: (string) (len=7) "VANILLA"
  },
  EntityMetadata: ([]types.BaseCnsEntityMetadata) <nil>,
  ContainerClusterArray: ([]types.CnsContainerCluster) (len=1 cap=1) {
   (types.CnsContainerCluster) {
    DynamicData: (types.DynamicData) {
    },
    ClusterType: (string) (len=10) "KUBERNETES",
    ClusterId: (string) (len=15) "demo-cluster-id",
    VSphereUser: (string) (len=27) "Administrator@vsphere.local",
    ClusterFlavor: (string) (len=7) "VANILLA"
   }
  }
 },
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000991a40)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2226d604-94d7-4358-af43-495388018f1c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
2020-07-16T06:53:38.008Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
2020-07-16T06:53:53.972Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "1353df2e-c731-11ea-8ac9-22f8ca2df1e3", opId: "91672864"	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
2020-07-16T06:53:53.972Z	DEBUG	volume/manager.go:244	Deleted task for 1353df2e-c731-11ea-8ac9-22f8ca2df1e3 from volumeTaskMap for statically provisioned volume	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
2020-07-16T06:53:53.972Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "1353df2e-c731-11ea-8ac9-22f8ca2df1e3", opId: "91672864", volumeID: "42618de1-3b93-4089-aefb-7cda81d6a774"	{"TraceId": "61c99ab1-c731-4c51-98cf-2bf02ea1fae5"}
</pre>

Full sync logs to show volume metadata updates getting invoked
<pre>
2020-07-16T06:54:04.014Z	DEBUG	syncer/fullsync.go:90	FullSync: pvToCnsEntityMetadataMap (map[string][]types.BaseCnsEntityMetadata) (len=2) {
 (string) (len=36) "de6738e2-9cd2-486f-9fbb-fcc290bb798b": ([]types.BaseCnsEntityMetadata) <nil>,
 (string) (len=36) "42618de1-3b93-4089-aefb-7cda81d6a774": ([]types.BaseCnsEntityMetadata) <nil>
}
 
 pvToK8sEntityMetadataMap: (map[string][]types.BaseCnsEntityMetadata) (len=2) {
 (string) (len=36) "42618de1-3b93-4089-aefb-7cda81d6a774": ([]types.BaseCnsEntityMetadata) (len=2 cap=2) {
  (*types.CnsKubernetesEntityMetadata)(0xc0006e2500)({
   CnsEntityMetadata: (types.CnsEntityMetadata) {
    DynamicData: (types.DynamicData) {
    },
    EntityName: (string) (len=40) "pvc-2226d604-94d7-4358-af43-495388018f1c",
    Labels: ([]types.KeyValue) <nil>,
    Delete: (bool) false,
    ClusterID: (string) (len=15) "demo-cluster-id"
   },
   EntityType: (string) (len=17) "PERSISTENT_VOLUME",
   Namespace: (string) "",
   ReferredEntity: ([]types.CnsKubernetesEntityReference) <nil>
  }),
  (*types.CnsKubernetesEntityMetadata)(0xc0006e2600)({
   CnsEntityMetadata: (types.CnsEntityMetadata) {
    DynamicData: (types.DynamicData) {
    },
    EntityName: (string) (len=16) "vcppvc-with-ds-2",
    Labels: ([]types.KeyValue) <nil>,
    Delete: (bool) false,
    ClusterID: (string) (len=15) "demo-cluster-id"
   },
   EntityType: (string) (len=23) "PERSISTENT_VOLUME_CLAIM",
   Namespace: (string) (len=7) "default",
   ReferredEntity: ([]types.CnsKubernetesEntityReference) (len=1 cap=1) {
    (types.CnsKubernetesEntityReference) {
     EntityType: (string) (len=17) "PERSISTENT_VOLUME",
     EntityName: (string) (len=40) "pvc-2226d604-94d7-4358-af43-495388018f1c",
     Namespace: (string) "",
     ClusterID: (string) (len=15) "demo-cluster-id"
    }
   }
  })
 },
 (string) (len=36) "de6738e2-9cd2-486f-9fbb-fcc290bb798b": ([]types.BaseCnsEntityMetadata) (len=2 cap=2) {
  (*types.CnsKubernetesEntityMetadata)(0xc0006e2780)({
   CnsEntityMetadata: (types.CnsEntityMetadata) {
    DynamicData: (types.DynamicData) {
    },
    EntityName: (string) (len=40) "pvc-6587c2cb-7625-4484-8404-c883ca3f90d0",
    Labels: ([]types.KeyValue) <nil>,
    Delete: (bool) false,
    ClusterID: (string) (len=15) "demo-cluster-id"
   },
   EntityType: (string) (len=17) "PERSISTENT_VOLUME",
   Namespace: (string) "",
   ReferredEntity: ([]types.CnsKubernetesEntityReference) <nil>
  }),
  (*types.CnsKubernetesEntityMetadata)(0xc0006e2880)({
   CnsEntityMetadata: (types.CnsEntityMetadata) {
    DynamicData: (types.DynamicData) {
    },
    EntityName: (string) (len=16) "vcppvc-with-ds-1",
    Labels: ([]types.KeyValue) <nil>,
    Delete: (bool) false,
    ClusterID: (string) (len=15) "demo-cluster-id"
   },
   EntityType: (string) (len=23) "PERSISTENT_VOLUME_CLAIM",
   Namespace: (string) (len=7) "default",
   ReferredEntity: ([]types.CnsKubernetesEntityReference) (len=1 cap=1) {
    (types.CnsKubernetesEntityReference) {
     EntityType: (string) (len=17) "PERSISTENT_VOLUME",
     EntityName: (string) (len=40) "pvc-6587c2cb-7625-4484-8404-c883ca3f90d0",
     Namespace: (string) "",
     ClusterID: (string) (len=15) "demo-cluster-id"
    }
   }
  })
 }
}
 

</pre>
Wait for Full Sync to be triggered and watch volume metadata getting updated in CNS UI:
[CNS UI](https://github.com/chethanv28/CSI-migration-syncer-logs/blob/master/Full-sync.png)
