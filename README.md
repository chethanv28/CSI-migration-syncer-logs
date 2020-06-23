# CSI-migration-syncer-logs
Syncer logs and CNS UI screenshots for CSI migration support
<pre>

root@kube-master:~# kubectl get pvc -A
NAMESPACE   NAME                        STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS               AGE
default     vcppvc                      Bound    pvc-3245eaaa-f56b-497b-95ac-0c9448eb678a   2Gi        RWO            vcpsc                      3d21h


kubectl label pv pvc-3245eaaa-f56b-497b-95ac-0c9448eb678a gold=yes

root@kube-master:~# kubectl describe pv pvc-3245eaaa-f56b-497b-95ac-0c9448eb678a
Name:            pvc-3245eaaa-f56b-497b-95ac-0c9448eb678a
Labels:          gold=yes
Annotations:     kubernetes.io/createdby: vsphere-volume-dynamic-provisioner
                 pv.kubernetes.io/bound-by-controller: yes
                 pv.kubernetes.io/migrated-to: csi.vsphere.vmware.com
                 pv.kubernetes.io/provisioned-by: kubernetes.io/vsphere-volume
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    vcpsc
Status:          Bound
Claim:           default/vcppvc
Reclaim Policy:  Delete
Access Modes:    RWO
VolumeMode:      Filesystem
Capacity:        2Gi
Node Affinity:   <none>
Message:         
Source:
    Type:               vSphereVolume (a Persistent Disk resource in vSphere)
    VolumePath:         [vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3245eaaa-f56b-497b-95ac-0c9448eb678a.vmdk
    FSType:             ext4
    StoragePolicyName:  vSAN Default Storage Policy
Events:                 <none>
  
  
   

2020-06-22T22:28:10.943Z	INFO	volume/manager.go:228	CreateVolume: Volume created successfully. VolumeName: "a148bc18-b4d7-11ea-9700-466f7e41fddd", opId: "3142e793", volumeID: "d797838e-0faa-46ed-bbf9-c6f9a853dff1"	{"TraceId": "09743ee1-4606-4638-a412-9397793f6eae"}



kubectl label pvc vcppvc silver=yes

2020-06-23T15:34:59.766Z	INFO	migration/migration.go:161	VolumeID: "d797838e-0faa-46ed-bbf9-c6f9a853dff1" found from the cache for VolumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3245eaaa-f56b-497b-95ac-0c9448eb678a.vmdk"	{"TraceId": "b2c8e106-f5d0-46fa-84b4-b482bd5cdf00"}
2020-06-23T15:34:59.767Z	INFO	syncer/metadatasyncer.go:580	PVCUpdated: volumeID: "d797838e-0faa-46ed-bbf9-c6f9a853dff1"	{"TraceId": "b2c8e106-f5d0-46fa-84b4-b482bd5cdf00"}
2020-06-23T15:34:59.767Z	DEBUG	syncer/metadatasyncer.go:602	PVCUpdated: Calling UpdateVolumeMetadata with updateSpec: (*types.CnsVolumeMetadataUpdateSpec)(0xc00092dd00)({
 DynamicData: (types.DynamicData) {
 },
 VolumeId: (types.CnsVolumeId) {
  DynamicData: (types.DynamicData) {
  },
  Id: (string) (len=36) "d797838e-0faa-46ed-bbf9-c6f9a853dff1"
 },
 Metadata: (types.CnsVolumeMetadata) {
  DynamicData: (types.DynamicData) {
  },
  ContainerCluster: (types.CnsContainerCluster) {
   DynamicData: (types.DynamicData) {
   },
   ClusterType: (string) (len=10) "KUBERNETES",
   ClusterId: (string) (len=15) "demo-cluster-id",
   VSphereUser: (string) (len=27) "Administrator@vsphere.local",
   ClusterFlavor: (string) ""
  },
  EntityMetadata: ([]types.BaseCnsEntityMetadata) (len=1 cap=1) {
   (*types.CnsKubernetesEntityMetadata)(0xc00092dc00)({
    CnsEntityMetadata: (types.CnsEntityMetadata) {
     DynamicData: (types.DynamicData) {
     },
     EntityName: (string) (len=6) "vcppvc",
     Labels: ([]types.KeyValue) (len=1 cap=1) {
      (types.KeyValue) {
       DynamicData: (types.DynamicData) {
       },
       Key: (string) (len=6) "silver",
       Value: (string) (len=3) "yes"
      }
     },
     Delete: (bool) false,
     ClusterID: (string) (len=15) "demo-cluster-id"
    },
    EntityType: (string) (len=23) "PERSISTENT_VOLUME_CLAIM",
    Namespace: (string) (len=7) "default",
    ReferredEntity: ([]types.CnsKubernetesEntityReference) (len=1 cap=1) {
     (types.CnsKubernetesEntityReference) {
      EntityType: (string) (len=17) "PERSISTENT_VOLUME",
      EntityName: (string) (len=40) "pvc-3245eaaa-f56b-497b-95ac-0c9448eb678a",
      Namespace: (string) "",
      ClusterID: (string) (len=15) "demo-cluster-id"
     }
    }
   })
  },
  ContainerClusterArray: ([]types.CnsContainerCluster) (len=1 cap=1) {
   (types.CnsContainerCluster) {
    DynamicData: (types.DynamicData) {
    },
    ClusterType: (string) (len=10) "KUBERNETES",
    ClusterId: (string) (len=15) "demo-cluster-id",
    VSphereUser: (string) (len=27) "Administrator@vsphere.local",
    ClusterFlavor: (string) ""
   }
  }
 }
})
	{"TraceId": "b2c8e106-f5d0-46fa-84b4-b482bd5cdf00"}
2020-06-23T15:34:59.799Z	DEBUG	volume/manager.go:470	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "b2c8e106-f5d0-46fa-84b4-b482bd5cdf00"}
2020-06-23T15:35:01.155Z	INFO	volume/manager.go:492	UpdateVolumeMetadata: volumeID: "d797838e-0faa-46ed-bbf9-c6f9a853dff1", opId: "6c1dc0f3"	{"TraceId": "b2c8e106-f5d0-46fa-84b4-b482bd5cdf00"}
2020-06-23T15:35:01.155Z	INFO	volume/manager.go:510	UpdateVolumeMetadata: Volume metadata updated successfully. volumeID: "d797838e-0faa-46ed-bbf9-c6f9a853dff1", opId: "6c1dc0f3"	{"TraceId": "b2c8e106-f5d0-46fa-84b4-b482bd5cdf00"}


</pre>
