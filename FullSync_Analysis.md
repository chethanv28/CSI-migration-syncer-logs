* Number of volumes created before migration:
500

* After enabling migration, wait until all volumes get "migrated-to" annotation:
<pre>
root@kube-master:~/csi-yamls# kubectl describe pvc -A | grep "pv.kubernetes.io/migrated-to" | wc -l
500
</pre>

* Deploy CSI driver

* Full sync triggered soon after csi driver starts

<pre>
2020-07-17T05:54:49.011Z	INFO	syncer/metadatasyncer.go:74	FullSync: fullSync interval is set to 30 minutes
2020-07-17T05:54:49.011Z	INFO	syncer/metadatasyncer.go:269	fullSync is triggered	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:54:49.011Z	INFO	syncer/fullsync.go:37	FullSync: start	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:54:49.011Z	DEBUG	syncer/util.go:22	FullSync: Getting all PVs in Bound, Available or Released state	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:54:49.015Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:54:49.016Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:54:49.017Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "06505112-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000665470)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:54:49.037Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.258Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "06505112-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c365"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.258Z	DEBUG	volume/manager.go:244	Deleted task for 06505112-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.258Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "06505112-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c365", volumeID: "030e44db-469d-4912-8bcf-85375b0cda89"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.258Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk" as container volume with ID: "030e44db-469d-4912-8bcf-85375b0cda89"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.258Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk" with CNS. VolumeID: "030e44db-469d-4912-8bcf-85375b0cda89"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.259Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {030e44db-469d-4912-8bcf-85375b0cda89      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk 030e44db-469d-4912-8bcf-85375b0cda89}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.259Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:030e44db-469d-4912-8bcf-85375b0cda89 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk VolumeID:030e44db-469d-4912-8bcf-85375b0cda89}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.281Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:55:05.284Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk", volumeID: "030e44db-469d-4912-8bcf-85375b0cda89" mapping in the cache
2020-07-17T05:55:05.300Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:030e44db-469d-4912-8bcf-85375b0cda89 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/030e44db-469d-4912-8bcf-85375b0cda89 UID:b8fc2d96-9a2b-4fd6-ad8f-4ad600863b7e ResourceVersion:7154223 Generation:1 CreationTimestamp:2020-07-17 05:55:05 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:55:05 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk VolumeID:030e44db-469d-4912-8bcf-85375b0cda89}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.300Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {030e44db-469d-4912-8bcf-85375b0cda89   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/030e44db-469d-4912-8bcf-85375b0cda89 b8fc2d96-9a2b-4fd6-ad8f-4ad600863b7e 7154223 1 2020-07-17 05:55:05 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:55:05 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78fa7454-0c63-4e8d-b8bb-7e2f8086bd10.vmdk 030e44db-469d-4912-8bcf-85375b0cda89}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.300Z	DEBUG	syncer/util.go:46	FullSync: pv 030e44db-469d-4912-8bcf-85375b0cda89 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.300Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.300Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.300Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004682a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "10051549-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000deede0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:05.327Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.056Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "10051549-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c368"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.057Z	DEBUG	volume/manager.go:244	Deleted task for 10051549-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.057Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "10051549-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c368", volumeID: "f5690cac-bef0-468e-8541-d9e608ca5b93"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.057Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk" as container volume with ID: "f5690cac-bef0-468e-8541-d9e608ca5b93"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.057Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk" with CNS. VolumeID: "f5690cac-bef0-468e-8541-d9e608ca5b93"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.057Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {f5690cac-bef0-468e-8541-d9e608ca5b93      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk f5690cac-bef0-468e-8541-d9e608ca5b93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.060Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f5690cac-bef0-468e-8541-d9e608ca5b93 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk VolumeID:f5690cac-bef0-468e-8541-d9e608ca5b93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.082Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:55:17.083Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk", volumeID: "f5690cac-bef0-468e-8541-d9e608ca5b93" mapping in the cache
2020-07-17T05:55:17.084Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f5690cac-bef0-468e-8541-d9e608ca5b93 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f5690cac-bef0-468e-8541-d9e608ca5b93 UID:79115eba-f9f3-4914-95e8-6300cbad91cf ResourceVersion:7154262 Generation:1 CreationTimestamp:2020-07-17 05:55:17 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:55:17 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk VolumeID:f5690cac-bef0-468e-8541-d9e608ca5b93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.084Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {f5690cac-bef0-468e-8541-d9e608ca5b93   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f5690cac-bef0-468e-8541-d9e608ca5b93 79115eba-f9f3-4914-95e8-6300cbad91cf 7154262 1 2020-07-17 05:55:17 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:55:17 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0880b350-5856-4e91-b2b9-d0348e1f4282.vmdk f5690cac-bef0-468e-8541-d9e608ca5b93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.084Z	DEBUG	syncer/util.go:46	FullSync: pv f5690cac-bef0-468e-8541-d9e608ca5b93 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.084Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.085Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.085Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "170b4a45-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dd3620)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:17.125Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.618Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "170b4a45-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c371"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.618Z	DEBUG	volume/manager.go:244	Deleted task for 170b4a45-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.618Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "170b4a45-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c371", volumeID: "fdbdb660-ef31-469c-8393-6e8ced53494c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.618Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk" as container volume with ID: "fdbdb660-ef31-469c-8393-6e8ced53494c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.618Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk" with CNS. VolumeID: "fdbdb660-ef31-469c-8393-6e8ced53494c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.618Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {fdbdb660-ef31-469c-8393-6e8ced53494c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk fdbdb660-ef31-469c-8393-6e8ced53494c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.618Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fdbdb660-ef31-469c-8393-6e8ced53494c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk VolumeID:fdbdb660-ef31-469c-8393-6e8ced53494c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.632Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fdbdb660-ef31-469c-8393-6e8ced53494c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fdbdb660-ef31-469c-8393-6e8ced53494c UID:ae8c29bc-a233-49e7-8c37-bd398ba4ae03 ResourceVersion:7154300 Generation:1 CreationTimestamp:2020-07-17 05:55:28 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:55:28 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk VolumeID:fdbdb660-ef31-469c-8393-6e8ced53494c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.633Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {fdbdb660-ef31-469c-8393-6e8ced53494c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fdbdb660-ef31-469c-8393-6e8ced53494c ae8c29bc-a233-49e7-8c37-bd398ba4ae03 7154300 1 2020-07-17 05:55:28 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:55:28 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk fdbdb660-ef31-469c-8393-6e8ced53494c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.634Z	DEBUG	syncer/util.go:46	FullSync: pv fdbdb660-ef31-469c-8393-6e8ced53494c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.635Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.636Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.636Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "1dedd636-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00069d890)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:28.635Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:55:28.642Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-048b6d12-7fb2-46f4-b876-a2ee718e30e9.vmdk", volumeID: "fdbdb660-ef31-469c-8393-6e8ced53494c" mapping in the cache
2020-07-17T05:55:28.656Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.555Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "1dedd636-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c374"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.555Z	DEBUG	volume/manager.go:244	Deleted task for 1dedd636-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.555Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "1dedd636-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c374", volumeID: "3cc10997-2baa-450e-830c-9530736ff965"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.555Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk" as container volume with ID: "3cc10997-2baa-450e-830c-9530736ff965"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.555Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk" with CNS. VolumeID: "3cc10997-2baa-450e-830c-9530736ff965"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.556Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {3cc10997-2baa-450e-830c-9530736ff965      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk 3cc10997-2baa-450e-830c-9530736ff965}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.557Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3cc10997-2baa-450e-830c-9530736ff965 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk VolumeID:3cc10997-2baa-450e-830c-9530736ff965}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.575Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:55:44.577Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk", volumeID: "3cc10997-2baa-450e-830c-9530736ff965" mapping in the cache
2020-07-17T05:55:44.579Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3cc10997-2baa-450e-830c-9530736ff965 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3cc10997-2baa-450e-830c-9530736ff965 UID:05780ee5-740c-4876-98dd-12db780be938 ResourceVersion:7154348 Generation:1 CreationTimestamp:2020-07-17 05:55:44 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:55:44 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk VolumeID:3cc10997-2baa-450e-830c-9530736ff965}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.583Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {3cc10997-2baa-450e-830c-9530736ff965   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3cc10997-2baa-450e-830c-9530736ff965 05780ee5-740c-4876-98dd-12db780be938 7154348 1 2020-07-17 05:55:44 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:55:44 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-59b2fade-9ca6-424d-98c0-2ae864a0016e.vmdk 3cc10997-2baa-450e-830c-9530736ff965}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.583Z	DEBUG	syncer/util.go:46	FullSync: pv 3cc10997-2baa-450e-830c-9530736ff965 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.583Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.584Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.584Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "276f4869-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000475e30)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:44.617Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.237Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "276f4869-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c390"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.237Z	DEBUG	volume/manager.go:244	Deleted task for 276f4869-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.237Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "276f4869-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c390", volumeID: "fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.237Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk" as container volume with ID: "fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.237Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk" with CNS. VolumeID: "fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.237Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.237Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk VolumeID:fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.245Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:55:54.246Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk", volumeID: "fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f" mapping in the cache
2020-07-17T05:55:54.249Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f UID:883290ed-d396-443f-a4d0-796ec84ca724 ResourceVersion:7154378 Generation:1 CreationTimestamp:2020-07-17 05:55:54 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:55:54 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk VolumeID:fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.250Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f 883290ed-d396-443f-a4d0-796ec84ca724 7154378 1 2020-07-17 05:55:54 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:55:54 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c7132a1-940e-49f3-8e2f-0d7b983b4021.vmdk fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.250Z	DEBUG	syncer/util.go:46	FullSync: pv fe01ee32-dcbb-4e42-a6f8-f27faf3eee9f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.251Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.257Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.257Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac62a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "2d327a39-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00019bdd0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:55:54.272Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.407Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "2d327a39-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c392"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.407Z	DEBUG	volume/manager.go:244	Deleted task for 2d327a39-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.407Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "2d327a39-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c392", volumeID: "18291ccc-8e77-4807-aef6-de1d8073bfed"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.407Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk" as container volume with ID: "18291ccc-8e77-4807-aef6-de1d8073bfed"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.407Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk" with CNS. VolumeID: "18291ccc-8e77-4807-aef6-de1d8073bfed"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.407Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {18291ccc-8e77-4807-aef6-de1d8073bfed      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk 18291ccc-8e77-4807-aef6-de1d8073bfed}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.407Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:18291ccc-8e77-4807-aef6-de1d8073bfed GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk VolumeID:18291ccc-8e77-4807-aef6-de1d8073bfed}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.417Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:56:09.419Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:18291ccc-8e77-4807-aef6-de1d8073bfed GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/18291ccc-8e77-4807-aef6-de1d8073bfed UID:a503c3c5-5de8-4ae9-8738-3afd2b31e2f3 ResourceVersion:7154427 Generation:1 CreationTimestamp:2020-07-17 05:56:09 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:56:09 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk VolumeID:18291ccc-8e77-4807-aef6-de1d8073bfed}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.419Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {18291ccc-8e77-4807-aef6-de1d8073bfed   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/18291ccc-8e77-4807-aef6-de1d8073bfed a503c3c5-5de8-4ae9-8738-3afd2b31e2f3 7154427 1 2020-07-17 05:56:09 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:56:09 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk 18291ccc-8e77-4807-aef6-de1d8073bfed}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.419Z	DEBUG	syncer/util.go:46	FullSync: pv 18291ccc-8e77-4807-aef6-de1d8073bfed is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.419Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-77d9649b-3ebd-4e86-b467-65bbf2fb93bc.vmdk", volumeID: "18291ccc-8e77-4807-aef6-de1d8073bfed" mapping in the cache
2020-07-17T05:56:09.419Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.419Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.420Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468700)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "363cf06a-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005709c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:09.436Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.747Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "363cf06a-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c395"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.747Z	DEBUG	volume/manager.go:244	Deleted task for 363cf06a-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.747Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "363cf06a-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c395", volumeID: "8ac56613-5934-45ea-9b20-05b8ce640af8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.747Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk" as container volume with ID: "8ac56613-5934-45ea-9b20-05b8ce640af8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.747Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk" with CNS. VolumeID: "8ac56613-5934-45ea-9b20-05b8ce640af8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.747Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {8ac56613-5934-45ea-9b20-05b8ce640af8      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk 8ac56613-5934-45ea-9b20-05b8ce640af8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.747Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:8ac56613-5934-45ea-9b20-05b8ce640af8 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk VolumeID:8ac56613-5934-45ea-9b20-05b8ce640af8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.763Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:56:21.765Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk", volumeID: "8ac56613-5934-45ea-9b20-05b8ce640af8" mapping in the cache
2020-07-17T05:56:21.773Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:8ac56613-5934-45ea-9b20-05b8ce640af8 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/8ac56613-5934-45ea-9b20-05b8ce640af8 UID:ea62be6e-72bf-43ce-a19a-0c07fe91bb57 ResourceVersion:7154467 Generation:1 CreationTimestamp:2020-07-17 05:56:21 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:56:21 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk VolumeID:8ac56613-5934-45ea-9b20-05b8ce640af8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.776Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {8ac56613-5934-45ea-9b20-05b8ce640af8   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/8ac56613-5934-45ea-9b20-05b8ce640af8 ea62be6e-72bf-43ce-a19a-0c07fe91bb57 7154467 1 2020-07-17 05:56:21 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:56:21 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b8963103-40d2-4d77-8bf0-cdb729b85955.vmdk 8ac56613-5934-45ea-9b20-05b8ce640af8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.776Z	DEBUG	syncer/util.go:46	FullSync: pv 8ac56613-5934-45ea-9b20-05b8ce640af8 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.776Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.777Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.778Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004687e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "3d9a77f0-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002e30b0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:21.804Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.094Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "3d9a77f0-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3ad"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.094Z	DEBUG	volume/manager.go:244	Deleted task for 3d9a77f0-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.094Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "3d9a77f0-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3ad", volumeID: "d965e02f-ba76-4d79-8274-1be3b7a5553f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.094Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk" as container volume with ID: "d965e02f-ba76-4d79-8274-1be3b7a5553f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.094Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk" with CNS. VolumeID: "d965e02f-ba76-4d79-8274-1be3b7a5553f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.095Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {d965e02f-ba76-4d79-8274-1be3b7a5553f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk d965e02f-ba76-4d79-8274-1be3b7a5553f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.095Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d965e02f-ba76-4d79-8274-1be3b7a5553f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk VolumeID:d965e02f-ba76-4d79-8274-1be3b7a5553f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.114Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:56:36.114Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk", volumeID: "d965e02f-ba76-4d79-8274-1be3b7a5553f" mapping in the cache
2020-07-17T05:56:36.119Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d965e02f-ba76-4d79-8274-1be3b7a5553f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d965e02f-ba76-4d79-8274-1be3b7a5553f UID:deae0836-15cb-4dd1-91a8-21371d8dbbec ResourceVersion:7154510 Generation:1 CreationTimestamp:2020-07-17 05:56:36 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:56:36 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk VolumeID:d965e02f-ba76-4d79-8274-1be3b7a5553f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.119Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {d965e02f-ba76-4d79-8274-1be3b7a5553f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d965e02f-ba76-4d79-8274-1be3b7a5553f deae0836-15cb-4dd1-91a8-21371d8dbbec 7154510 1 2020-07-17 05:56:36 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:56:36 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-654cb13f-9675-4940-b8b1-f4ca29b06ba5.vmdk d965e02f-ba76-4d79-8274-1be3b7a5553f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.119Z	DEBUG	syncer/util.go:46	FullSync: pv d965e02f-ba76-4d79-8274-1be3b7a5553f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.119Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.119Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.124Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "4627012c-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b0000)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:36.149Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.395Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "4627012c-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3b0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.395Z	DEBUG	volume/manager.go:244	Deleted task for 4627012c-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.395Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "4627012c-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3b0", volumeID: "be1ff498-9b0b-4a66-ad56-ab602af1dd09"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.395Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk" as container volume with ID: "be1ff498-9b0b-4a66-ad56-ab602af1dd09"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.396Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk" with CNS. VolumeID: "be1ff498-9b0b-4a66-ad56-ab602af1dd09"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.396Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {be1ff498-9b0b-4a66-ad56-ab602af1dd09      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk be1ff498-9b0b-4a66-ad56-ab602af1dd09}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.396Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:be1ff498-9b0b-4a66-ad56-ab602af1dd09 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk VolumeID:be1ff498-9b0b-4a66-ad56-ab602af1dd09}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.445Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:56:47.446Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk", volumeID: "be1ff498-9b0b-4a66-ad56-ab602af1dd09" mapping in the cache
2020-07-17T05:56:47.455Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:be1ff498-9b0b-4a66-ad56-ab602af1dd09 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/be1ff498-9b0b-4a66-ad56-ab602af1dd09 UID:92f945d7-1474-4271-be75-c5a75516da89 ResourceVersion:7154546 Generation:1 CreationTimestamp:2020-07-17 05:56:47 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:56:47 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk VolumeID:be1ff498-9b0b-4a66-ad56-ab602af1dd09}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.455Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {be1ff498-9b0b-4a66-ad56-ab602af1dd09   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/be1ff498-9b0b-4a66-ad56-ab602af1dd09 92f945d7-1474-4271-be75-c5a75516da89 7154546 1 2020-07-17 05:56:47 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:56:47 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-46b397ca-c469-4c52-ba8b-ce658629247c.vmdk be1ff498-9b0b-4a66-ad56-ab602af1dd09}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.456Z	DEBUG	syncer/util.go:46	FullSync: pv be1ff498-9b0b-4a66-ad56-ab602af1dd09 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.456Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.457Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.462Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6700)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "4ce8f7ef-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005e9500)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:56:47.477Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.026Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "4ce8f7ef-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3cb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.026Z	DEBUG	volume/manager.go:244	Deleted task for 4ce8f7ef-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.026Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "4ce8f7ef-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3cb", volumeID: "0a951fd5-9ffc-4778-b4eb-14cb838a710c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.026Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk" as container volume with ID: "0a951fd5-9ffc-4778-b4eb-14cb838a710c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.026Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk" with CNS. VolumeID: "0a951fd5-9ffc-4778-b4eb-14cb838a710c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.026Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {0a951fd5-9ffc-4778-b4eb-14cb838a710c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk 0a951fd5-9ffc-4778-b4eb-14cb838a710c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.026Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:0a951fd5-9ffc-4778-b4eb-14cb838a710c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk VolumeID:0a951fd5-9ffc-4778-b4eb-14cb838a710c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.035Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:57:05.036Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk", volumeID: "0a951fd5-9ffc-4778-b4eb-14cb838a710c" mapping in the cache
2020-07-17T05:57:05.039Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:0a951fd5-9ffc-4778-b4eb-14cb838a710c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/0a951fd5-9ffc-4778-b4eb-14cb838a710c UID:26da8774-e6a0-47c4-9c83-1b92fcc8ac8b ResourceVersion:7154600 Generation:1 CreationTimestamp:2020-07-17 05:57:05 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:57:05 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk VolumeID:0a951fd5-9ffc-4778-b4eb-14cb838a710c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.039Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {0a951fd5-9ffc-4778-b4eb-14cb838a710c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/0a951fd5-9ffc-4778-b4eb-14cb838a710c 26da8774-e6a0-47c4-9c83-1b92fcc8ac8b 7154600 1 2020-07-17 05:57:05 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:57:05 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-457388ee-49a4-44ac-996c-79a6364c030c.vmdk 0a951fd5-9ffc-4778-b4eb-14cb838a710c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.039Z	DEBUG	syncer/util.go:46	FullSync: pv 0a951fd5-9ffc-4778-b4eb-14cb838a710c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.039Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.039Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.039Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "5763d046-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00069dd10)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:05.060Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.792Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "5763d046-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3ce"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.792Z	DEBUG	volume/manager.go:244	Deleted task for 5763d046-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.792Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "5763d046-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3ce", volumeID: "0f5eca2d-96b6-4a5b-b06e-db82c53b36df"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.792Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk" as container volume with ID: "0f5eca2d-96b6-4a5b-b06e-db82c53b36df"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.792Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk" with CNS. VolumeID: "0f5eca2d-96b6-4a5b-b06e-db82c53b36df"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.802Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {0f5eca2d-96b6-4a5b-b06e-db82c53b36df      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk 0f5eca2d-96b6-4a5b-b06e-db82c53b36df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.803Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:0f5eca2d-96b6-4a5b-b06e-db82c53b36df GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk VolumeID:0f5eca2d-96b6-4a5b-b06e-db82c53b36df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.818Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:57:18.823Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk", volumeID: "0f5eca2d-96b6-4a5b-b06e-db82c53b36df" mapping in the cache
2020-07-17T05:57:18.823Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:0f5eca2d-96b6-4a5b-b06e-db82c53b36df GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/0f5eca2d-96b6-4a5b-b06e-db82c53b36df UID:ee967f54-f789-4eac-ab9b-bbb25d3a6092 ResourceVersion:7154644 Generation:1 CreationTimestamp:2020-07-17 05:57:18 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:57:18 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk VolumeID:0f5eca2d-96b6-4a5b-b06e-db82c53b36df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.823Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {0f5eca2d-96b6-4a5b-b06e-db82c53b36df   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/0f5eca2d-96b6-4a5b-b06e-db82c53b36df ee967f54-f789-4eac-ab9b-bbb25d3a6092 7154644 1 2020-07-17 05:57:18 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:57:18 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6dad526-23fe-47b5-8051-9d381861ae85.vmdk 0f5eca2d-96b6-4a5b-b06e-db82c53b36df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.823Z	DEBUG	syncer/util.go:46	FullSync: pv 0f5eca2d-96b6-4a5b-b06e-db82c53b36df is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.823Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.825Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.825Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac61c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "5f9b202f-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00059dd40)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:18.846Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.393Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "5f9b202f-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3d1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.397Z	DEBUG	volume/manager.go:244	Deleted task for 5f9b202f-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.398Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "5f9b202f-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3d1", volumeID: "1d876a54-d82a-47f6-b33e-61126302d525"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.399Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk" as container volume with ID: "1d876a54-d82a-47f6-b33e-61126302d525"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.400Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk" with CNS. VolumeID: "1d876a54-d82a-47f6-b33e-61126302d525"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.400Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {1d876a54-d82a-47f6-b33e-61126302d525      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk 1d876a54-d82a-47f6-b33e-61126302d525}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.400Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:1d876a54-d82a-47f6-b33e-61126302d525 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk VolumeID:1d876a54-d82a-47f6-b33e-61126302d525}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.415Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:57:31.415Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk", volumeID: "1d876a54-d82a-47f6-b33e-61126302d525" mapping in the cache
2020-07-17T05:57:31.416Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:1d876a54-d82a-47f6-b33e-61126302d525 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/1d876a54-d82a-47f6-b33e-61126302d525 UID:745bf814-d0fd-41a5-b207-c2ab22163a68 ResourceVersion:7154685 Generation:1 CreationTimestamp:2020-07-17 05:57:31 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:57:31 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk VolumeID:1d876a54-d82a-47f6-b33e-61126302d525}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.416Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {1d876a54-d82a-47f6-b33e-61126302d525   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/1d876a54-d82a-47f6-b33e-61126302d525 745bf814-d0fd-41a5-b207-c2ab22163a68 7154685 1 2020-07-17 05:57:31 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:57:31 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f152a318-991e-452f-9659-84ac77b95d6d.vmdk 1d876a54-d82a-47f6-b33e-61126302d525}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.416Z	DEBUG	syncer/util.go:46	FullSync: pv 1d876a54-d82a-47f6-b33e-61126302d525 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.416Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.416Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.416Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac62a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "671ca295-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0003fc810)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:31.433Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.175Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "671ca295-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3d4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.176Z	DEBUG	volume/manager.go:244	Deleted task for 671ca295-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.176Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "671ca295-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3d4", volumeID: "76facb42-dcb0-4ae1-bff9-fa299f66e4cf"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.176Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk" as container volume with ID: "76facb42-dcb0-4ae1-bff9-fa299f66e4cf"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.176Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk" with CNS. VolumeID: "76facb42-dcb0-4ae1-bff9-fa299f66e4cf"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.176Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {76facb42-dcb0-4ae1-bff9-fa299f66e4cf      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk 76facb42-dcb0-4ae1-bff9-fa299f66e4cf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.183Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:76facb42-dcb0-4ae1-bff9-fa299f66e4cf GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk VolumeID:76facb42-dcb0-4ae1-bff9-fa299f66e4cf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.201Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:57:46.201Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk", volumeID: "76facb42-dcb0-4ae1-bff9-fa299f66e4cf" mapping in the cache
2020-07-17T05:57:46.202Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:76facb42-dcb0-4ae1-bff9-fa299f66e4cf GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/76facb42-dcb0-4ae1-bff9-fa299f66e4cf UID:daddd2ca-8abd-4244-81ef-36c6d85d2b1c ResourceVersion:7154729 Generation:1 CreationTimestamp:2020-07-17 05:57:46 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:57:46 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk VolumeID:76facb42-dcb0-4ae1-bff9-fa299f66e4cf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.202Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {76facb42-dcb0-4ae1-bff9-fa299f66e4cf   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/76facb42-dcb0-4ae1-bff9-fa299f66e4cf daddd2ca-8abd-4244-81ef-36c6d85d2b1c 7154729 1 2020-07-17 05:57:46 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:57:46 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6a334f7c-6f99-4b71-8316-f93e0dab913f.vmdk 76facb42-dcb0-4ae1-bff9-fa299f66e4cf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.203Z	DEBUG	syncer/util.go:46	FullSync: pv 76facb42-dcb0-4ae1-bff9-fa299f66e4cf is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.203Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.203Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.203Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "6fece545-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007a2ea0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:57:46.218Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.201Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "6fece545-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3ea"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.201Z	DEBUG	volume/manager.go:244	Deleted task for 6fece545-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.201Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "6fece545-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3ea", volumeID: "2d1b1271-cc9c-4c78-9e35-94cfa546f4a0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.201Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk" as container volume with ID: "2d1b1271-cc9c-4c78-9e35-94cfa546f4a0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.201Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk" with CNS. VolumeID: "2d1b1271-cc9c-4c78-9e35-94cfa546f4a0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.201Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {2d1b1271-cc9c-4c78-9e35-94cfa546f4a0      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk 2d1b1271-cc9c-4c78-9e35-94cfa546f4a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.201Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2d1b1271-cc9c-4c78-9e35-94cfa546f4a0 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk VolumeID:2d1b1271-cc9c-4c78-9e35-94cfa546f4a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.215Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:58:00.217Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk", volumeID: "2d1b1271-cc9c-4c78-9e35-94cfa546f4a0" mapping in the cache
2020-07-17T05:58:00.223Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2d1b1271-cc9c-4c78-9e35-94cfa546f4a0 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2d1b1271-cc9c-4c78-9e35-94cfa546f4a0 UID:1db445ea-cdab-44ed-a408-bb56c1e7654d ResourceVersion:7154775 Generation:1 CreationTimestamp:2020-07-17 05:58:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:58:00 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk VolumeID:2d1b1271-cc9c-4c78-9e35-94cfa546f4a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.223Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {2d1b1271-cc9c-4c78-9e35-94cfa546f4a0   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2d1b1271-cc9c-4c78-9e35-94cfa546f4a0 1db445ea-cdab-44ed-a408-bb56c1e7654d 7154775 1 2020-07-17 05:58:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:58:00 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9304370-775a-4904-b70e-1f1f15f9df4a.vmdk 2d1b1271-cc9c-4c78-9e35-94cfa546f4a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.224Z	DEBUG	syncer/util.go:46	FullSync: pv 2d1b1271-cc9c-4c78-9e35-94cfa546f4a0 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.224Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.224Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.224Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004682a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "78485714-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004476b0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:00.240Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.942Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "78485714-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3f6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.942Z	DEBUG	volume/manager.go:244	Deleted task for 78485714-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.942Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "78485714-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c3f6", volumeID: "e52ab7f1-0046-4297-91f9-c5baa4c356d6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.942Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk" as container volume with ID: "e52ab7f1-0046-4297-91f9-c5baa4c356d6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.942Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk" with CNS. VolumeID: "e52ab7f1-0046-4297-91f9-c5baa4c356d6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.942Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {e52ab7f1-0046-4297-91f9-c5baa4c356d6      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk e52ab7f1-0046-4297-91f9-c5baa4c356d6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.942Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e52ab7f1-0046-4297-91f9-c5baa4c356d6 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk VolumeID:e52ab7f1-0046-4297-91f9-c5baa4c356d6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.955Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e52ab7f1-0046-4297-91f9-c5baa4c356d6 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e52ab7f1-0046-4297-91f9-c5baa4c356d6 UID:ff379fe6-2553-4fd6-abe4-e8df439a5005 ResourceVersion:7154822 Generation:1 CreationTimestamp:2020-07-17 05:58:15 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:58:15 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk VolumeID:e52ab7f1-0046-4297-91f9-c5baa4c356d6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.958Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:58:15.959Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk", volumeID: "e52ab7f1-0046-4297-91f9-c5baa4c356d6" mapping in the cache
2020-07-17T05:58:15.959Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {e52ab7f1-0046-4297-91f9-c5baa4c356d6   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e52ab7f1-0046-4297-91f9-c5baa4c356d6 ff379fe6-2553-4fd6-abe4-e8df439a5005 7154822 1 2020-07-17 05:58:15 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:58:15 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5606f1a0-e736-4274-8bb2-d9a95d166d54.vmdk e52ab7f1-0046-4297-91f9-c5baa4c356d6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.959Z	DEBUG	syncer/util.go:46	FullSync: pv e52ab7f1-0046-4297-91f9-c5baa4c356d6 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.959Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.960Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.961Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "81a9690f-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b12f0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:15.980Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.777Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "81a9690f-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c40e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.777Z	DEBUG	volume/manager.go:244	Deleted task for 81a9690f-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.777Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "81a9690f-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c40e", volumeID: "c87af12e-a291-4dcb-96e9-7d05ee39319f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.777Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk" as container volume with ID: "c87af12e-a291-4dcb-96e9-7d05ee39319f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.777Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk" with CNS. VolumeID: "c87af12e-a291-4dcb-96e9-7d05ee39319f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.777Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {c87af12e-a291-4dcb-96e9-7d05ee39319f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk c87af12e-a291-4dcb-96e9-7d05ee39319f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.777Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c87af12e-a291-4dcb-96e9-7d05ee39319f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk VolumeID:c87af12e-a291-4dcb-96e9-7d05ee39319f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.790Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:58:26.794Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk", volumeID: "c87af12e-a291-4dcb-96e9-7d05ee39319f" mapping in the cache
2020-07-17T05:58:26.800Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c87af12e-a291-4dcb-96e9-7d05ee39319f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c87af12e-a291-4dcb-96e9-7d05ee39319f UID:e572b312-d9fe-485f-a8ce-d42fe867c62d ResourceVersion:7154857 Generation:1 CreationTimestamp:2020-07-17 05:58:26 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:58:26 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk VolumeID:c87af12e-a291-4dcb-96e9-7d05ee39319f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.800Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {c87af12e-a291-4dcb-96e9-7d05ee39319f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c87af12e-a291-4dcb-96e9-7d05ee39319f e572b312-d9fe-485f-a8ce-d42fe867c62d 7154857 1 2020-07-17 05:58:26 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:58:26 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ec3252ed-86e8-4e31-bc1e-cb157faa6235.vmdk c87af12e-a291-4dcb-96e9-7d05ee39319f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.804Z	DEBUG	syncer/util.go:46	FullSync: pv c87af12e-a291-4dcb-96e9-7d05ee39319f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.804Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.805Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.805Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "8820269b-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004df8c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:26.822Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.137Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "8820269b-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c410"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.137Z	DEBUG	volume/manager.go:244	Deleted task for 8820269b-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.137Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "8820269b-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c410", volumeID: "229aa39a-31a1-429a-84d0-12bdde1633fb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.137Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk" as container volume with ID: "229aa39a-31a1-429a-84d0-12bdde1633fb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.137Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk" with CNS. VolumeID: "229aa39a-31a1-429a-84d0-12bdde1633fb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.137Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {229aa39a-31a1-429a-84d0-12bdde1633fb      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk 229aa39a-31a1-429a-84d0-12bdde1633fb}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.137Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:229aa39a-31a1-429a-84d0-12bdde1633fb GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk VolumeID:229aa39a-31a1-429a-84d0-12bdde1633fb}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.144Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:58:41.144Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk", volumeID: "229aa39a-31a1-429a-84d0-12bdde1633fb" mapping in the cache
2020-07-17T05:58:41.148Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:229aa39a-31a1-429a-84d0-12bdde1633fb GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/229aa39a-31a1-429a-84d0-12bdde1633fb UID:813e1baf-4993-4d83-9b4a-9c52b71c1292 ResourceVersion:7154903 Generation:1 CreationTimestamp:2020-07-17 05:58:41 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:58:41 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk VolumeID:229aa39a-31a1-429a-84d0-12bdde1633fb}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.149Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {229aa39a-31a1-429a-84d0-12bdde1633fb   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/229aa39a-31a1-429a-84d0-12bdde1633fb 813e1baf-4993-4d83-9b4a-9c52b71c1292 7154903 1 2020-07-17 05:58:41 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:58:41 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-fb38ac54-7e20-41b0-bb7f-4797565ed6bc.vmdk 229aa39a-31a1-429a-84d0-12bdde1633fb}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.149Z	DEBUG	syncer/util.go:46	FullSync: pv 229aa39a-31a1-429a-84d0-12bdde1633fb is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.149Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.149Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.150Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac68c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "90acfd0e-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dd3800)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:41.165Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.018Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "90acfd0e-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c416"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.018Z	DEBUG	volume/manager.go:244	Deleted task for 90acfd0e-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.018Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "90acfd0e-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c416", volumeID: "177f8672-1627-44a6-9b95-920b1551153e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.018Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk" as container volume with ID: "177f8672-1627-44a6-9b95-920b1551153e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.020Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk" with CNS. VolumeID: "177f8672-1627-44a6-9b95-920b1551153e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.021Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {177f8672-1627-44a6-9b95-920b1551153e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk 177f8672-1627-44a6-9b95-920b1551153e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.021Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:177f8672-1627-44a6-9b95-920b1551153e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk VolumeID:177f8672-1627-44a6-9b95-920b1551153e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.036Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:58:55.036Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk", volumeID: "177f8672-1627-44a6-9b95-920b1551153e" mapping in the cache
2020-07-17T05:58:55.037Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:177f8672-1627-44a6-9b95-920b1551153e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/177f8672-1627-44a6-9b95-920b1551153e UID:93e08a89-c5fc-4293-876a-d67b1b3cefe1 ResourceVersion:7154947 Generation:1 CreationTimestamp:2020-07-17 05:58:55 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:58:55 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk VolumeID:177f8672-1627-44a6-9b95-920b1551153e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.037Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {177f8672-1627-44a6-9b95-920b1551153e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/177f8672-1627-44a6-9b95-920b1551153e 93e08a89-c5fc-4293-876a-d67b1b3cefe1 7154947 1 2020-07-17 05:58:55 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:58:55 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-df8c4d23-504f-488c-a78a-a05d6adab133.vmdk 177f8672-1627-44a6-9b95-920b1551153e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.037Z	DEBUG	syncer/util.go:46	FullSync: pv 177f8672-1627-44a6-9b95-920b1551153e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.037Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.038Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.038Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "98f4419b-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dd2cc0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:58:55.057Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.097Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "98f4419b-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c438"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.098Z	DEBUG	volume/manager.go:244	Deleted task for 98f4419b-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.098Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "98f4419b-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c438", volumeID: "b0474346-d037-44c0-91ab-fe0300de1357"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.098Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk" as container volume with ID: "b0474346-d037-44c0-91ab-fe0300de1357"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.098Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk" with CNS. VolumeID: "b0474346-d037-44c0-91ab-fe0300de1357"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.098Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {b0474346-d037-44c0-91ab-fe0300de1357      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk b0474346-d037-44c0-91ab-fe0300de1357}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.098Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b0474346-d037-44c0-91ab-fe0300de1357 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk VolumeID:b0474346-d037-44c0-91ab-fe0300de1357}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.115Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:59:08.116Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk", volumeID: "b0474346-d037-44c0-91ab-fe0300de1357" mapping in the cache
2020-07-17T05:59:08.117Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b0474346-d037-44c0-91ab-fe0300de1357 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b0474346-d037-44c0-91ab-fe0300de1357 UID:c0f70d09-7474-4c48-b6dd-777a534b6d61 ResourceVersion:7154989 Generation:1 CreationTimestamp:2020-07-17 05:59:08 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:59:08 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk VolumeID:b0474346-d037-44c0-91ab-fe0300de1357}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.117Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {b0474346-d037-44c0-91ab-fe0300de1357   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b0474346-d037-44c0-91ab-fe0300de1357 c0f70d09-7474-4c48-b6dd-777a534b6d61 7154989 1 2020-07-17 05:59:08 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:59:08 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d8e66975-e105-4166-a387-72286f2777f6.vmdk b0474346-d037-44c0-91ab-fe0300de1357}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.117Z	DEBUG	syncer/util.go:46	FullSync: pv b0474346-d037-44c0-91ab-fe0300de1357 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.117Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.117Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.118Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac61c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "a0c01456-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004de1b0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:08.143Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.921Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "a0c01456-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c43a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.921Z	DEBUG	volume/manager.go:244	Deleted task for a0c01456-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.921Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "a0c01456-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c43a", volumeID: "dab97282-06d6-4030-be13-abc79f23ba93"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.921Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk" as container volume with ID: "dab97282-06d6-4030-be13-abc79f23ba93"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.921Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk" with CNS. VolumeID: "dab97282-06d6-4030-be13-abc79f23ba93"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.923Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {dab97282-06d6-4030-be13-abc79f23ba93      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk dab97282-06d6-4030-be13-abc79f23ba93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.925Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dab97282-06d6-4030-be13-abc79f23ba93 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk VolumeID:dab97282-06d6-4030-be13-abc79f23ba93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.946Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:59:18.949Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dab97282-06d6-4030-be13-abc79f23ba93 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dab97282-06d6-4030-be13-abc79f23ba93 UID:8058fca0-67f9-4bb0-996d-2e051f0d48c0 ResourceVersion:7155022 Generation:1 CreationTimestamp:2020-07-17 05:59:18 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:59:18 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk VolumeID:dab97282-06d6-4030-be13-abc79f23ba93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.950Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {dab97282-06d6-4030-be13-abc79f23ba93   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dab97282-06d6-4030-be13-abc79f23ba93 8058fca0-67f9-4bb0-996d-2e051f0d48c0 7155022 1 2020-07-17 05:59:18 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:59:18 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk dab97282-06d6-4030-be13-abc79f23ba93}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.950Z	DEBUG	syncer/util.go:46	FullSync: pv dab97282-06d6-4030-be13-abc79f23ba93 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.951Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.946Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2f32df5d-6b14-4814-95af-d3e3177637ed.vmdk", volumeID: "dab97282-06d6-4030-be13-abc79f23ba93" mapping in the cache
2020-07-17T05:59:18.953Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.954Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "a73589e7-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004785a0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:18.972Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.400Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "a73589e7-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c43d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.400Z	DEBUG	volume/manager.go:244	Deleted task for a73589e7-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.401Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "a73589e7-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c43d", volumeID: "db519d6e-41f7-44b7-8816-d0c04a257f47"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.401Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk" as container volume with ID: "db519d6e-41f7-44b7-8816-d0c04a257f47"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.403Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk" with CNS. VolumeID: "db519d6e-41f7-44b7-8816-d0c04a257f47"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.404Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {db519d6e-41f7-44b7-8816-d0c04a257f47      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk db519d6e-41f7-44b7-8816-d0c04a257f47}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.405Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:db519d6e-41f7-44b7-8816-d0c04a257f47 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk VolumeID:db519d6e-41f7-44b7-8816-d0c04a257f47}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.427Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:59:29.427Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk", volumeID: "db519d6e-41f7-44b7-8816-d0c04a257f47" mapping in the cache
2020-07-17T05:59:29.430Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:db519d6e-41f7-44b7-8816-d0c04a257f47 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/db519d6e-41f7-44b7-8816-d0c04a257f47 UID:29d6ade4-450a-4c45-90d3-4ff1acee7662 ResourceVersion:7155055 Generation:1 CreationTimestamp:2020-07-17 05:59:29 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:59:29 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk VolumeID:db519d6e-41f7-44b7-8816-d0c04a257f47}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.431Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {db519d6e-41f7-44b7-8816-d0c04a257f47   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/db519d6e-41f7-44b7-8816-d0c04a257f47 29d6ade4-450a-4c45-90d3-4ff1acee7662 7155055 1 2020-07-17 05:59:29 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:59:29 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8f05cda3-9d4f-4ba8-aad2-8b81f03c697f.vmdk db519d6e-41f7-44b7-8816-d0c04a257f47}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.431Z	DEBUG	syncer/util.go:46	FullSync: pv db519d6e-41f7-44b7-8816-d0c04a257f47 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.431Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.431Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.432Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "ad7441af-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002e3170)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:29.451Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.432Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "ad7441af-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c43f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.432Z	DEBUG	volume/manager.go:244	Deleted task for ad7441af-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.432Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "ad7441af-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c43f", volumeID: "9bc02f0d-0106-4421-8ae9-aa179b2afc3e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.434Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk" as container volume with ID: "9bc02f0d-0106-4421-8ae9-aa179b2afc3e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.434Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk" with CNS. VolumeID: "9bc02f0d-0106-4421-8ae9-aa179b2afc3e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.435Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {9bc02f0d-0106-4421-8ae9-aa179b2afc3e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk 9bc02f0d-0106-4421-8ae9-aa179b2afc3e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.435Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:9bc02f0d-0106-4421-8ae9-aa179b2afc3e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk VolumeID:9bc02f0d-0106-4421-8ae9-aa179b2afc3e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.448Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:9bc02f0d-0106-4421-8ae9-aa179b2afc3e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/9bc02f0d-0106-4421-8ae9-aa179b2afc3e UID:0f4e5f7e-0aa6-4bb8-84c1-5136e6274c2c ResourceVersion:7155106 Generation:1 CreationTimestamp:2020-07-17 05:59:44 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:59:44 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk VolumeID:9bc02f0d-0106-4421-8ae9-aa179b2afc3e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.448Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {9bc02f0d-0106-4421-8ae9-aa179b2afc3e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/9bc02f0d-0106-4421-8ae9-aa179b2afc3e 0f4e5f7e-0aa6-4bb8-84c1-5136e6274c2c 7155106 1 2020-07-17 05:59:44 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:59:44 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk 9bc02f0d-0106-4421-8ae9-aa179b2afc3e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.449Z	DEBUG	syncer/util.go:46	FullSync: pv 9bc02f0d-0106-4421-8ae9-aa179b2afc3e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.449Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.451Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.451Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "b66834c7-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005718f0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:44.452Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:59:44.452Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baecdda3-3eef-43d4-830e-056745eee68e.vmdk", volumeID: "9bc02f0d-0106-4421-8ae9-aa179b2afc3e" mapping in the cache
2020-07-17T05:59:44.471Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.169Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "b66834c7-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c45f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.169Z	DEBUG	volume/manager.go:244	Deleted task for b66834c7-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.169Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "b66834c7-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c45f", volumeID: "dac10677-2fea-4d6b-904d-5ab90c1d71f1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.169Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk" as container volume with ID: "dac10677-2fea-4d6b-904d-5ab90c1d71f1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.170Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk" with CNS. VolumeID: "dac10677-2fea-4d6b-904d-5ab90c1d71f1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.170Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {dac10677-2fea-4d6b-904d-5ab90c1d71f1      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk dac10677-2fea-4d6b-904d-5ab90c1d71f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.170Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dac10677-2fea-4d6b-904d-5ab90c1d71f1 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk VolumeID:dac10677-2fea-4d6b-904d-5ab90c1d71f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.182Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dac10677-2fea-4d6b-904d-5ab90c1d71f1 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dac10677-2fea-4d6b-904d-5ab90c1d71f1 UID:bc77bce9-9ae3-435b-863c-a5ab3f136092 ResourceVersion:7155145 Generation:1 CreationTimestamp:2020-07-17 05:59:58 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 05:59:58 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk VolumeID:dac10677-2fea-4d6b-904d-5ab90c1d71f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.182Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {dac10677-2fea-4d6b-904d-5ab90c1d71f1   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dac10677-2fea-4d6b-904d-5ab90c1d71f1 bc77bce9-9ae3-435b-863c-a5ab3f136092 7155145 1 2020-07-17 05:59:58 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 05:59:58 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk dac10677-2fea-4d6b-904d-5ab90c1d71f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.182Z	DEBUG	syncer/util.go:46	FullSync: pv dac10677-2fea-4d6b-904d-5ab90c1d71f1 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.182Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.182Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.197Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "be9768f1-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002d41b0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T05:59:58.198Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T05:59:58.198Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3d9e3b18-9f22-44f7-a792-0c9a21021f64.vmdk", volumeID: "dac10677-2fea-4d6b-904d-5ab90c1d71f1" mapping in the cache
2020-07-17T05:59:58.215Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.308Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "be9768f1-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c46a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.308Z	DEBUG	volume/manager.go:244	Deleted task for be9768f1-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.308Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "be9768f1-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c46a", volumeID: "922f1a1c-0842-42fd-8a0d-7f7472b8c58c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.308Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk" as container volume with ID: "922f1a1c-0842-42fd-8a0d-7f7472b8c58c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.308Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk" with CNS. VolumeID: "922f1a1c-0842-42fd-8a0d-7f7472b8c58c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.308Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {922f1a1c-0842-42fd-8a0d-7f7472b8c58c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk 922f1a1c-0842-42fd-8a0d-7f7472b8c58c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.308Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:922f1a1c-0842-42fd-8a0d-7f7472b8c58c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk VolumeID:922f1a1c-0842-42fd-8a0d-7f7472b8c58c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.316Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:00:16.316Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk", volumeID: "922f1a1c-0842-42fd-8a0d-7f7472b8c58c" mapping in the cache
2020-07-17T06:00:16.316Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:922f1a1c-0842-42fd-8a0d-7f7472b8c58c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/922f1a1c-0842-42fd-8a0d-7f7472b8c58c UID:f0b7cd6e-9b68-452f-a7e3-0ea854f12127 ResourceVersion:7155202 Generation:1 CreationTimestamp:2020-07-17 06:00:16 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:00:16 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk VolumeID:922f1a1c-0842-42fd-8a0d-7f7472b8c58c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.316Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {922f1a1c-0842-42fd-8a0d-7f7472b8c58c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/922f1a1c-0842-42fd-8a0d-7f7472b8c58c f0b7cd6e-9b68-452f-a7e3-0ea854f12127 7155202 1 2020-07-17 06:00:16 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:00:16 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3b3a1a0e-38d3-4b90-9d03-14b25c316b92.vmdk 922f1a1c-0842-42fd-8a0d-7f7472b8c58c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.316Z	DEBUG	syncer/util.go:46	FullSync: pv 922f1a1c-0842-42fd-8a0d-7f7472b8c58c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.316Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.316Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.316Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "c966637f-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00059d0e0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:16.326Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.035Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "c966637f-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c483"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.035Z	DEBUG	volume/manager.go:244	Deleted task for c966637f-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.035Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "c966637f-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c483", volumeID: "d1321f01-5de4-4619-9bd7-a4c5694ec1d0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.035Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk" as container volume with ID: "d1321f01-5de4-4619-9bd7-a4c5694ec1d0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.035Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk" with CNS. VolumeID: "d1321f01-5de4-4619-9bd7-a4c5694ec1d0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.035Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {d1321f01-5de4-4619-9bd7-a4c5694ec1d0      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk d1321f01-5de4-4619-9bd7-a4c5694ec1d0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.036Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d1321f01-5de4-4619-9bd7-a4c5694ec1d0 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk VolumeID:d1321f01-5de4-4619-9bd7-a4c5694ec1d0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.041Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:00:32.042Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk", volumeID: "d1321f01-5de4-4619-9bd7-a4c5694ec1d0" mapping in the cache
2020-07-17T06:00:32.042Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d1321f01-5de4-4619-9bd7-a4c5694ec1d0 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d1321f01-5de4-4619-9bd7-a4c5694ec1d0 UID:c0574100-a8dd-47a7-9a06-2fae758f2a94 ResourceVersion:7155253 Generation:1 CreationTimestamp:2020-07-17 06:00:32 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:00:32 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk VolumeID:d1321f01-5de4-4619-9bd7-a4c5694ec1d0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.044Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {d1321f01-5de4-4619-9bd7-a4c5694ec1d0   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d1321f01-5de4-4619-9bd7-a4c5694ec1d0 c0574100-a8dd-47a7-9a06-2fae758f2a94 7155253 1 2020-07-17 06:00:32 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:00:32 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-18988d8c-dc1f-48ff-b720-653ed09b1ecd.vmdk d1321f01-5de4-4619-9bd7-a4c5694ec1d0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.045Z	DEBUG	syncer/util.go:46	FullSync: pv d1321f01-5de4-4619-9bd7-a4c5694ec1d0 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.045Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.045Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.045Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac67e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "d2c6622c-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00061f290)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:32.055Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.563Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "d2c6622c-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c48e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.563Z	DEBUG	volume/manager.go:244	Deleted task for d2c6622c-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.563Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "d2c6622c-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c48e", volumeID: "4939c045-8231-4e44-b4f6-326c13f484a0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.563Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk" as container volume with ID: "4939c045-8231-4e44-b4f6-326c13f484a0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.563Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk" with CNS. VolumeID: "4939c045-8231-4e44-b4f6-326c13f484a0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.563Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {4939c045-8231-4e44-b4f6-326c13f484a0      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk 4939c045-8231-4e44-b4f6-326c13f484a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.563Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4939c045-8231-4e44-b4f6-326c13f484a0 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk VolumeID:4939c045-8231-4e44-b4f6-326c13f484a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.571Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:00:42.571Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4939c045-8231-4e44-b4f6-326c13f484a0 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4939c045-8231-4e44-b4f6-326c13f484a0 UID:b40fa37b-bd14-4445-9cc2-6c7748f57b4d ResourceVersion:7155286 Generation:1 CreationTimestamp:2020-07-17 06:00:42 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:00:42 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk VolumeID:4939c045-8231-4e44-b4f6-326c13f484a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.572Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {4939c045-8231-4e44-b4f6-326c13f484a0   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4939c045-8231-4e44-b4f6-326c13f484a0 b40fa37b-bd14-4445-9cc2-6c7748f57b4d 7155286 1 2020-07-17 06:00:42 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:00:42 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk 4939c045-8231-4e44-b4f6-326c13f484a0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.572Z	DEBUG	syncer/util.go:46	FullSync: pv 4939c045-8231-4e44-b4f6-326c13f484a0 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.572Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.574Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.575Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac6a80)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "d90cf5b0-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000defc80)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:42.572Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2633454e-fd31-4053-8458-e9529cba060c.vmdk", volumeID: "4939c045-8231-4e44-b4f6-326c13f484a0" mapping in the cache
2020-07-17T06:00:42.585Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.592Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "d90cf5b0-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4b1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.592Z	DEBUG	volume/manager.go:244	Deleted task for d90cf5b0-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.592Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "d90cf5b0-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4b1", volumeID: "2c076e92-56da-4fed-acef-10ae4f1eac61"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.592Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk" as container volume with ID: "2c076e92-56da-4fed-acef-10ae4f1eac61"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.592Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk" with CNS. VolumeID: "2c076e92-56da-4fed-acef-10ae4f1eac61"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.592Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {2c076e92-56da-4fed-acef-10ae4f1eac61      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk 2c076e92-56da-4fed-acef-10ae4f1eac61}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.593Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2c076e92-56da-4fed-acef-10ae4f1eac61 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk VolumeID:2c076e92-56da-4fed-acef-10ae4f1eac61}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.600Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:00:59.601Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk", volumeID: "2c076e92-56da-4fed-acef-10ae4f1eac61" mapping in the cache
2020-07-17T06:00:59.600Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2c076e92-56da-4fed-acef-10ae4f1eac61 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2c076e92-56da-4fed-acef-10ae4f1eac61 UID:7af682dc-a355-4330-8b18-20f337f1e268 ResourceVersion:7155337 Generation:1 CreationTimestamp:2020-07-17 06:00:59 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:00:59 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk VolumeID:2c076e92-56da-4fed-acef-10ae4f1eac61}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.601Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {2c076e92-56da-4fed-acef-10ae4f1eac61   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2c076e92-56da-4fed-acef-10ae4f1eac61 7af682dc-a355-4330-8b18-20f337f1e268 7155337 1 2020-07-17 06:00:59 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:00:59 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0ef43afc-be03-4508-826f-f1a10a01c9ff.vmdk 2c076e92-56da-4fed-acef-10ae4f1eac61}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.601Z	DEBUG	syncer/util.go:46	FullSync: pv 2c076e92-56da-4fed-acef-10ae4f1eac61 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.601Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.601Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.601Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "e333244b-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000def440)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:00:59.609Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.745Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "e333244b-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4b4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.745Z	DEBUG	volume/manager.go:244	Deleted task for e333244b-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.745Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "e333244b-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4b4", volumeID: "cef89cbc-d037-4172-99e0-6a3c6f5e2415"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.745Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk" as container volume with ID: "cef89cbc-d037-4172-99e0-6a3c6f5e2415"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.745Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk" with CNS. VolumeID: "cef89cbc-d037-4172-99e0-6a3c6f5e2415"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.745Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {cef89cbc-d037-4172-99e0-6a3c6f5e2415      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk cef89cbc-d037-4172-99e0-6a3c6f5e2415}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.746Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:cef89cbc-d037-4172-99e0-6a3c6f5e2415 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk VolumeID:cef89cbc-d037-4172-99e0-6a3c6f5e2415}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.754Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:cef89cbc-d037-4172-99e0-6a3c6f5e2415 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/cef89cbc-d037-4172-99e0-6a3c6f5e2415 UID:f428721a-b6a1-4faf-adbc-a6523fd1e967 ResourceVersion:7155406 Generation:1 CreationTimestamp:2020-07-17 06:01:20 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:01:20 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk VolumeID:cef89cbc-d037-4172-99e0-6a3c6f5e2415}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.754Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {cef89cbc-d037-4172-99e0-6a3c6f5e2415   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/cef89cbc-d037-4172-99e0-6a3c6f5e2415 f428721a-b6a1-4faf-adbc-a6523fd1e967 7155406 1 2020-07-17 06:01:20 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:01:20 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk cef89cbc-d037-4172-99e0-6a3c6f5e2415}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.754Z	DEBUG	syncer/util.go:46	FullSync: pv cef89cbc-d037-4172-99e0-6a3c6f5e2415 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.755Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.755Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.757Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ac60e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "efcf0a9a-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00069d6b0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:20.759Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:01:20.760Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6f30995f-621a-448e-8f2d-95257fa9505b.vmdk", volumeID: "cef89cbc-d037-4172-99e0-6a3c6f5e2415" mapping in the cache
2020-07-17T06:01:20.768Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.399Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "efcf0a9a-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4c0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.400Z	DEBUG	volume/manager.go:244	Deleted task for efcf0a9a-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.400Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "efcf0a9a-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4c0", volumeID: "3bf64a81-f740-46ca-b2e3-04416fd5ca83"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.400Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk" as container volume with ID: "3bf64a81-f740-46ca-b2e3-04416fd5ca83"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.400Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk" with CNS. VolumeID: "3bf64a81-f740-46ca-b2e3-04416fd5ca83"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.400Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {3bf64a81-f740-46ca-b2e3-04416fd5ca83      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk 3bf64a81-f740-46ca-b2e3-04416fd5ca83}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.400Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3bf64a81-f740-46ca-b2e3-04416fd5ca83 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk VolumeID:3bf64a81-f740-46ca-b2e3-04416fd5ca83}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.409Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:01:33.409Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk", volumeID: "3bf64a81-f740-46ca-b2e3-04416fd5ca83" mapping in the cache
2020-07-17T06:01:33.411Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3bf64a81-f740-46ca-b2e3-04416fd5ca83 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3bf64a81-f740-46ca-b2e3-04416fd5ca83 UID:424173fa-3f95-40c5-9eaa-bb313249735d ResourceVersion:7155444 Generation:1 CreationTimestamp:2020-07-17 06:01:33 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:01:33 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk VolumeID:3bf64a81-f740-46ca-b2e3-04416fd5ca83}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.411Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {3bf64a81-f740-46ca-b2e3-04416fd5ca83   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3bf64a81-f740-46ca-b2e3-04416fd5ca83 424173fa-3f95-40c5-9eaa-bb313249735d 7155444 1 2020-07-17 06:01:33 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:01:33 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-15408906-4a45-4768-827b-46deca147439.vmdk 3bf64a81-f740-46ca-b2e3-04416fd5ca83}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.411Z	DEBUG	syncer/util.go:46	FullSync: pv 3bf64a81-f740-46ca-b2e3-04416fd5ca83 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.412Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.412Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.413Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004682a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "f75a3fc5-c7f2-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00059dbf0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:33.422Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.090Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "f75a3fc5-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4c3"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.091Z	DEBUG	volume/manager.go:244	Deleted task for f75a3fc5-c7f2-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.091Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "f75a3fc5-c7f2-11ea-8f7c-9695ff1c5b6f", opId: "9168c4c3", volumeID: "5df79911-285c-4f58-bf66-11a67545f2ea"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.091Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk" as container volume with ID: "5df79911-285c-4f58-bf66-11a67545f2ea"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.091Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk" with CNS. VolumeID: "5df79911-285c-4f58-bf66-11a67545f2ea"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.091Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {5df79911-285c-4f58-bf66-11a67545f2ea      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk 5df79911-285c-4f58-bf66-11a67545f2ea}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.091Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:5df79911-285c-4f58-bf66-11a67545f2ea GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk VolumeID:5df79911-285c-4f58-bf66-11a67545f2ea}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.097Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:01:48.099Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk", volumeID: "5df79911-285c-4f58-bf66-11a67545f2ea" mapping in the cache
2020-07-17T06:01:48.100Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:5df79911-285c-4f58-bf66-11a67545f2ea GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/5df79911-285c-4f58-bf66-11a67545f2ea UID:5e80c1b5-3869-42f8-9608-564b96f76c30 ResourceVersion:7155489 Generation:1 CreationTimestamp:2020-07-17 06:01:48 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:01:48 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk VolumeID:5df79911-285c-4f58-bf66-11a67545f2ea}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.100Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {5df79911-285c-4f58-bf66-11a67545f2ea   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/5df79911-285c-4f58-bf66-11a67545f2ea 5e80c1b5-3869-42f8-9608-564b96f76c30 7155489 1 2020-07-17 06:01:48 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:01:48 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f7e2e80c-60bd-4c77-a6ff-0f11a2fa3444.vmdk 5df79911-285c-4f58-bf66-11a67545f2ea}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.100Z	DEBUG	syncer/util.go:46	FullSync: pv 5df79911-285c-4f58-bf66-11a67545f2ea is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.100Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.101Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.101Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "001b932c-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0001b3bf0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:01:48.111Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.140Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "001b932c-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c4ea"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.140Z	DEBUG	volume/manager.go:244	Deleted task for 001b932c-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.140Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "001b932c-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c4ea", volumeID: "2544b150-c73b-4c72-9bcb-3b5bd343c335"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.140Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk" as container volume with ID: "2544b150-c73b-4c72-9bcb-3b5bd343c335"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.140Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk" with CNS. VolumeID: "2544b150-c73b-4c72-9bcb-3b5bd343c335"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.140Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {2544b150-c73b-4c72-9bcb-3b5bd343c335      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk 2544b150-c73b-4c72-9bcb-3b5bd343c335}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.140Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2544b150-c73b-4c72-9bcb-3b5bd343c335 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk VolumeID:2544b150-c73b-4c72-9bcb-3b5bd343c335}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.161Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:02:01.161Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk", volumeID: "2544b150-c73b-4c72-9bcb-3b5bd343c335" mapping in the cache
2020-07-17T06:02:01.162Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2544b150-c73b-4c72-9bcb-3b5bd343c335 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2544b150-c73b-4c72-9bcb-3b5bd343c335 UID:a85ce81d-c216-4bfa-a8e6-3286b766d691 ResourceVersion:7155531 Generation:1 CreationTimestamp:2020-07-17 06:02:01 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:02:01 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk VolumeID:2544b150-c73b-4c72-9bcb-3b5bd343c335}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.162Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {2544b150-c73b-4c72-9bcb-3b5bd343c335   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2544b150-c73b-4c72-9bcb-3b5bd343c335 a85ce81d-c216-4bfa-a8e6-3286b766d691 7155531 1 2020-07-17 06:02:01 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:02:01 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34ea46dd-562c-4b3a-b4e9-100563c31aed.vmdk 2544b150-c73b-4c72-9bcb-3b5bd343c335}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.162Z	DEBUG	syncer/util.go:46	FullSync: pv 2544b150-c73b-4c72-9bcb-3b5bd343c335 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.162Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.164Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.164Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "07e4a187-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000571a10)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:01.208Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.611Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "07e4a187-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c4ed"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.611Z	DEBUG	volume/manager.go:244	Deleted task for 07e4a187-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.611Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "07e4a187-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c4ed", volumeID: "61542097-8ac3-46e6-b09b-ade29db3341f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.611Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk" as container volume with ID: "61542097-8ac3-46e6-b09b-ade29db3341f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.611Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk" with CNS. VolumeID: "61542097-8ac3-46e6-b09b-ade29db3341f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.611Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {61542097-8ac3-46e6-b09b-ade29db3341f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk 61542097-8ac3-46e6-b09b-ade29db3341f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.612Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:61542097-8ac3-46e6-b09b-ade29db3341f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk VolumeID:61542097-8ac3-46e6-b09b-ade29db3341f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.630Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:02:11.631Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk", volumeID: "61542097-8ac3-46e6-b09b-ade29db3341f" mapping in the cache
2020-07-17T06:02:11.631Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:61542097-8ac3-46e6-b09b-ade29db3341f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/61542097-8ac3-46e6-b09b-ade29db3341f UID:ba1517d4-e953-4181-a9eb-79f81f6c7fb2 ResourceVersion:7155565 Generation:1 CreationTimestamp:2020-07-17 06:02:11 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:02:11 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk VolumeID:61542097-8ac3-46e6-b09b-ade29db3341f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.631Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {61542097-8ac3-46e6-b09b-ade29db3341f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/61542097-8ac3-46e6-b09b-ade29db3341f ba1517d4-e953-4181-a9eb-79f81f6c7fb2 7155565 1 2020-07-17 06:02:11 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:02:11 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c5da1723-7567-431c-bbcb-2e25db743f80.vmdk 61542097-8ac3-46e6-b09b-ade29db3341f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.632Z	DEBUG	syncer/util.go:46	FullSync: pv 61542097-8ac3-46e6-b09b-ade29db3341f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.632Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.632Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.633Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468700)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "0e2244b1-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002e3cb0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:11.651Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.687Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "0e2244b1-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c4f8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.687Z	DEBUG	volume/manager.go:244	Deleted task for 0e2244b1-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.687Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "0e2244b1-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c4f8", volumeID: "26939519-8a83-4d94-9b4d-998a2cfdddd0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.687Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk" as container volume with ID: "26939519-8a83-4d94-9b4d-998a2cfdddd0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.687Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk" with CNS. VolumeID: "26939519-8a83-4d94-9b4d-998a2cfdddd0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.687Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {26939519-8a83-4d94-9b4d-998a2cfdddd0      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk 26939519-8a83-4d94-9b4d-998a2cfdddd0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.687Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:26939519-8a83-4d94-9b4d-998a2cfdddd0 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk VolumeID:26939519-8a83-4d94-9b4d-998a2cfdddd0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.696Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:26939519-8a83-4d94-9b4d-998a2cfdddd0 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/26939519-8a83-4d94-9b4d-998a2cfdddd0 UID:aa0b6d18-01dd-4f11-b7bf-9020467d83ec ResourceVersion:7155602 Generation:1 CreationTimestamp:2020-07-17 06:02:23 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:02:23 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk VolumeID:26939519-8a83-4d94-9b4d-998a2cfdddd0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.696Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {26939519-8a83-4d94-9b4d-998a2cfdddd0   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/26939519-8a83-4d94-9b4d-998a2cfdddd0 aa0b6d18-01dd-4f11-b7bf-9020467d83ec 7155602 1 2020-07-17 06:02:23 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:02:23 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk 26939519-8a83-4d94-9b4d-998a2cfdddd0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.696Z	DEBUG	syncer/util.go:46	FullSync: pv 26939519-8a83-4d94-9b4d-998a2cfdddd0 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.696Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.696Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.696Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:02:23.696Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b224290a-08eb-419a-9564-68d4dcd35b68.vmdk", volumeID: "26939519-8a83-4d94-9b4d-998a2cfdddd0" mapping in the cache
2020-07-17T06:02:23.697Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004688c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "1552fb01-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b9290)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:23.706Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.774Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "1552fb01-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c525"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.774Z	DEBUG	volume/manager.go:244	Deleted task for 1552fb01-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.774Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "1552fb01-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c525", volumeID: "382dad65-ad70-4b36-ab96-285efd2510d4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.774Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk" as container volume with ID: "382dad65-ad70-4b36-ab96-285efd2510d4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.774Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk" with CNS. VolumeID: "382dad65-ad70-4b36-ab96-285efd2510d4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.775Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {382dad65-ad70-4b36-ab96-285efd2510d4      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk 382dad65-ad70-4b36-ab96-285efd2510d4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.821Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:382dad65-ad70-4b36-ab96-285efd2510d4 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk VolumeID:382dad65-ad70-4b36-ab96-285efd2510d4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.832Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:02:36.833Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk", volumeID: "382dad65-ad70-4b36-ab96-285efd2510d4" mapping in the cache
2020-07-17T06:02:36.835Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:382dad65-ad70-4b36-ab96-285efd2510d4 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/382dad65-ad70-4b36-ab96-285efd2510d4 UID:37a05d5e-4092-42dd-9125-d544ff578b9d ResourceVersion:7155643 Generation:1 CreationTimestamp:2020-07-17 06:02:36 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:02:36 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk VolumeID:382dad65-ad70-4b36-ab96-285efd2510d4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.836Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {382dad65-ad70-4b36-ab96-285efd2510d4   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/382dad65-ad70-4b36-ab96-285efd2510d4 37a05d5e-4092-42dd-9125-d544ff578b9d 7155643 1 2020-07-17 06:02:36 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:02:36 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d5fe221b-3ba2-4b5d-8c97-0fd39800f424.vmdk 382dad65-ad70-4b36-ab96-285efd2510d4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.836Z	DEBUG	syncer/util.go:46	FullSync: pv 382dad65-ad70-4b36-ab96-285efd2510d4 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.836Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.836Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.836Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468b60)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "1d280bdc-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004dfe90)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:36.847Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.464Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "1d280bdc-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c528"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.464Z	DEBUG	volume/manager.go:244	Deleted task for 1d280bdc-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.464Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "1d280bdc-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c528", volumeID: "1c9a97f6-3c65-4430-b731-06ea4ea4fc8e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.464Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk" as container volume with ID: "1c9a97f6-3c65-4430-b731-06ea4ea4fc8e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.464Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk" with CNS. VolumeID: "1c9a97f6-3c65-4430-b731-06ea4ea4fc8e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.464Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {1c9a97f6-3c65-4430-b731-06ea4ea4fc8e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk 1c9a97f6-3c65-4430-b731-06ea4ea4fc8e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.464Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:1c9a97f6-3c65-4430-b731-06ea4ea4fc8e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk VolumeID:1c9a97f6-3c65-4430-b731-06ea4ea4fc8e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.474Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:1c9a97f6-3c65-4430-b731-06ea4ea4fc8e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/1c9a97f6-3c65-4430-b731-06ea4ea4fc8e UID:2b876579-b503-4140-9515-461285e828b4 ResourceVersion:7155681 Generation:1 CreationTimestamp:2020-07-17 06:02:48 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:02:48 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk VolumeID:1c9a97f6-3c65-4430-b731-06ea4ea4fc8e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.474Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {1c9a97f6-3c65-4430-b731-06ea4ea4fc8e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/1c9a97f6-3c65-4430-b731-06ea4ea4fc8e 2b876579-b503-4140-9515-461285e828b4 7155681 1 2020-07-17 06:02:48 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:02:48 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk 1c9a97f6-3c65-4430-b731-06ea4ea4fc8e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.474Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:02:48.475Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ab3cd43c-b6d8-42f3-996e-5e4bb163d413.vmdk", volumeID: "1c9a97f6-3c65-4430-b731-06ea4ea4fc8e" mapping in the cache
2020-07-17T06:02:48.474Z	DEBUG	syncer/util.go:46	FullSync: pv 1c9a97f6-3c65-4430-b731-06ea4ea4fc8e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.476Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.477Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.477Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468d20)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "24183f61-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00089b2c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:48.488Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.256Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "24183f61-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c554"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.256Z	DEBUG	volume/manager.go:244	Deleted task for 24183f61-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.257Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "24183f61-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c554", volumeID: "dc70fbac-9906-4e42-a036-522e093221af"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.257Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk" as container volume with ID: "dc70fbac-9906-4e42-a036-522e093221af"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.257Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk" with CNS. VolumeID: "dc70fbac-9906-4e42-a036-522e093221af"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.257Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {dc70fbac-9906-4e42-a036-522e093221af      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk dc70fbac-9906-4e42-a036-522e093221af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.257Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dc70fbac-9906-4e42-a036-522e093221af GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk VolumeID:dc70fbac-9906-4e42-a036-522e093221af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.266Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:02:59.267Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk", volumeID: "dc70fbac-9906-4e42-a036-522e093221af" mapping in the cache
2020-07-17T06:02:59.267Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dc70fbac-9906-4e42-a036-522e093221af GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dc70fbac-9906-4e42-a036-522e093221af UID:c5799ae0-2096-4334-9765-6f823b9dd728 ResourceVersion:7155714 Generation:1 CreationTimestamp:2020-07-17 06:02:59 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:02:59 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk VolumeID:dc70fbac-9906-4e42-a036-522e093221af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.267Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {dc70fbac-9906-4e42-a036-522e093221af   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dc70fbac-9906-4e42-a036-522e093221af c5799ae0-2096-4334-9765-6f823b9dd728 7155714 1 2020-07-17 06:02:59 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:02:59 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b6eb852d-76bb-486b-a7e4-3ead76392545.vmdk dc70fbac-9906-4e42-a036-522e093221af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.267Z	DEBUG	syncer/util.go:46	FullSync: pv dc70fbac-9906-4e42-a036-522e093221af is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.267Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.267Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.268Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "2a86c334-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dd2240)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:02:59.276Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.553Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "2a86c334-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c557"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.553Z	DEBUG	volume/manager.go:244	Deleted task for 2a86c334-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.553Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "2a86c334-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c557", volumeID: "a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.553Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk" as container volume with ID: "a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.553Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk" with CNS. VolumeID: "a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.553Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.553Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk VolumeID:a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.559Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:03:11.561Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07 UID:019c0167-ef98-4f71-8c75-fbca04598bb1 ResourceVersion:7155753 Generation:1 CreationTimestamp:2020-07-17 06:03:11 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:03:11 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk VolumeID:a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.561Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07 019c0167-ef98-4f71-8c75-fbca04598bb1 7155753 1 2020-07-17 06:03:11 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:03:11 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.562Z	DEBUG	syncer/util.go:46	FullSync: pv a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.562Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.562Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.562Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000974000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "31dac69e-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dbf290)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:11.563Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0812f07f-aa26-415c-8b16-24e4b722ac6e.vmdk", volumeID: "a1f2f83f-5695-4ffb-9fe8-e4e3a9572d07" mapping in the cache
2020-07-17T06:03:11.571Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.795Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "31dac69e-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c559"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.795Z	DEBUG	volume/manager.go:244	Deleted task for 31dac69e-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.796Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "31dac69e-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c559", volumeID: "4e38aa9c-4071-43ae-bc95-d04e973c3dd6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.796Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk" as container volume with ID: "4e38aa9c-4071-43ae-bc95-d04e973c3dd6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.796Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk" with CNS. VolumeID: "4e38aa9c-4071-43ae-bc95-d04e973c3dd6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.796Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {4e38aa9c-4071-43ae-bc95-d04e973c3dd6      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk 4e38aa9c-4071-43ae-bc95-d04e973c3dd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.796Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4e38aa9c-4071-43ae-bc95-d04e973c3dd6 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk VolumeID:4e38aa9c-4071-43ae-bc95-d04e973c3dd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.803Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:03:25.803Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk", volumeID: "4e38aa9c-4071-43ae-bc95-d04e973c3dd6" mapping in the cache
2020-07-17T06:03:25.806Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4e38aa9c-4071-43ae-bc95-d04e973c3dd6 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4e38aa9c-4071-43ae-bc95-d04e973c3dd6 UID:22642b5a-db06-4d99-9ae1-aee6e8a0baaf ResourceVersion:7155797 Generation:1 CreationTimestamp:2020-07-17 06:03:25 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:03:25 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk VolumeID:4e38aa9c-4071-43ae-bc95-d04e973c3dd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.806Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {4e38aa9c-4071-43ae-bc95-d04e973c3dd6   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4e38aa9c-4071-43ae-bc95-d04e973c3dd6 22642b5a-db06-4d99-9ae1-aee6e8a0baaf 7155797 1 2020-07-17 06:03:25 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:03:25 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-962caab7-cd1c-4ba7-83b9-4a8a7e6c3cff.vmdk 4e38aa9c-4071-43ae-bc95-d04e973c3dd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.806Z	DEBUG	syncer/util.go:46	FullSync: pv 4e38aa9c-4071-43ae-bc95-d04e973c3dd6 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.806Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.806Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.807Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0009741c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "3a584e61-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004df950)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:25.817Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.694Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "3a584e61-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c564"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.695Z	DEBUG	volume/manager.go:244	Deleted task for 3a584e61-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.695Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "3a584e61-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c564", volumeID: "b6327c4a-8c1d-4276-b274-4693bf677594"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.695Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk" as container volume with ID: "b6327c4a-8c1d-4276-b274-4693bf677594"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.695Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk" with CNS. VolumeID: "b6327c4a-8c1d-4276-b274-4693bf677594"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.695Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {b6327c4a-8c1d-4276-b274-4693bf677594      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk b6327c4a-8c1d-4276-b274-4693bf677594}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.695Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b6327c4a-8c1d-4276-b274-4693bf677594 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk VolumeID:b6327c4a-8c1d-4276-b274-4693bf677594}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.705Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b6327c4a-8c1d-4276-b274-4693bf677594 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b6327c4a-8c1d-4276-b274-4693bf677594 UID:afc781d8-c57c-45fe-8596-7748b5160c4c ResourceVersion:7155842 Generation:1 CreationTimestamp:2020-07-17 06:03:39 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:03:39 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk VolumeID:b6327c4a-8c1d-4276-b274-4693bf677594}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.705Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {b6327c4a-8c1d-4276-b274-4693bf677594   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b6327c4a-8c1d-4276-b274-4693bf677594 afc781d8-c57c-45fe-8596-7748b5160c4c 7155842 1 2020-07-17 06:03:39 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:03:39 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk b6327c4a-8c1d-4276-b274-4693bf677594}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.705Z	DEBUG	syncer/util.go:46	FullSync: pv b6327c4a-8c1d-4276-b274-4693bf677594 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.705Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.705Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.705Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "42a123b5-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000447770)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:39.706Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:03:39.706Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26459e5e-9515-44a8-9863-4682aa658ab3.vmdk", volumeID: "b6327c4a-8c1d-4276-b274-4693bf677594" mapping in the cache
2020-07-17T06:03:39.723Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.061Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "42a123b5-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c56a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.062Z	DEBUG	volume/manager.go:244	Deleted task for 42a123b5-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.062Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "42a123b5-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c56a", volumeID: "b8069279-a4e9-4c7f-8c0b-945e8b982400"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.062Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk" as container volume with ID: "b8069279-a4e9-4c7f-8c0b-945e8b982400"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.062Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk" with CNS. VolumeID: "b8069279-a4e9-4c7f-8c0b-945e8b982400"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.062Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {b8069279-a4e9-4c7f-8c0b-945e8b982400      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk b8069279-a4e9-4c7f-8c0b-945e8b982400}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.062Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b8069279-a4e9-4c7f-8c0b-945e8b982400 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk VolumeID:b8069279-a4e9-4c7f-8c0b-945e8b982400}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.070Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b8069279-a4e9-4c7f-8c0b-945e8b982400 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b8069279-a4e9-4c7f-8c0b-945e8b982400 UID:2365cfd2-a296-4c8b-8003-a3eb4a68e0cb ResourceVersion:7155879 Generation:1 CreationTimestamp:2020-07-17 06:03:51 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:03:51 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk VolumeID:b8069279-a4e9-4c7f-8c0b-945e8b982400}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.070Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {b8069279-a4e9-4c7f-8c0b-945e8b982400   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b8069279-a4e9-4c7f-8c0b-945e8b982400 2365cfd2-a296-4c8b-8003-a3eb4a68e0cb 7155879 1 2020-07-17 06:03:51 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:03:51 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk b8069279-a4e9-4c7f-8c0b-945e8b982400}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.070Z	DEBUG	syncer/util.go:46	FullSync: pv b8069279-a4e9-4c7f-8c0b-945e8b982400 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.070Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.071Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.071Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "49674cde-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007a3ef0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:03:51.072Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:03:51.072Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3bb8bf75-2983-4155-8ba3-16e1f98b7308.vmdk", volumeID: "b8069279-a4e9-4c7f-8c0b-945e8b982400" mapping in the cache
2020-07-17T06:03:51.079Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.337Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "49674cde-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c599"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.337Z	DEBUG	volume/manager.go:244	Deleted task for 49674cde-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.337Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "49674cde-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c599", volumeID: "fe9e983c-3f0f-4635-a15e-dc78f525c62d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.337Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk" as container volume with ID: "fe9e983c-3f0f-4635-a15e-dc78f525c62d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.337Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk" with CNS. VolumeID: "fe9e983c-3f0f-4635-a15e-dc78f525c62d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.337Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {fe9e983c-3f0f-4635-a15e-dc78f525c62d      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk fe9e983c-3f0f-4635-a15e-dc78f525c62d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.337Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fe9e983c-3f0f-4635-a15e-dc78f525c62d GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk VolumeID:fe9e983c-3f0f-4635-a15e-dc78f525c62d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.346Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:04:07.347Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk", volumeID: "fe9e983c-3f0f-4635-a15e-dc78f525c62d" mapping in the cache
2020-07-17T06:04:07.347Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fe9e983c-3f0f-4635-a15e-dc78f525c62d GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fe9e983c-3f0f-4635-a15e-dc78f525c62d UID:855b3fa6-64c2-4172-a393-fa910059b8f4 ResourceVersion:7155928 Generation:1 CreationTimestamp:2020-07-17 06:04:07 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:04:07 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk VolumeID:fe9e983c-3f0f-4635-a15e-dc78f525c62d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.348Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {fe9e983c-3f0f-4635-a15e-dc78f525c62d   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fe9e983c-3f0f-4635-a15e-dc78f525c62d 855b3fa6-64c2-4172-a393-fa910059b8f4 7155928 1 2020-07-17 06:04:07 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:04:07 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ffcdf91-cc00-4fc6-91bd-ada8ddcff6fc.vmdk fe9e983c-3f0f-4635-a15e-dc78f525c62d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.348Z	DEBUG	syncer/util.go:46	FullSync: pv fe9e983c-3f0f-4635-a15e-dc78f525c62d is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.348Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.348Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.348Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004687e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "531b0449-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0003fd740)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:07.359Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.297Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "531b0449-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c59b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.297Z	DEBUG	volume/manager.go:244	Deleted task for 531b0449-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.297Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "531b0449-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c59b", volumeID: "4d583097-f808-41b6-907d-0520fe363598"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.297Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk" as container volume with ID: "4d583097-f808-41b6-907d-0520fe363598"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.297Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk" with CNS. VolumeID: "4d583097-f808-41b6-907d-0520fe363598"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.297Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {4d583097-f808-41b6-907d-0520fe363598      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk 4d583097-f808-41b6-907d-0520fe363598}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.298Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4d583097-f808-41b6-907d-0520fe363598 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk VolumeID:4d583097-f808-41b6-907d-0520fe363598}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.304Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:04:23.305Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk", volumeID: "4d583097-f808-41b6-907d-0520fe363598" mapping in the cache
2020-07-17T06:04:23.307Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4d583097-f808-41b6-907d-0520fe363598 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4d583097-f808-41b6-907d-0520fe363598 UID:acda9bad-e468-4524-8976-fca2c874904d ResourceVersion:7155979 Generation:1 CreationTimestamp:2020-07-17 06:04:23 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:04:23 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk VolumeID:4d583097-f808-41b6-907d-0520fe363598}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.307Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {4d583097-f808-41b6-907d-0520fe363598   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4d583097-f808-41b6-907d-0520fe363598 acda9bad-e468-4524-8976-fca2c874904d 7155979 1 2020-07-17 06:04:23 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:04:23 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1d9d808b-8458-4d82-94a2-0f98bfaff9fd.vmdk 4d583097-f808-41b6-907d-0520fe363598}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.308Z	DEBUG	syncer/util.go:46	FullSync: pv 4d583097-f808-41b6-907d-0520fe363598 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.308Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.308Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.309Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468a80)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "5c9e6d7a-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005dc810)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:23.319Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.108Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "5c9e6d7a-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5bd"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.108Z	DEBUG	volume/manager.go:244	Deleted task for 5c9e6d7a-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.108Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "5c9e6d7a-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5bd", volumeID: "a0811375-9233-4d62-9cd8-86bb9769d009"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.108Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk" as container volume with ID: "a0811375-9233-4d62-9cd8-86bb9769d009"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.108Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk" with CNS. VolumeID: "a0811375-9233-4d62-9cd8-86bb9769d009"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.108Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {a0811375-9233-4d62-9cd8-86bb9769d009      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk a0811375-9233-4d62-9cd8-86bb9769d009}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.108Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:a0811375-9233-4d62-9cd8-86bb9769d009 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk VolumeID:a0811375-9233-4d62-9cd8-86bb9769d009}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.116Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:04:38.117Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk", volumeID: "a0811375-9233-4d62-9cd8-86bb9769d009" mapping in the cache
2020-07-17T06:04:38.118Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:a0811375-9233-4d62-9cd8-86bb9769d009 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/a0811375-9233-4d62-9cd8-86bb9769d009 UID:a42e69ac-9332-4152-a629-bd02273a383d ResourceVersion:7156027 Generation:1 CreationTimestamp:2020-07-17 06:04:38 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:04:38 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk VolumeID:a0811375-9233-4d62-9cd8-86bb9769d009}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.118Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {a0811375-9233-4d62-9cd8-86bb9769d009   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/a0811375-9233-4d62-9cd8-86bb9769d009 a42e69ac-9332-4152-a629-bd02273a383d 7156027 1 2020-07-17 06:04:38 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:04:38 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5e15d5ef-e8c8-4a05-aa8d-0ee9bfc30f57.vmdk a0811375-9233-4d62-9cd8-86bb9769d009}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.119Z	DEBUG	syncer/util.go:46	FullSync: pv a0811375-9233-4d62-9cd8-86bb9769d009 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.119Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.119Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.120Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468c40)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "65725b59-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00061e870)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:38.130Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.469Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "65725b59-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5c1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.471Z	DEBUG	volume/manager.go:244	Deleted task for 65725b59-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.471Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "65725b59-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5c1", volumeID: "b7831f09-dc11-4e37-90d5-fb1e1f36160c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.471Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk" as container volume with ID: "b7831f09-dc11-4e37-90d5-fb1e1f36160c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.471Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk" with CNS. VolumeID: "b7831f09-dc11-4e37-90d5-fb1e1f36160c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.471Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {b7831f09-dc11-4e37-90d5-fb1e1f36160c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk b7831f09-dc11-4e37-90d5-fb1e1f36160c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.471Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b7831f09-dc11-4e37-90d5-fb1e1f36160c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk VolumeID:b7831f09-dc11-4e37-90d5-fb1e1f36160c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.480Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:04:54.480Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk", volumeID: "b7831f09-dc11-4e37-90d5-fb1e1f36160c" mapping in the cache
2020-07-17T06:04:54.482Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b7831f09-dc11-4e37-90d5-fb1e1f36160c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b7831f09-dc11-4e37-90d5-fb1e1f36160c UID:844fd794-fce5-457e-ad5a-a96ffbb2c049 ResourceVersion:7156080 Generation:1 CreationTimestamp:2020-07-17 06:04:54 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:04:54 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk VolumeID:b7831f09-dc11-4e37-90d5-fb1e1f36160c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.482Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {b7831f09-dc11-4e37-90d5-fb1e1f36160c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b7831f09-dc11-4e37-90d5-fb1e1f36160c 844fd794-fce5-457e-ad5a-a96ffbb2c049 7156080 1 2020-07-17 06:04:54 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:04:54 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5b97678a-91fb-4bc1-a8e9-1ed5a55bae19.vmdk b7831f09-dc11-4e37-90d5-fb1e1f36160c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.482Z	DEBUG	syncer/util.go:46	FullSync: pv b7831f09-dc11-4e37-90d5-fb1e1f36160c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.482Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.482Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.483Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0009742a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "6f33311c-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000988120)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:04:54.500Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.804Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "6f33311c-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5f5"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.804Z	DEBUG	volume/manager.go:244	Deleted task for 6f33311c-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.804Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "6f33311c-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5f5", volumeID: "5de01ad3-be5a-4167-a2a4-8a4c26a4056e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.804Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk" as container volume with ID: "5de01ad3-be5a-4167-a2a4-8a4c26a4056e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.804Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk" with CNS. VolumeID: "5de01ad3-be5a-4167-a2a4-8a4c26a4056e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.804Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {5de01ad3-be5a-4167-a2a4-8a4c26a4056e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk 5de01ad3-be5a-4167-a2a4-8a4c26a4056e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.804Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:5de01ad3-be5a-4167-a2a4-8a4c26a4056e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk VolumeID:5de01ad3-be5a-4167-a2a4-8a4c26a4056e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.811Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:05:09.812Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk", volumeID: "5de01ad3-be5a-4167-a2a4-8a4c26a4056e" mapping in the cache
2020-07-17T06:05:09.814Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:5de01ad3-be5a-4167-a2a4-8a4c26a4056e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/5de01ad3-be5a-4167-a2a4-8a4c26a4056e UID:fc133f07-0f0b-41c8-9090-89b37fceed4b ResourceVersion:7156125 Generation:1 CreationTimestamp:2020-07-17 06:05:09 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:05:09 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk VolumeID:5de01ad3-be5a-4167-a2a4-8a4c26a4056e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.815Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {5de01ad3-be5a-4167-a2a4-8a4c26a4056e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/5de01ad3-be5a-4167-a2a4-8a4c26a4056e fc133f07-0f0b-41c8-9090-89b37fceed4b 7156125 1 2020-07-17 06:05:09 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:05:09 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e897a4bd-19d1-40b7-aab4-d572db0bbcde.vmdk 5de01ad3-be5a-4167-a2a4-8a4c26a4056e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.815Z	DEBUG	syncer/util.go:46	FullSync: pv 5de01ad3-be5a-4167-a2a4-8a4c26a4056e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.815Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.815Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.815Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "7856c824-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000935c80)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:09.825Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.418Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "7856c824-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5f8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.418Z	DEBUG	volume/manager.go:244	Deleted task for 7856c824-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.418Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "7856c824-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5f8", volumeID: "2f9823d9-8f86-45fe-abe2-ed22fd39616e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.418Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk" as container volume with ID: "2f9823d9-8f86-45fe-abe2-ed22fd39616e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.418Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk" with CNS. VolumeID: "2f9823d9-8f86-45fe-abe2-ed22fd39616e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.418Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {2f9823d9-8f86-45fe-abe2-ed22fd39616e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk 2f9823d9-8f86-45fe-abe2-ed22fd39616e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.419Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2f9823d9-8f86-45fe-abe2-ed22fd39616e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk VolumeID:2f9823d9-8f86-45fe-abe2-ed22fd39616e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.425Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:05:21.426Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk", volumeID: "2f9823d9-8f86-45fe-abe2-ed22fd39616e" mapping in the cache
2020-07-17T06:05:21.427Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2f9823d9-8f86-45fe-abe2-ed22fd39616e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2f9823d9-8f86-45fe-abe2-ed22fd39616e UID:b65ca541-b85f-4320-b7af-796f30f8da6e ResourceVersion:7156164 Generation:1 CreationTimestamp:2020-07-17 06:05:21 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:05:21 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk VolumeID:2f9823d9-8f86-45fe-abe2-ed22fd39616e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.427Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {2f9823d9-8f86-45fe-abe2-ed22fd39616e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2f9823d9-8f86-45fe-abe2-ed22fd39616e b65ca541-b85f-4320-b7af-796f30f8da6e 7156164 1 2020-07-17 06:05:21 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:05:21 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-50526dab-9de3-4eb4-b0f1-542461f13d7f.vmdk 2f9823d9-8f86-45fe-abe2-ed22fd39616e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.427Z	DEBUG	syncer/util.go:46	FullSync: pv 2f9823d9-8f86-45fe-abe2-ed22fd39616e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.427Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.427Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.428Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "7f42a589-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000665260)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:21.460Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.343Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "7f42a589-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5fb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.343Z	DEBUG	volume/manager.go:244	Deleted task for 7f42a589-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.343Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "7f42a589-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c5fb", volumeID: "49413da1-751c-4787-9aae-c6bb67c07b04"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.343Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk" as container volume with ID: "49413da1-751c-4787-9aae-c6bb67c07b04"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.343Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk" with CNS. VolumeID: "49413da1-751c-4787-9aae-c6bb67c07b04"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.343Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {49413da1-751c-4787-9aae-c6bb67c07b04      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk 49413da1-751c-4787-9aae-c6bb67c07b04}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.344Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:49413da1-751c-4787-9aae-c6bb67c07b04 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk VolumeID:49413da1-751c-4787-9aae-c6bb67c07b04}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.350Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:05:34.351Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk", volumeID: "49413da1-751c-4787-9aae-c6bb67c07b04" mapping in the cache
2020-07-17T06:05:34.351Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:49413da1-751c-4787-9aae-c6bb67c07b04 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/49413da1-751c-4787-9aae-c6bb67c07b04 UID:ce6da120-75df-42d5-ba57-5b7e9f35f6b3 ResourceVersion:7156203 Generation:1 CreationTimestamp:2020-07-17 06:05:34 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:05:34 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk VolumeID:49413da1-751c-4787-9aae-c6bb67c07b04}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.351Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {49413da1-751c-4787-9aae-c6bb67c07b04   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/49413da1-751c-4787-9aae-c6bb67c07b04 ce6da120-75df-42d5-ba57-5b7e9f35f6b3 7156203 1 2020-07-17 06:05:34 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:05:34 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0c629b23-5d09-4ede-a293-b500f3525293.vmdk 49413da1-751c-4787-9aae-c6bb67c07b04}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.351Z	DEBUG	syncer/util.go:46	FullSync: pv 49413da1-751c-4787-9aae-c6bb67c07b04 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.351Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.352Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.352Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "86f6c1ad-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00030a690)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:34.361Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.698Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "86f6c1ad-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c60e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.699Z	DEBUG	volume/manager.go:244	Deleted task for 86f6c1ad-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.699Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "86f6c1ad-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c60e", volumeID: "c594e05a-39ea-4b3b-8178-ada3f3527510"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.699Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk" as container volume with ID: "c594e05a-39ea-4b3b-8178-ada3f3527510"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.699Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk" with CNS. VolumeID: "c594e05a-39ea-4b3b-8178-ada3f3527510"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.700Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {c594e05a-39ea-4b3b-8178-ada3f3527510      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk c594e05a-39ea-4b3b-8178-ada3f3527510}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.700Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c594e05a-39ea-4b3b-8178-ada3f3527510 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk VolumeID:c594e05a-39ea-4b3b-8178-ada3f3527510}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.707Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c594e05a-39ea-4b3b-8178-ada3f3527510 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c594e05a-39ea-4b3b-8178-ada3f3527510 UID:e7ba98da-ea86-4d79-8de5-4a0533b25550 ResourceVersion:7156241 Generation:1 CreationTimestamp:2020-07-17 06:05:46 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:05:46 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk VolumeID:c594e05a-39ea-4b3b-8178-ada3f3527510}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.707Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {c594e05a-39ea-4b3b-8178-ada3f3527510   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c594e05a-39ea-4b3b-8178-ada3f3527510 e7ba98da-ea86-4d79-8de5-4a0533b25550 7156241 1 2020-07-17 06:05:46 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:05:46 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk c594e05a-39ea-4b3b-8178-ada3f3527510}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.707Z	DEBUG	syncer/util.go:46	FullSync: pv c594e05a-39ea-4b3b-8178-ada3f3527510 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.707Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.708Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.708Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0009740e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "8e541ff9-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00051c390)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:46.709Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:05:46.709Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-66e1feb9-7900-4598-811c-538d881870f6.vmdk", volumeID: "c594e05a-39ea-4b3b-8178-ada3f3527510" mapping in the cache
2020-07-17T06:05:46.717Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.578Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "8e541ff9-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c649"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.579Z	DEBUG	volume/manager.go:244	Deleted task for 8e541ff9-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.579Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "8e541ff9-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c649", volumeID: "d7f2fb36-1e32-4753-b6c0-447193c33b85"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.579Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk" as container volume with ID: "d7f2fb36-1e32-4753-b6c0-447193c33b85"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.579Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk" with CNS. VolumeID: "d7f2fb36-1e32-4753-b6c0-447193c33b85"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.579Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {d7f2fb36-1e32-4753-b6c0-447193c33b85      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk d7f2fb36-1e32-4753-b6c0-447193c33b85}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.581Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d7f2fb36-1e32-4753-b6c0-447193c33b85 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk VolumeID:d7f2fb36-1e32-4753-b6c0-447193c33b85}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.599Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:05:59.600Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d7f2fb36-1e32-4753-b6c0-447193c33b85 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d7f2fb36-1e32-4753-b6c0-447193c33b85 UID:44c5e507-6b38-4619-935f-6dbcd231e224 ResourceVersion:7156284 Generation:1 CreationTimestamp:2020-07-17 06:05:59 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:05:59 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk VolumeID:d7f2fb36-1e32-4753-b6c0-447193c33b85}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.600Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {d7f2fb36-1e32-4753-b6c0-447193c33b85   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d7f2fb36-1e32-4753-b6c0-447193c33b85 44c5e507-6b38-4619-935f-6dbcd231e224 7156284 1 2020-07-17 06:05:59 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:05:59 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk d7f2fb36-1e32-4753-b6c0-447193c33b85}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.601Z	DEBUG	syncer/util.go:46	FullSync: pv d7f2fb36-1e32-4753-b6c0-447193c33b85 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.601Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.601Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.601Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "96037415-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007a3530)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:05:59.602Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-bc5d4ac4-b575-4a57-a306-7d96abf94669.vmdk", volumeID: "d7f2fb36-1e32-4753-b6c0-447193c33b85" mapping in the cache
2020-07-17T06:05:59.612Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.677Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "96037415-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c64c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.677Z	DEBUG	volume/manager.go:244	Deleted task for 96037415-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.677Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "96037415-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c64c", volumeID: "baf4e625-a7f8-420b-98c0-7a1b449290b6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.677Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk" as container volume with ID: "baf4e625-a7f8-420b-98c0-7a1b449290b6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.677Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk" with CNS. VolumeID: "baf4e625-a7f8-420b-98c0-7a1b449290b6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.677Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {baf4e625-a7f8-420b-98c0-7a1b449290b6      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk baf4e625-a7f8-420b-98c0-7a1b449290b6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.677Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:baf4e625-a7f8-420b-98c0-7a1b449290b6 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk VolumeID:baf4e625-a7f8-420b-98c0-7a1b449290b6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.687Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:baf4e625-a7f8-420b-98c0-7a1b449290b6 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/baf4e625-a7f8-420b-98c0-7a1b449290b6 UID:b5b1c4fd-7c2c-48e9-9419-40d28f42a9ea ResourceVersion:7156329 Generation:1 CreationTimestamp:2020-07-17 06:06:13 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:06:13 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk VolumeID:baf4e625-a7f8-420b-98c0-7a1b449290b6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.687Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {baf4e625-a7f8-420b-98c0-7a1b449290b6   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/baf4e625-a7f8-420b-98c0-7a1b449290b6 b5b1c4fd-7c2c-48e9-9419-40d28f42a9ea 7156329 1 2020-07-17 06:06:13 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:06:13 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk baf4e625-a7f8-420b-98c0-7a1b449290b6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.687Z	DEBUG	syncer/util.go:46	FullSync: pv baf4e625-a7f8-420b-98c0-7a1b449290b6 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.687Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.687Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:06:13.687Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.687Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2d74d3f9-1e27-490e-9433-90c5f2de573d.vmdk", volumeID: "baf4e625-a7f8-420b-98c0-7a1b449290b6" mapping in the cache
2020-07-17T06:06:13.687Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000974380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "9e68e297-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002e3f20)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:13.697Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.110Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "9e68e297-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c64e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.110Z	DEBUG	volume/manager.go:244	Deleted task for 9e68e297-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.110Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "9e68e297-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c64e", volumeID: "e6afb640-c144-45ce-b3a8-6585de4c952b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.110Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk" as container volume with ID: "e6afb640-c144-45ce-b3a8-6585de4c952b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.110Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk" with CNS. VolumeID: "e6afb640-c144-45ce-b3a8-6585de4c952b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.111Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {e6afb640-c144-45ce-b3a8-6585de4c952b      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk e6afb640-c144-45ce-b3a8-6585de4c952b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.111Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e6afb640-c144-45ce-b3a8-6585de4c952b GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk VolumeID:e6afb640-c144-45ce-b3a8-6585de4c952b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.118Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e6afb640-c144-45ce-b3a8-6585de4c952b GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e6afb640-c144-45ce-b3a8-6585de4c952b UID:37263699-6762-4bb2-88fd-3a2102a31cbd ResourceVersion:7156362 Generation:1 CreationTimestamp:2020-07-17 06:06:25 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:06:25 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk VolumeID:e6afb640-c144-45ce-b3a8-6585de4c952b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.118Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {e6afb640-c144-45ce-b3a8-6585de4c952b   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e6afb640-c144-45ce-b3a8-6585de4c952b 37263699-6762-4bb2-88fd-3a2102a31cbd 7156362 1 2020-07-17 06:06:25 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:06:25 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk e6afb640-c144-45ce-b3a8-6585de4c952b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.118Z	DEBUG	syncer/util.go:46	FullSync: pv e6afb640-c144-45ce-b3a8-6585de4c952b is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.118Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.118Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.118Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000974540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "a5391984-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004de750)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:25.119Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:06:25.119Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7d9b433a-f2a2-4468-9107-dc5356c09e2b.vmdk", volumeID: "e6afb640-c144-45ce-b3a8-6585de4c952b" mapping in the cache
2020-07-17T06:06:25.127Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.773Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "a5391984-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c666"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.773Z	DEBUG	volume/manager.go:244	Deleted task for a5391984-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.773Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "a5391984-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c666", volumeID: "b6f02b84-2142-46fc-9bc9-c67eefe04379"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.773Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk" as container volume with ID: "b6f02b84-2142-46fc-9bc9-c67eefe04379"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.773Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk" with CNS. VolumeID: "b6f02b84-2142-46fc-9bc9-c67eefe04379"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.774Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {b6f02b84-2142-46fc-9bc9-c67eefe04379      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk b6f02b84-2142-46fc-9bc9-c67eefe04379}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.774Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b6f02b84-2142-46fc-9bc9-c67eefe04379 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk VolumeID:b6f02b84-2142-46fc-9bc9-c67eefe04379}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.788Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:06:37.794Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk", volumeID: "b6f02b84-2142-46fc-9bc9-c67eefe04379" mapping in the cache
2020-07-17T06:06:37.802Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b6f02b84-2142-46fc-9bc9-c67eefe04379 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b6f02b84-2142-46fc-9bc9-c67eefe04379 UID:57a7bfcd-eb17-4fb3-9b25-986367f93e60 ResourceVersion:7156400 Generation:1 CreationTimestamp:2020-07-17 06:06:37 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:06:37 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk VolumeID:b6f02b84-2142-46fc-9bc9-c67eefe04379}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.802Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {b6f02b84-2142-46fc-9bc9-c67eefe04379   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b6f02b84-2142-46fc-9bc9-c67eefe04379 57a7bfcd-eb17-4fb3-9b25-986367f93e60 7156400 1 2020-07-17 06:06:37 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:06:37 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-dcccd496-af90-439e-a9a8-a459ac45790a.vmdk b6f02b84-2142-46fc-9bc9-c67eefe04379}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.802Z	DEBUG	syncer/util.go:46	FullSync: pv b6f02b84-2142-46fc-9bc9-c67eefe04379 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.802Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.803Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.804Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004687e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "acc8a02c-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dd2db0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:37.843Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.880Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "acc8a02c-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c672"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.880Z	DEBUG	volume/manager.go:244	Deleted task for acc8a02c-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.880Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "acc8a02c-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c672", volumeID: "6ce581ba-924b-40af-bbd5-ced391574849"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.880Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk" as container volume with ID: "6ce581ba-924b-40af-bbd5-ced391574849"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.880Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk" with CNS. VolumeID: "6ce581ba-924b-40af-bbd5-ced391574849"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.880Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {6ce581ba-924b-40af-bbd5-ced391574849      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk 6ce581ba-924b-40af-bbd5-ced391574849}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.880Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6ce581ba-924b-40af-bbd5-ced391574849 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk VolumeID:6ce581ba-924b-40af-bbd5-ced391574849}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.899Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6ce581ba-924b-40af-bbd5-ced391574849 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6ce581ba-924b-40af-bbd5-ced391574849 UID:28144331-4436-4915-a965-0776caf2200b ResourceVersion:7156446 Generation:1 CreationTimestamp:2020-07-17 06:06:51 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:06:51 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk VolumeID:6ce581ba-924b-40af-bbd5-ced391574849}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.900Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {6ce581ba-924b-40af-bbd5-ced391574849   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6ce581ba-924b-40af-bbd5-ced391574849 28144331-4436-4915-a965-0776caf2200b 7156446 1 2020-07-17 06:06:51 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:06:51 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk 6ce581ba-924b-40af-bbd5-ced391574849}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.900Z	DEBUG	syncer/util.go:46	FullSync: pv 6ce581ba-924b-40af-bbd5-ced391574849 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.901Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.902Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.902Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468a80)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "b52ff222-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0009cf350)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:06:51.904Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:06:51.905Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ddfbe3e9-6e66-4783-91db-148770e9f828.vmdk", volumeID: "6ce581ba-924b-40af-bbd5-ced391574849" mapping in the cache
2020-07-17T06:06:51.925Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.955Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "b52ff222-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6aa"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.955Z	DEBUG	volume/manager.go:244	Deleted task for b52ff222-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.955Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "b52ff222-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6aa", volumeID: "09f04849-61de-42c4-af1c-217581882caa"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.955Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk" as container volume with ID: "09f04849-61de-42c4-af1c-217581882caa"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.955Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk" with CNS. VolumeID: "09f04849-61de-42c4-af1c-217581882caa"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.955Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {09f04849-61de-42c4-af1c-217581882caa      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk 09f04849-61de-42c4-af1c-217581882caa}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.955Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:09f04849-61de-42c4-af1c-217581882caa GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk VolumeID:09f04849-61de-42c4-af1c-217581882caa}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.971Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:09f04849-61de-42c4-af1c-217581882caa GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/09f04849-61de-42c4-af1c-217581882caa UID:1f6a9cde-b447-4e68-90f4-bd4d68dc6e91 ResourceVersion:7156484 Generation:1 CreationTimestamp:2020-07-17 06:07:03 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:07:03 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk VolumeID:09f04849-61de-42c4-af1c-217581882caa}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.972Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {09f04849-61de-42c4-af1c-217581882caa   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/09f04849-61de-42c4-af1c-217581882caa 1f6a9cde-b447-4e68-90f4-bd4d68dc6e91 7156484 1 2020-07-17 06:07:03 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:07:03 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk 09f04849-61de-42c4-af1c-217581882caa}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.972Z	DEBUG	syncer/util.go:46	FullSync: pv 09f04849-61de-42c4-af1c-217581882caa is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.972Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.974Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.976Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "bc61fdad-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000deef90)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:03.973Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:07:03.976Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d95fba7a-ccca-4a55-97bf-a84c12778380.vmdk", volumeID: "09f04849-61de-42c4-af1c-217581882caa" mapping in the cache
2020-07-17T06:07:04.001Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.955Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "bc61fdad-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6ac"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.955Z	DEBUG	volume/manager.go:244	Deleted task for bc61fdad-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.955Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "bc61fdad-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6ac", volumeID: "59e4b50c-b292-4343-be1a-600b90f68e64"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.955Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk" as container volume with ID: "59e4b50c-b292-4343-be1a-600b90f68e64"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.955Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk" with CNS. VolumeID: "59e4b50c-b292-4343-be1a-600b90f68e64"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.955Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {59e4b50c-b292-4343-be1a-600b90f68e64      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk 59e4b50c-b292-4343-be1a-600b90f68e64}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.955Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:59e4b50c-b292-4343-be1a-600b90f68e64 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk VolumeID:59e4b50c-b292-4343-be1a-600b90f68e64}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.965Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:07:18.968Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:59e4b50c-b292-4343-be1a-600b90f68e64 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/59e4b50c-b292-4343-be1a-600b90f68e64 UID:6339f66f-e551-495b-aa6b-e27510d29d51 ResourceVersion:7156534 Generation:1 CreationTimestamp:2020-07-17 06:07:18 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:07:18 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk VolumeID:59e4b50c-b292-4343-be1a-600b90f68e64}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.968Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {59e4b50c-b292-4343-be1a-600b90f68e64   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/59e4b50c-b292-4343-be1a-600b90f68e64 6339f66f-e551-495b-aa6b-e27510d29d51 7156534 1 2020-07-17 06:07:18 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:07:18 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk 59e4b50c-b292-4343-be1a-600b90f68e64}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.968Z	DEBUG	syncer/util.go:46	FullSync: pv 59e4b50c-b292-4343-be1a-600b90f68e64 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.968Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.968Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.968Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000964000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "c551ea93-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dd3410)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:18.982Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d3f0b083-8176-4250-9a1b-db0d5b191230.vmdk", volumeID: "59e4b50c-b292-4343-be1a-600b90f68e64" mapping in the cache
2020-07-17T06:07:18.988Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.132Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "c551ea93-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6af"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.132Z	DEBUG	volume/manager.go:244	Deleted task for c551ea93-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.132Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "c551ea93-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6af", volumeID: "cfe51a00-91fc-4e97-a971-1abddd69436d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.132Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk" as container volume with ID: "cfe51a00-91fc-4e97-a971-1abddd69436d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.132Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk" with CNS. VolumeID: "cfe51a00-91fc-4e97-a971-1abddd69436d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.132Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {cfe51a00-91fc-4e97-a971-1abddd69436d      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk cfe51a00-91fc-4e97-a971-1abddd69436d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.132Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:cfe51a00-91fc-4e97-a971-1abddd69436d GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk VolumeID:cfe51a00-91fc-4e97-a971-1abddd69436d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.140Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:07:32.141Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk", volumeID: "cfe51a00-91fc-4e97-a971-1abddd69436d" mapping in the cache
2020-07-17T06:07:32.144Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:cfe51a00-91fc-4e97-a971-1abddd69436d GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/cfe51a00-91fc-4e97-a971-1abddd69436d UID:3cd157be-3ff2-40c1-a2f8-a710a8b144b7 ResourceVersion:7156574 Generation:1 CreationTimestamp:2020-07-17 06:07:32 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:07:32 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk VolumeID:cfe51a00-91fc-4e97-a971-1abddd69436d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.144Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {cfe51a00-91fc-4e97-a971-1abddd69436d   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/cfe51a00-91fc-4e97-a971-1abddd69436d 3cd157be-3ff2-40c1-a2f8-a710a8b144b7 7156574 1 2020-07-17 06:07:32 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:07:32 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-baeb47a2-58f1-421b-bc6b-e17b0cc5b3a2.vmdk cfe51a00-91fc-4e97-a971-1abddd69436d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.144Z	DEBUG	syncer/util.go:46	FullSync: pv cfe51a00-91fc-4e97-a971-1abddd69436d is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.144Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.144Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.144Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "cd2c664f-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004de510)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:32.171Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.575Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "cd2c664f-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6b2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.576Z	DEBUG	volume/manager.go:244	Deleted task for cd2c664f-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.577Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "cd2c664f-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c6b2", volumeID: "6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.578Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk" as container volume with ID: "6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.578Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk" with CNS. VolumeID: "6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.579Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk 6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.579Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk VolumeID:6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.593Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:07:43.593Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk", volumeID: "6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2" mapping in the cache
2020-07-17T06:07:43.595Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2 UID:a10ed0b1-ca4c-42da-9b93-61b2f772150c ResourceVersion:7156611 Generation:1 CreationTimestamp:2020-07-17 06:07:43 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:07:43 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk VolumeID:6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.595Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2 a10ed0b1-ca4c-42da-9b93-61b2f772150c 7156611 1 2020-07-17 06:07:43 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:07:43 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2970338e-3695-4245-ae51-6cf645529d6c.vmdk 6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.595Z	DEBUG	syncer/util.go:46	FullSync: pv 6321a6f3-770e-44ee-a8f8-ac4bdf95f1f2 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.596Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.596Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.597Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "d3ffe9cd-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004798c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:43.617Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.736Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "d3ffe9cd-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c79d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.737Z	DEBUG	volume/manager.go:244	Deleted task for d3ffe9cd-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.737Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "d3ffe9cd-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c79d", volumeID: "90b49caf-b68c-4ce4-8bde-c4627087343e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.737Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk" as container volume with ID: "90b49caf-b68c-4ce4-8bde-c4627087343e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.737Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk" with CNS. VolumeID: "90b49caf-b68c-4ce4-8bde-c4627087343e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.737Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {90b49caf-b68c-4ce4-8bde-c4627087343e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk 90b49caf-b68c-4ce4-8bde-c4627087343e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.737Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:90b49caf-b68c-4ce4-8bde-c4627087343e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk VolumeID:90b49caf-b68c-4ce4-8bde-c4627087343e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.748Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:07:57.748Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk", volumeID: "90b49caf-b68c-4ce4-8bde-c4627087343e" mapping in the cache
2020-07-17T06:07:57.748Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:90b49caf-b68c-4ce4-8bde-c4627087343e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/90b49caf-b68c-4ce4-8bde-c4627087343e UID:0d246ad7-449e-4f3f-9ce4-01f68cbf63c1 ResourceVersion:7156655 Generation:1 CreationTimestamp:2020-07-17 06:07:57 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:07:57 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk VolumeID:90b49caf-b68c-4ce4-8bde-c4627087343e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.748Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {90b49caf-b68c-4ce4-8bde-c4627087343e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/90b49caf-b68c-4ce4-8bde-c4627087343e 0d246ad7-449e-4f3f-9ce4-01f68cbf63c1 7156655 1 2020-07-17 06:07:57 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:07:57 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-1e47fa7e-5b3d-4747-bd81-6dd5df1e556f.vmdk 90b49caf-b68c-4ce4-8bde-c4627087343e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.748Z	DEBUG	syncer/util.go:46	FullSync: pv 90b49caf-b68c-4ce4-8bde-c4627087343e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.748Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.748Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.749Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004687e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "dc6f6116-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007a3710)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:07:57.764Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.020Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "dc6f6116-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c7cb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.020Z	DEBUG	volume/manager.go:244	Deleted task for dc6f6116-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.020Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "dc6f6116-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c7cb", volumeID: "7e5d5bb8-3aa1-40ec-a361-863b5f1503a1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.020Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk" as container volume with ID: "7e5d5bb8-3aa1-40ec-a361-863b5f1503a1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.020Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk" with CNS. VolumeID: "7e5d5bb8-3aa1-40ec-a361-863b5f1503a1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.020Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {7e5d5bb8-3aa1-40ec-a361-863b5f1503a1      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk 7e5d5bb8-3aa1-40ec-a361-863b5f1503a1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.020Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7e5d5bb8-3aa1-40ec-a361-863b5f1503a1 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk VolumeID:7e5d5bb8-3aa1-40ec-a361-863b5f1503a1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.033Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:08:13.034Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk", volumeID: "7e5d5bb8-3aa1-40ec-a361-863b5f1503a1" mapping in the cache
2020-07-17T06:08:13.035Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7e5d5bb8-3aa1-40ec-a361-863b5f1503a1 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7e5d5bb8-3aa1-40ec-a361-863b5f1503a1 UID:9f34c9e2-2c41-4cfe-888c-94eb48b7f8cf ResourceVersion:7156699 Generation:1 CreationTimestamp:2020-07-17 06:08:13 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:08:13 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk VolumeID:7e5d5bb8-3aa1-40ec-a361-863b5f1503a1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.035Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {7e5d5bb8-3aa1-40ec-a361-863b5f1503a1   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7e5d5bb8-3aa1-40ec-a361-863b5f1503a1 9f34c9e2-2c41-4cfe-888c-94eb48b7f8cf 7156699 1 2020-07-17 06:08:13 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:08:13 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8e2e33c1-c11b-48b9-a77e-ea3feb012e16.vmdk 7e5d5bb8-3aa1-40ec-a361-863b5f1503a1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.035Z	DEBUG	syncer/util.go:46	FullSync: pv 7e5d5bb8-3aa1-40ec-a361-863b5f1503a1 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.036Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.036Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.039Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0009640e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "e58c13a3-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00051d890)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:13.073Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.689Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "e58c13a3-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c87e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.689Z	DEBUG	volume/manager.go:244	Deleted task for e58c13a3-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.689Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "e58c13a3-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c87e", volumeID: "f4ebe2f0-105b-4ff4-b23c-8f640bce5709"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.689Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk" as container volume with ID: "f4ebe2f0-105b-4ff4-b23c-8f640bce5709"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.689Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk" with CNS. VolumeID: "f4ebe2f0-105b-4ff4-b23c-8f640bce5709"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.689Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {f4ebe2f0-105b-4ff4-b23c-8f640bce5709      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk f4ebe2f0-105b-4ff4-b23c-8f640bce5709}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.689Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f4ebe2f0-105b-4ff4-b23c-8f640bce5709 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk VolumeID:f4ebe2f0-105b-4ff4-b23c-8f640bce5709}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.713Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f4ebe2f0-105b-4ff4-b23c-8f640bce5709 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f4ebe2f0-105b-4ff4-b23c-8f640bce5709 UID:e82cf446-eb89-4f3b-b4f1-e4e487a0a510 ResourceVersion:7156736 Generation:1 CreationTimestamp:2020-07-17 06:08:24 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:08:24 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk VolumeID:f4ebe2f0-105b-4ff4-b23c-8f640bce5709}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.713Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {f4ebe2f0-105b-4ff4-b23c-8f640bce5709   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f4ebe2f0-105b-4ff4-b23c-8f640bce5709 e82cf446-eb89-4f3b-b4f1-e4e487a0a510 7156736 1 2020-07-17 06:08:24 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:08:24 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk f4ebe2f0-105b-4ff4-b23c-8f640bce5709}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.713Z	DEBUG	syncer/util.go:46	FullSync: pv f4ebe2f0-105b-4ff4-b23c-8f640bce5709 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.713Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.713Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.713Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0009642a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "ec81d245-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00030acc0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:24.708Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:08:24.725Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2c3369d5-1461-4a8e-8843-eb6a050ce2ae.vmdk", volumeID: "f4ebe2f0-105b-4ff4-b23c-8f640bce5709" mapping in the cache
2020-07-17T06:08:24.742Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:43.997Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "ec81d245-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c8f4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:43.997Z	DEBUG	volume/manager.go:244	Deleted task for ec81d245-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:43.997Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "ec81d245-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168c8f4", volumeID: "7c62a540-41b7-4149-838e-a40eaffe210c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:43.997Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk" as container volume with ID: "7c62a540-41b7-4149-838e-a40eaffe210c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:43.997Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk" with CNS. VolumeID: "7c62a540-41b7-4149-838e-a40eaffe210c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:43.997Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {7c62a540-41b7-4149-838e-a40eaffe210c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk 7c62a540-41b7-4149-838e-a40eaffe210c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:43.997Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7c62a540-41b7-4149-838e-a40eaffe210c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk VolumeID:7c62a540-41b7-4149-838e-a40eaffe210c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:44.100Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7c62a540-41b7-4149-838e-a40eaffe210c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7c62a540-41b7-4149-838e-a40eaffe210c UID:418424d6-8340-4b5d-9a22-8f89c96d9889 ResourceVersion:7156795 Generation:1 CreationTimestamp:2020-07-17 06:08:44 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:08:44 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk VolumeID:7c62a540-41b7-4149-838e-a40eaffe210c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:44.100Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {7c62a540-41b7-4149-838e-a40eaffe210c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7c62a540-41b7-4149-838e-a40eaffe210c 418424d6-8340-4b5d-9a22-8f89c96d9889 7156795 1 2020-07-17 06:08:44 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:08:44 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk 7c62a540-41b7-4149-838e-a40eaffe210c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:44.100Z	DEBUG	syncer/util.go:46	FullSync: pv 7c62a540-41b7-4149-838e-a40eaffe210c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:44.100Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:44.109Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:44.109Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468c40)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "f8116c81-c7f3-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00061e150)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:44.109Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:08:44.110Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c614a19e-d151-4839-bb97-a9d35413a8ea.vmdk", volumeID: "7c62a540-41b7-4149-838e-a40eaffe210c" mapping in the cache
2020-07-17T06:08:44.135Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.790Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "f8116c81-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168ca2d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.790Z	DEBUG	volume/manager.go:244	Deleted task for f8116c81-c7f3-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.790Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "f8116c81-c7f3-11ea-8f7c-9695ff1c5b6f", opId: "9168ca2d", volumeID: "de48237a-3d06-4a80-a230-e05fa651612e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.790Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk" as container volume with ID: "de48237a-3d06-4a80-a230-e05fa651612e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.790Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk" with CNS. VolumeID: "de48237a-3d06-4a80-a230-e05fa651612e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.790Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {de48237a-3d06-4a80-a230-e05fa651612e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk de48237a-3d06-4a80-a230-e05fa651612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.790Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:de48237a-3d06-4a80-a230-e05fa651612e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk VolumeID:de48237a-3d06-4a80-a230-e05fa651612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.801Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:08:58.802Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk", volumeID: "de48237a-3d06-4a80-a230-e05fa651612e" mapping in the cache
2020-07-17T06:08:58.803Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:de48237a-3d06-4a80-a230-e05fa651612e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/de48237a-3d06-4a80-a230-e05fa651612e UID:d706643b-faec-4675-8817-b581d70372d6 ResourceVersion:7156842 Generation:1 CreationTimestamp:2020-07-17 06:08:58 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:08:58 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk VolumeID:de48237a-3d06-4a80-a230-e05fa651612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.803Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {de48237a-3d06-4a80-a230-e05fa651612e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/de48237a-3d06-4a80-a230-e05fa651612e d706643b-faec-4675-8817-b581d70372d6 7156842 1 2020-07-17 06:08:58 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:08:58 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-265d8ae6-334b-4b9d-abb2-747456cf1960.vmdk de48237a-3d06-4a80-a230-e05fa651612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.804Z	DEBUG	syncer/util.go:46	FullSync: pv de48237a-3d06-4a80-a230-e05fa651612e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.804Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.804Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.805Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "00d3c005-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000664a80)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:08:58.826Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.467Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "00d3c005-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cb66"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.467Z	DEBUG	volume/manager.go:244	Deleted task for 00d3c005-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.467Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "00d3c005-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cb66", volumeID: "5012d7fd-6aae-4b8f-a767-353a06ad8c14"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.467Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk" as container volume with ID: "5012d7fd-6aae-4b8f-a767-353a06ad8c14"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.467Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk" with CNS. VolumeID: "5012d7fd-6aae-4b8f-a767-353a06ad8c14"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.467Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {5012d7fd-6aae-4b8f-a767-353a06ad8c14      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk 5012d7fd-6aae-4b8f-a767-353a06ad8c14}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.467Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:5012d7fd-6aae-4b8f-a767-353a06ad8c14 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk VolumeID:5012d7fd-6aae-4b8f-a767-353a06ad8c14}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.476Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:09:13.476Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:5012d7fd-6aae-4b8f-a767-353a06ad8c14 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/5012d7fd-6aae-4b8f-a767-353a06ad8c14 UID:caa5b9bd-d0b5-46bd-bd60-9f24d15c0149 ResourceVersion:7156889 Generation:1 CreationTimestamp:2020-07-17 06:09:13 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:09:13 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk VolumeID:5012d7fd-6aae-4b8f-a767-353a06ad8c14}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.476Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {5012d7fd-6aae-4b8f-a767-353a06ad8c14   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/5012d7fd-6aae-4b8f-a767-353a06ad8c14 caa5b9bd-d0b5-46bd-bd60-9f24d15c0149 7156889 1 2020-07-17 06:09:13 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:09:13 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk 5012d7fd-6aae-4b8f-a767-353a06ad8c14}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.476Z	DEBUG	syncer/util.go:46	FullSync: pv 5012d7fd-6aae-4b8f-a767-353a06ad8c14 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.476Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.477Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.477Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004682a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "099292f3-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00030ac90)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:13.478Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-13a4811e-5389-4e3c-90de-1ec9210ca144.vmdk", volumeID: "5012d7fd-6aae-4b8f-a767-353a06ad8c14" mapping in the cache
2020-07-17T06:09:13.503Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.653Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "099292f3-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ccfa"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.653Z	DEBUG	volume/manager.go:244	Deleted task for 099292f3-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.653Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "099292f3-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ccfa", volumeID: "d122dde8-33ac-463e-bc58-dc0e453b4899"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.653Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk" as container volume with ID: "d122dde8-33ac-463e-bc58-dc0e453b4899"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.653Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk" with CNS. VolumeID: "d122dde8-33ac-463e-bc58-dc0e453b4899"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.653Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {d122dde8-33ac-463e-bc58-dc0e453b4899      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk d122dde8-33ac-463e-bc58-dc0e453b4899}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.653Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d122dde8-33ac-463e-bc58-dc0e453b4899 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk VolumeID:d122dde8-33ac-463e-bc58-dc0e453b4899}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.666Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:09:26.666Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk", volumeID: "d122dde8-33ac-463e-bc58-dc0e453b4899" mapping in the cache
2020-07-17T06:09:26.669Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d122dde8-33ac-463e-bc58-dc0e453b4899 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d122dde8-33ac-463e-bc58-dc0e453b4899 UID:2cd7fceb-5bfd-4f34-981c-6c59d1d342e5 ResourceVersion:7156929 Generation:1 CreationTimestamp:2020-07-17 06:09:26 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:09:26 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk VolumeID:d122dde8-33ac-463e-bc58-dc0e453b4899}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.669Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {d122dde8-33ac-463e-bc58-dc0e453b4899   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d122dde8-33ac-463e-bc58-dc0e453b4899 2cd7fceb-5bfd-4f34-981c-6c59d1d342e5 7156929 1 2020-07-17 06:09:26 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:09:26 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b72e0cd9-4491-4c95-b327-c898eaedf534.vmdk d122dde8-33ac-463e-bc58-dc0e453b4899}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.669Z	DEBUG	syncer/util.go:46	FullSync: pv d122dde8-33ac-463e-bc58-dc0e453b4899 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.669Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.670Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.670Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "116faa0a-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0003fd740)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:26.689Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.727Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "116faa0a-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cd82"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.727Z	DEBUG	volume/manager.go:244	Deleted task for 116faa0a-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.727Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "116faa0a-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cd82", volumeID: "cc73ce20-18e0-4dcd-aaac-f7127da3fad2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.727Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk" as container volume with ID: "cc73ce20-18e0-4dcd-aaac-f7127da3fad2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.727Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk" with CNS. VolumeID: "cc73ce20-18e0-4dcd-aaac-f7127da3fad2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.727Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {cc73ce20-18e0-4dcd-aaac-f7127da3fad2      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk cc73ce20-18e0-4dcd-aaac-f7127da3fad2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.727Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:cc73ce20-18e0-4dcd-aaac-f7127da3fad2 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk VolumeID:cc73ce20-18e0-4dcd-aaac-f7127da3fad2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.736Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:09:45.737Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk", volumeID: "cc73ce20-18e0-4dcd-aaac-f7127da3fad2" mapping in the cache
2020-07-17T06:09:45.737Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:cc73ce20-18e0-4dcd-aaac-f7127da3fad2 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/cc73ce20-18e0-4dcd-aaac-f7127da3fad2 UID:3e90d6f5-fda3-4bc9-b634-bce0c0433767 ResourceVersion:7156991 Generation:1 CreationTimestamp:2020-07-17 06:09:45 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:09:45 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk VolumeID:cc73ce20-18e0-4dcd-aaac-f7127da3fad2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.737Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {cc73ce20-18e0-4dcd-aaac-f7127da3fad2   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/cc73ce20-18e0-4dcd-aaac-f7127da3fad2 3e90d6f5-fda3-4bc9-b634-bce0c0433767 7156991 1 2020-07-17 06:09:45 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:09:45 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c09a4f0b-3c31-470e-beba-da8b3f9e0040.vmdk cc73ce20-18e0-4dcd-aaac-f7127da3fad2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.737Z	DEBUG	syncer/util.go:46	FullSync: pv cc73ce20-18e0-4dcd-aaac-f7127da3fad2 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.737Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.738Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.738Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "1ccd36af-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007a3b30)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:09:45.758Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.476Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "1ccd36af-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cdca"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.476Z	DEBUG	volume/manager.go:244	Deleted task for 1ccd36af-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.476Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "1ccd36af-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cdca", volumeID: "ed08a9be-c6d8-4706-b5dc-3c3f542ddc80"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.476Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk" as container volume with ID: "ed08a9be-c6d8-4706-b5dc-3c3f542ddc80"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.476Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk" with CNS. VolumeID: "ed08a9be-c6d8-4706-b5dc-3c3f542ddc80"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.476Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {ed08a9be-c6d8-4706-b5dc-3c3f542ddc80      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk ed08a9be-c6d8-4706-b5dc-3c3f542ddc80}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.476Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:ed08a9be-c6d8-4706-b5dc-3c3f542ddc80 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk VolumeID:ed08a9be-c6d8-4706-b5dc-3c3f542ddc80}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.489Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:10:03.505Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk", volumeID: "ed08a9be-c6d8-4706-b5dc-3c3f542ddc80" mapping in the cache
2020-07-17T06:10:03.504Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:ed08a9be-c6d8-4706-b5dc-3c3f542ddc80 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/ed08a9be-c6d8-4706-b5dc-3c3f542ddc80 UID:fa1b6e3e-b033-40c4-815e-2eb11148d31e ResourceVersion:7157044 Generation:1 CreationTimestamp:2020-07-17 06:10:03 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:10:03 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk VolumeID:ed08a9be-c6d8-4706-b5dc-3c3f542ddc80}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.510Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {ed08a9be-c6d8-4706-b5dc-3c3f542ddc80   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/ed08a9be-c6d8-4706-b5dc-3c3f542ddc80 fa1b6e3e-b033-40c4-815e-2eb11148d31e 7157044 1 2020-07-17 06:10:03 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:10:03 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-27c25023-ffdd-4cb1-beff-fa28d09c8984.vmdk ed08a9be-c6d8-4706-b5dc-3c3f542ddc80}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.511Z	DEBUG	syncer/util.go:46	FullSync: pv ed08a9be-c6d8-4706-b5dc-3c3f542ddc80 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.511Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.511Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.512Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f00e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "27654786-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000479a10)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:03.531Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.090Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "27654786-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cded"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.090Z	DEBUG	volume/manager.go:244	Deleted task for 27654786-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.090Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "27654786-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cded", volumeID: "08e739dd-19a2-41f9-8e2c-eacd6cca205d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.093Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk" as container volume with ID: "08e739dd-19a2-41f9-8e2c-eacd6cca205d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.093Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk" with CNS. VolumeID: "08e739dd-19a2-41f9-8e2c-eacd6cca205d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.094Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {08e739dd-19a2-41f9-8e2c-eacd6cca205d      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk 08e739dd-19a2-41f9-8e2c-eacd6cca205d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.094Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:08e739dd-19a2-41f9-8e2c-eacd6cca205d GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk VolumeID:08e739dd-19a2-41f9-8e2c-eacd6cca205d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.111Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:10:15.111Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk", volumeID: "08e739dd-19a2-41f9-8e2c-eacd6cca205d" mapping in the cache
2020-07-17T06:10:15.114Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:08e739dd-19a2-41f9-8e2c-eacd6cca205d GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/08e739dd-19a2-41f9-8e2c-eacd6cca205d UID:940607ef-28cc-414e-8914-f83ec690cf6d ResourceVersion:7157082 Generation:1 CreationTimestamp:2020-07-17 06:10:15 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:10:15 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk VolumeID:08e739dd-19a2-41f9-8e2c-eacd6cca205d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.114Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {08e739dd-19a2-41f9-8e2c-eacd6cca205d   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/08e739dd-19a2-41f9-8e2c-eacd6cca205d 940607ef-28cc-414e-8914-f83ec690cf6d 7157082 1 2020-07-17 06:10:15 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:10:15 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-82ba756e-21a1-4df7-8f7b-e71ebab42483.vmdk 08e739dd-19a2-41f9-8e2c-eacd6cca205d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.114Z	DEBUG	syncer/util.go:46	FullSync: pv 08e739dd-19a2-41f9-8e2c-eacd6cca205d is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.114Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.114Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.114Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004687e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "2e4fa64d-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004df800)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:15.124Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.479Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "2e4fa64d-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cdef"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.479Z	DEBUG	volume/manager.go:244	Deleted task for 2e4fa64d-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.479Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "2e4fa64d-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cdef", volumeID: "00e5e339-65e2-4918-9fae-cb576680d129"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.479Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk" as container volume with ID: "00e5e339-65e2-4918-9fae-cb576680d129"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.480Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk" with CNS. VolumeID: "00e5e339-65e2-4918-9fae-cb576680d129"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.481Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {00e5e339-65e2-4918-9fae-cb576680d129      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk 00e5e339-65e2-4918-9fae-cb576680d129}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.482Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:00e5e339-65e2-4918-9fae-cb576680d129 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk VolumeID:00e5e339-65e2-4918-9fae-cb576680d129}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.496Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:00e5e339-65e2-4918-9fae-cb576680d129 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/00e5e339-65e2-4918-9fae-cb576680d129 UID:a620b227-0e7a-41af-a848-7604eb9fe4a6 ResourceVersion:7157125 Generation:1 CreationTimestamp:2020-07-17 06:10:28 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:10:28 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk VolumeID:00e5e339-65e2-4918-9fae-cb576680d129}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.496Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {00e5e339-65e2-4918-9fae-cb576680d129   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/00e5e339-65e2-4918-9fae-cb576680d129 a620b227-0e7a-41af-a848-7604eb9fe4a6 7157125 1 2020-07-17 06:10:28 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:10:28 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk 00e5e339-65e2-4918-9fae-cb576680d129}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.496Z	DEBUG	syncer/util.go:46	FullSync: pv 00e5e339-65e2-4918-9fae-cb576680d129 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.496Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.499Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.500Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f02a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "364a22f7-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00089a210)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:28.503Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:10:28.503Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-44d520bf-f6be-429e-a736-47302203ee3d.vmdk", volumeID: "00e5e339-65e2-4918-9fae-cb576680d129" mapping in the cache
2020-07-17T06:10:28.529Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.267Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "364a22f7-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce0f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.269Z	DEBUG	volume/manager.go:244	Deleted task for 364a22f7-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.269Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "364a22f7-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce0f", volumeID: "627f80e6-ba7a-4eb0-a5f8-ac965586e6f1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.270Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk" as container volume with ID: "627f80e6-ba7a-4eb0-a5f8-ac965586e6f1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.270Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk" with CNS. VolumeID: "627f80e6-ba7a-4eb0-a5f8-ac965586e6f1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.271Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {627f80e6-ba7a-4eb0-a5f8-ac965586e6f1      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk 627f80e6-ba7a-4eb0-a5f8-ac965586e6f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.272Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:627f80e6-ba7a-4eb0-a5f8-ac965586e6f1 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk VolumeID:627f80e6-ba7a-4eb0-a5f8-ac965586e6f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.295Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:627f80e6-ba7a-4eb0-a5f8-ac965586e6f1 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/627f80e6-ba7a-4eb0-a5f8-ac965586e6f1 UID:b64b783d-895c-4156-8966-bfc184f183b4 ResourceVersion:7157160 Generation:1 CreationTimestamp:2020-07-17 06:10:40 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:10:40 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk VolumeID:627f80e6-ba7a-4eb0-a5f8-ac965586e6f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.295Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {627f80e6-ba7a-4eb0-a5f8-ac965586e6f1   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/627f80e6-ba7a-4eb0-a5f8-ac965586e6f1 b64b783d-895c-4156-8966-bfc184f183b4 7157160 1 2020-07-17 06:10:40 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:10:40 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk 627f80e6-ba7a-4eb0-a5f8-ac965586e6f1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.295Z	DEBUG	syncer/util.go:46	FullSync: pv 627f80e6-ba7a-4eb0-a5f8-ac965586e6f1 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.295Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.296Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.296Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468a80)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "3d5209ef-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000988360)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:40.303Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:10:40.306Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b64a01e1-8cb1-425f-a0cd-d2d903cecaf1.vmdk", volumeID: "627f80e6-ba7a-4eb0-a5f8-ac965586e6f1" mapping in the cache
2020-07-17T06:10:40.326Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.037Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "3d5209ef-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce14"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.038Z	DEBUG	volume/manager.go:244	Deleted task for 3d5209ef-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.038Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "3d5209ef-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce14", volumeID: "64dcfc39-91fc-46e5-9114-caa54b5ef99d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.038Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk" as container volume with ID: "64dcfc39-91fc-46e5-9114-caa54b5ef99d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.038Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk" with CNS. VolumeID: "64dcfc39-91fc-46e5-9114-caa54b5ef99d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.039Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {64dcfc39-91fc-46e5-9114-caa54b5ef99d      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk 64dcfc39-91fc-46e5-9114-caa54b5ef99d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.039Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:64dcfc39-91fc-46e5-9114-caa54b5ef99d GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk VolumeID:64dcfc39-91fc-46e5-9114-caa54b5ef99d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.057Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:10:56.064Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk", volumeID: "64dcfc39-91fc-46e5-9114-caa54b5ef99d" mapping in the cache
2020-07-17T06:10:56.065Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:64dcfc39-91fc-46e5-9114-caa54b5ef99d GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/64dcfc39-91fc-46e5-9114-caa54b5ef99d UID:5fde6439-77dc-466c-985f-a3f8f45ad692 ResourceVersion:7157209 Generation:1 CreationTimestamp:2020-07-17 06:10:56 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:10:56 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk VolumeID:64dcfc39-91fc-46e5-9114-caa54b5ef99d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.066Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {64dcfc39-91fc-46e5-9114-caa54b5ef99d   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/64dcfc39-91fc-46e5-9114-caa54b5ef99d 5fde6439-77dc-466c-985f-a3f8f45ad692 7157209 1 2020-07-17 06:10:56 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:10:56 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-4f3c4823-4de3-4aa4-b59e-452d9f6db3b4.vmdk 64dcfc39-91fc-46e5-9114-caa54b5ef99d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.066Z	DEBUG	syncer/util.go:46	FullSync: pv 64dcfc39-91fc-46e5-9114-caa54b5ef99d is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.066Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.066Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.066Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f0460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "46b86f40-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0011812c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:10:56.085Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.200Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "46b86f40-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce59"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.200Z	DEBUG	volume/manager.go:244	Deleted task for 46b86f40-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.200Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "46b86f40-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce59", volumeID: "9458f3f7-15ab-4c7d-b37b-70c9610818e0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.201Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk" as container volume with ID: "9458f3f7-15ab-4c7d-b37b-70c9610818e0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.201Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk" with CNS. VolumeID: "9458f3f7-15ab-4c7d-b37b-70c9610818e0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.202Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {9458f3f7-15ab-4c7d-b37b-70c9610818e0      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk 9458f3f7-15ab-4c7d-b37b-70c9610818e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.203Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:9458f3f7-15ab-4c7d-b37b-70c9610818e0 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk VolumeID:9458f3f7-15ab-4c7d-b37b-70c9610818e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.213Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:11:11.213Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk", volumeID: "9458f3f7-15ab-4c7d-b37b-70c9610818e0" mapping in the cache
2020-07-17T06:11:11.218Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:9458f3f7-15ab-4c7d-b37b-70c9610818e0 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/9458f3f7-15ab-4c7d-b37b-70c9610818e0 UID:ec2af1e3-32e5-4d69-9653-356867810b45 ResourceVersion:7157259 Generation:1 CreationTimestamp:2020-07-17 06:11:11 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:11:11 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk VolumeID:9458f3f7-15ab-4c7d-b37b-70c9610818e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.218Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {9458f3f7-15ab-4c7d-b37b-70c9610818e0   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/9458f3f7-15ab-4c7d-b37b-70c9610818e0 ec2af1e3-32e5-4d69-9653-356867810b45 7157259 1 2020-07-17 06:11:11 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:11:11 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-34d69e65-ca00-445b-b820-c1d6d7d4b6e1.vmdk 9458f3f7-15ab-4c7d-b37b-70c9610818e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.218Z	DEBUG	syncer/util.go:46	FullSync: pv 9458f3f7-15ab-4c7d-b37b-70c9610818e0 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.218Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.218Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.219Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f0000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "4fc08cce-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000989440)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:11.235Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.893Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "4fc08cce-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce5d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.893Z	DEBUG	volume/manager.go:244	Deleted task for 4fc08cce-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.893Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "4fc08cce-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce5d", volumeID: "8b37fc63-e38d-4aaf-b11d-b990c7695e0f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.893Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk" as container volume with ID: "8b37fc63-e38d-4aaf-b11d-b990c7695e0f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.893Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk" with CNS. VolumeID: "8b37fc63-e38d-4aaf-b11d-b990c7695e0f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.893Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {8b37fc63-e38d-4aaf-b11d-b990c7695e0f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk 8b37fc63-e38d-4aaf-b11d-b990c7695e0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.893Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:8b37fc63-e38d-4aaf-b11d-b990c7695e0f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk VolumeID:8b37fc63-e38d-4aaf-b11d-b990c7695e0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.909Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:8b37fc63-e38d-4aaf-b11d-b990c7695e0f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/8b37fc63-e38d-4aaf-b11d-b990c7695e0f UID:224850cb-71dd-4779-bf26-4b9b292adbcc ResourceVersion:7157317 Generation:1 CreationTimestamp:2020-07-17 06:11:29 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:11:29 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk VolumeID:8b37fc63-e38d-4aaf-b11d-b990c7695e0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.912Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {8b37fc63-e38d-4aaf-b11d-b990c7695e0f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/8b37fc63-e38d-4aaf-b11d-b990c7695e0f 224850cb-71dd-4779-bf26-4b9b292adbcc 7157317 1 2020-07-17 06:11:29 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:11:29 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk 8b37fc63-e38d-4aaf-b11d-b990c7695e0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.912Z	DEBUG	syncer/util.go:46	FullSync: pv 8b37fc63-e38d-4aaf-b11d-b990c7695e0f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.913Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.913Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.914Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f01c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "5ae51b90-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dbe330)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:29.917Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:11:29.917Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-12a3cc91-be1f-477e-85d6-1429b3aec9d3.vmdk", volumeID: "8b37fc63-e38d-4aaf-b11d-b990c7695e0f" mapping in the cache
2020-07-17T06:11:29.927Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.692Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "5ae51b90-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce60"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.692Z	DEBUG	volume/manager.go:244	Deleted task for 5ae51b90-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.692Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "5ae51b90-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce60", volumeID: "db1bccba-dc3d-4f31-b08f-1cd5669847d5"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.692Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk" as container volume with ID: "db1bccba-dc3d-4f31-b08f-1cd5669847d5"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.692Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk" with CNS. VolumeID: "db1bccba-dc3d-4f31-b08f-1cd5669847d5"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.692Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {db1bccba-dc3d-4f31-b08f-1cd5669847d5      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk db1bccba-dc3d-4f31-b08f-1cd5669847d5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.692Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:db1bccba-dc3d-4f31-b08f-1cd5669847d5 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk VolumeID:db1bccba-dc3d-4f31-b08f-1cd5669847d5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.703Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:db1bccba-dc3d-4f31-b08f-1cd5669847d5 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/db1bccba-dc3d-4f31-b08f-1cd5669847d5 UID:5ec1ec90-24d2-44b5-b926-4af4dbd68037 ResourceVersion:7157356 Generation:1 CreationTimestamp:2020-07-17 06:11:41 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:11:41 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk VolumeID:db1bccba-dc3d-4f31-b08f-1cd5669847d5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.703Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {db1bccba-dc3d-4f31-b08f-1cd5669847d5   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/db1bccba-dc3d-4f31-b08f-1cd5669847d5 5ec1ec90-24d2-44b5-b926-4af4dbd68037 7157356 1 2020-07-17 06:11:41 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:11:41 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk db1bccba-dc3d-4f31-b08f-1cd5669847d5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.704Z	DEBUG	syncer/util.go:46	FullSync: pv db1bccba-dc3d-4f31-b08f-1cd5669847d5 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.704Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.704Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.704Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f02a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "61ec399f-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005e9980)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:41.703Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:11:41.704Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-78d23dbf-4abd-4ec3-9baf-f7d6b2e1db28.vmdk", volumeID: "db1bccba-dc3d-4f31-b08f-1cd5669847d5" mapping in the cache
2020-07-17T06:11:41.724Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.756Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "61ec399f-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce64"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.756Z	DEBUG	volume/manager.go:244	Deleted task for 61ec399f-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.756Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "61ec399f-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ce64", volumeID: "3b2d0418-8845-4d82-b0ff-c3ddfe960fd6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.756Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk" as container volume with ID: "3b2d0418-8845-4d82-b0ff-c3ddfe960fd6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.757Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk" with CNS. VolumeID: "3b2d0418-8845-4d82-b0ff-c3ddfe960fd6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.757Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {3b2d0418-8845-4d82-b0ff-c3ddfe960fd6      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk 3b2d0418-8845-4d82-b0ff-c3ddfe960fd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.758Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3b2d0418-8845-4d82-b0ff-c3ddfe960fd6 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk VolumeID:3b2d0418-8845-4d82-b0ff-c3ddfe960fd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.772Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:11:55.772Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk", volumeID: "3b2d0418-8845-4d82-b0ff-c3ddfe960fd6" mapping in the cache
2020-07-17T06:11:55.775Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3b2d0418-8845-4d82-b0ff-c3ddfe960fd6 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3b2d0418-8845-4d82-b0ff-c3ddfe960fd6 UID:83079200-efa7-4a2a-9c42-35e33a705337 ResourceVersion:7157397 Generation:1 CreationTimestamp:2020-07-17 06:11:55 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:11:55 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk VolumeID:3b2d0418-8845-4d82-b0ff-c3ddfe960fd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.775Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {3b2d0418-8845-4d82-b0ff-c3ddfe960fd6   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3b2d0418-8845-4d82-b0ff-c3ddfe960fd6 83079200-efa7-4a2a-9c42-35e33a705337 7157397 1 2020-07-17 06:11:55 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:11:55 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-26e835be-b674-46b3-8d8a-c45d7041c19e.vmdk 3b2d0418-8845-4d82-b0ff-c3ddfe960fd6}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.775Z	DEBUG	syncer/util.go:46	FullSync: pv 3b2d0418-8845-4d82-b0ff-c3ddfe960fd6 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.775Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.775Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.775Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f0540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "6a4f5df8-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b1710)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:11:55.790Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.470Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "6a4f5df8-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ceaa"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.470Z	DEBUG	volume/manager.go:244	Deleted task for 6a4f5df8-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.470Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "6a4f5df8-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ceaa", volumeID: "dc5c36ac-2bd6-4b34-8064-4e9676c23a6b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.470Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk" as container volume with ID: "dc5c36ac-2bd6-4b34-8064-4e9676c23a6b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.470Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk" with CNS. VolumeID: "dc5c36ac-2bd6-4b34-8064-4e9676c23a6b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.470Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {dc5c36ac-2bd6-4b34-8064-4e9676c23a6b      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk dc5c36ac-2bd6-4b34-8064-4e9676c23a6b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.470Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dc5c36ac-2bd6-4b34-8064-4e9676c23a6b GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk VolumeID:dc5c36ac-2bd6-4b34-8064-4e9676c23a6b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.483Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:dc5c36ac-2bd6-4b34-8064-4e9676c23a6b GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dc5c36ac-2bd6-4b34-8064-4e9676c23a6b UID:8598b149-8289-4d98-a2f3-add9d2ed5599 ResourceVersion:7157453 Generation:1 CreationTimestamp:2020-07-17 06:12:13 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:12:13 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk VolumeID:dc5c36ac-2bd6-4b34-8064-4e9676c23a6b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.483Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {dc5c36ac-2bd6-4b34-8064-4e9676c23a6b   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/dc5c36ac-2bd6-4b34-8064-4e9676c23a6b 8598b149-8289-4d98-a2f3-add9d2ed5599 7157453 1 2020-07-17 06:12:13 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:12:13 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk dc5c36ac-2bd6-4b34-8064-4e9676c23a6b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.483Z	DEBUG	syncer/util.go:46	FullSync: pv dc5c36ac-2bd6-4b34-8064-4e9676c23a6b is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.483Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.484Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.483Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:12:13.484Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3a49ba93-ba6a-4cf2-b81b-bb52c348637d.vmdk", volumeID: "dc5c36ac-2bd6-4b34-8064-4e9676c23a6b" mapping in the cache
2020-07-17T06:12:13.484Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f0700)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "74dd7754-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007a2ff0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:13.499Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.251Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "74dd7754-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ceae"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.251Z	DEBUG	volume/manager.go:244	Deleted task for 74dd7754-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.252Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "74dd7754-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168ceae", volumeID: "f84c3819-879b-4bbf-a183-7c48536013c1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.252Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk" as container volume with ID: "f84c3819-879b-4bbf-a183-7c48536013c1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.252Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk" with CNS. VolumeID: "f84c3819-879b-4bbf-a183-7c48536013c1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.252Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {f84c3819-879b-4bbf-a183-7c48536013c1      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk f84c3819-879b-4bbf-a183-7c48536013c1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.252Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f84c3819-879b-4bbf-a183-7c48536013c1 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk VolumeID:f84c3819-879b-4bbf-a183-7c48536013c1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.264Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:12:27.264Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk", volumeID: "f84c3819-879b-4bbf-a183-7c48536013c1" mapping in the cache
2020-07-17T06:12:27.265Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f84c3819-879b-4bbf-a183-7c48536013c1 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f84c3819-879b-4bbf-a183-7c48536013c1 UID:7c838d0c-eaf2-4813-b13a-ba3eb19d4469 ResourceVersion:7157497 Generation:1 CreationTimestamp:2020-07-17 06:12:27 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:12:27 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk VolumeID:f84c3819-879b-4bbf-a183-7c48536013c1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.265Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {f84c3819-879b-4bbf-a183-7c48536013c1   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f84c3819-879b-4bbf-a183-7c48536013c1 7c838d0c-eaf2-4813-b13a-ba3eb19d4469 7157497 1 2020-07-17 06:12:27 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:12:27 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-718feab2-4d08-401b-a655-ff16818b9e4a.vmdk f84c3819-879b-4bbf-a183-7c48536013c1}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.265Z	DEBUG	syncer/util.go:46	FullSync: pv f84c3819-879b-4bbf-a183-7c48536013c1 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.265Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.266Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.266Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004682a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "7d14707b-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0003fd140)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:27.283Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.710Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "7d14707b-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cec6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.710Z	DEBUG	volume/manager.go:244	Deleted task for 7d14707b-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.710Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "7d14707b-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cec6", volumeID: "79e6e129-9aba-4abe-be9f-585b2aa19d2b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.710Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk" as container volume with ID: "79e6e129-9aba-4abe-be9f-585b2aa19d2b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.710Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk" with CNS. VolumeID: "79e6e129-9aba-4abe-be9f-585b2aa19d2b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.713Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {79e6e129-9aba-4abe-be9f-585b2aa19d2b      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk 79e6e129-9aba-4abe-be9f-585b2aa19d2b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.713Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:79e6e129-9aba-4abe-be9f-585b2aa19d2b GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk VolumeID:79e6e129-9aba-4abe-be9f-585b2aa19d2b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.728Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:12:41.728Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk", volumeID: "79e6e129-9aba-4abe-be9f-585b2aa19d2b" mapping in the cache
2020-07-17T06:12:41.733Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:79e6e129-9aba-4abe-be9f-585b2aa19d2b GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/79e6e129-9aba-4abe-be9f-585b2aa19d2b UID:c8d58ef3-22b8-4cc2-a440-8fd512cf4832 ResourceVersion:7157543 Generation:1 CreationTimestamp:2020-07-17 06:12:41 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:12:41 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk VolumeID:79e6e129-9aba-4abe-be9f-585b2aa19d2b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.733Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {79e6e129-9aba-4abe-be9f-585b2aa19d2b   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/79e6e129-9aba-4abe-be9f-585b2aa19d2b c8d58ef3-22b8-4cc2-a440-8fd512cf4832 7157543 1 2020-07-17 06:12:41 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:12:41 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-96834345-5a2b-4787-8c7b-aa9fe9cdeb67.vmdk 79e6e129-9aba-4abe-be9f-585b2aa19d2b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.733Z	DEBUG	syncer/util.go:46	FullSync: pv 79e6e129-9aba-4abe-be9f-585b2aa19d2b is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.733Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.734Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.734Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007f08c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "85b41157-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00030b950)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:41.753Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.585Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "85b41157-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cecb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.586Z	DEBUG	volume/manager.go:244	Deleted task for 85b41157-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.586Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "85b41157-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cecb", volumeID: "75dc3039-07b2-493e-b421-5e3e99903182"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.590Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk" as container volume with ID: "75dc3039-07b2-493e-b421-5e3e99903182"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.590Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk" with CNS. VolumeID: "75dc3039-07b2-493e-b421-5e3e99903182"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.591Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {75dc3039-07b2-493e-b421-5e3e99903182      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk 75dc3039-07b2-493e-b421-5e3e99903182}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.591Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:75dc3039-07b2-493e-b421-5e3e99903182 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk VolumeID:75dc3039-07b2-493e-b421-5e3e99903182}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.604Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:75dc3039-07b2-493e-b421-5e3e99903182 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/75dc3039-07b2-493e-b421-5e3e99903182 UID:c4a84131-0dfe-40ab-9c7b-d971a08ac617 ResourceVersion:7157586 Generation:1 CreationTimestamp:2020-07-17 06:12:55 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:12:55 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk VolumeID:75dc3039-07b2-493e-b421-5e3e99903182}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.604Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {75dc3039-07b2-493e-b421-5e3e99903182   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/75dc3039-07b2-493e-b421-5e3e99903182 c4a84131-0dfe-40ab-9c7b-d971a08ac617 7157586 1 2020-07-17 06:12:55 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:12:55 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk 75dc3039-07b2-493e-b421-5e3e99903182}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.604Z	DEBUG	syncer/util.go:46	FullSync: pv 75dc3039-07b2-493e-b421-5e3e99903182 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.604Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.604Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.605Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "8df88cfa-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00069d890)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:12:55.606Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:12:55.621Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9225ddb0-5a44-4a94-abf9-1b125642976d.vmdk", volumeID: "75dc3039-07b2-493e-b421-5e3e99903182" mapping in the cache
2020-07-17T06:12:55.626Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.279Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "8df88cfa-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf07"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.279Z	DEBUG	volume/manager.go:244	Deleted task for 8df88cfa-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.279Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "8df88cfa-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf07", volumeID: "271208e8-8fdc-46ac-930f-0df89306b9e0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.279Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk" as container volume with ID: "271208e8-8fdc-46ac-930f-0df89306b9e0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.279Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk" with CNS. VolumeID: "271208e8-8fdc-46ac-930f-0df89306b9e0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.279Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {271208e8-8fdc-46ac-930f-0df89306b9e0      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk 271208e8-8fdc-46ac-930f-0df89306b9e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.279Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:271208e8-8fdc-46ac-930f-0df89306b9e0 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk VolumeID:271208e8-8fdc-46ac-930f-0df89306b9e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.291Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:271208e8-8fdc-46ac-930f-0df89306b9e0 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/271208e8-8fdc-46ac-930f-0df89306b9e0 UID:3eeb784f-bd7b-439a-bad4-53c92009d773 ResourceVersion:7157643 Generation:1 CreationTimestamp:2020-07-17 06:13:14 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:13:14 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk VolumeID:271208e8-8fdc-46ac-930f-0df89306b9e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.291Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {271208e8-8fdc-46ac-930f-0df89306b9e0   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/271208e8-8fdc-46ac-930f-0df89306b9e0 3eeb784f-bd7b-439a-bad4-53c92009d773 7157643 1 2020-07-17 06:13:14 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:13:14 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk 271208e8-8fdc-46ac-930f-0df89306b9e0}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.291Z	DEBUG	syncer/util.go:46	FullSync: pv 271208e8-8fdc-46ac-930f-0df89306b9e0 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.291Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.291Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.293Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "991bf596-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005dc990)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:14.297Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:13:14.299Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-233aeeb1-5006-48f7-8fc8-2088898a658d.vmdk", volumeID: "271208e8-8fdc-46ac-930f-0df89306b9e0" mapping in the cache
2020-07-17T06:13:14.315Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.611Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "991bf596-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf14"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.614Z	DEBUG	volume/manager.go:244	Deleted task for 991bf596-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.614Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "991bf596-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf14", volumeID: "b5b7f352-589d-437a-8656-b79c63d71ab4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.614Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk" as container volume with ID: "b5b7f352-589d-437a-8656-b79c63d71ab4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.614Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk" with CNS. VolumeID: "b5b7f352-589d-437a-8656-b79c63d71ab4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.614Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {b5b7f352-589d-437a-8656-b79c63d71ab4      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk b5b7f352-589d-437a-8656-b79c63d71ab4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.614Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b5b7f352-589d-437a-8656-b79c63d71ab4 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk VolumeID:b5b7f352-589d-437a-8656-b79c63d71ab4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.623Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:13:26.623Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk", volumeID: "b5b7f352-589d-437a-8656-b79c63d71ab4" mapping in the cache
2020-07-17T06:13:26.626Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:b5b7f352-589d-437a-8656-b79c63d71ab4 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b5b7f352-589d-437a-8656-b79c63d71ab4 UID:f145625b-e0b9-4501-95d0-96129d9916e7 ResourceVersion:7157682 Generation:1 CreationTimestamp:2020-07-17 06:13:26 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:13:26 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk VolumeID:b5b7f352-589d-437a-8656-b79c63d71ab4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.626Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {b5b7f352-589d-437a-8656-b79c63d71ab4   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/b5b7f352-589d-437a-8656-b79c63d71ab4 f145625b-e0b9-4501-95d0-96129d9916e7 7157682 1 2020-07-17 06:13:26 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:13:26 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-763a1b1e-c904-47a7-95f7-1209ad3a9941.vmdk b5b7f352-589d-437a-8656-b79c63d71ab4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.626Z	DEBUG	syncer/util.go:46	FullSync: pv b5b7f352-589d-437a-8656-b79c63d71ab4 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.626Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.626Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.626Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "a0761c8d-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00030bd70)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:26.643Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.703Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "a0761c8d-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf16"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.703Z	DEBUG	volume/manager.go:244	Deleted task for a0761c8d-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.703Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "a0761c8d-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf16", volumeID: "d3078dd0-b0ef-4722-966f-626635ad46a2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.703Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk" as container volume with ID: "d3078dd0-b0ef-4722-966f-626635ad46a2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.703Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk" with CNS. VolumeID: "d3078dd0-b0ef-4722-966f-626635ad46a2"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.703Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {d3078dd0-b0ef-4722-966f-626635ad46a2      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk d3078dd0-b0ef-4722-966f-626635ad46a2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.703Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d3078dd0-b0ef-4722-966f-626635ad46a2 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk VolumeID:d3078dd0-b0ef-4722-966f-626635ad46a2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.717Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:13:37.717Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk", volumeID: "d3078dd0-b0ef-4722-966f-626635ad46a2" mapping in the cache
2020-07-17T06:13:37.717Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d3078dd0-b0ef-4722-966f-626635ad46a2 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d3078dd0-b0ef-4722-966f-626635ad46a2 UID:e25309f3-e12d-4d4f-8759-49cdfb823e52 ResourceVersion:7157719 Generation:1 CreationTimestamp:2020-07-17 06:13:37 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:13:37 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk VolumeID:d3078dd0-b0ef-4722-966f-626635ad46a2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.718Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {d3078dd0-b0ef-4722-966f-626635ad46a2   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d3078dd0-b0ef-4722-966f-626635ad46a2 e25309f3-e12d-4d4f-8759-49cdfb823e52 7157719 1 2020-07-17 06:13:37 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:13:37 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2e35e4f3-8715-4a1c-9327-5f3a7c126684.vmdk d3078dd0-b0ef-4722-966f-626635ad46a2}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.718Z	DEBUG	syncer/util.go:46	FullSync: pv d3078dd0-b0ef-4722-966f-626635ad46a2 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.718Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.718Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.719Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ab8000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "a7129a88-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005703c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:37.747Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.798Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "a7129a88-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf18"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.798Z	DEBUG	volume/manager.go:244	Deleted task for a7129a88-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.798Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "a7129a88-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf18", volumeID: "3bd395b9-fce6-419f-ae82-35886c1779f8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.798Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk" as container volume with ID: "3bd395b9-fce6-419f-ae82-35886c1779f8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.799Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk" with CNS. VolumeID: "3bd395b9-fce6-419f-ae82-35886c1779f8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.799Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {3bd395b9-fce6-419f-ae82-35886c1779f8      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk 3bd395b9-fce6-419f-ae82-35886c1779f8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.803Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3bd395b9-fce6-419f-ae82-35886c1779f8 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk VolumeID:3bd395b9-fce6-419f-ae82-35886c1779f8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.815Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:13:51.815Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk", volumeID: "3bd395b9-fce6-419f-ae82-35886c1779f8" mapping in the cache
2020-07-17T06:13:51.816Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:3bd395b9-fce6-419f-ae82-35886c1779f8 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3bd395b9-fce6-419f-ae82-35886c1779f8 UID:6cd1b5aa-9b79-4b1e-acca-807e733448e2 ResourceVersion:7157764 Generation:1 CreationTimestamp:2020-07-17 06:13:51 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:13:51 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk VolumeID:3bd395b9-fce6-419f-ae82-35886c1779f8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.816Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {3bd395b9-fce6-419f-ae82-35886c1779f8   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/3bd395b9-fce6-419f-ae82-35886c1779f8 6cd1b5aa-9b79-4b1e-acca-807e733448e2 7157764 1 2020-07-17 06:13:51 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:13:51 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-278d12d2-01de-4739-b125-4a7bd4b0716a.vmdk 3bd395b9-fce6-419f-ae82-35886c1779f8}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.816Z	DEBUG	syncer/util.go:46	FullSync: pv 3bd395b9-fce6-419f-ae82-35886c1779f8 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.817Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.817Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.817Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ab81c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "af79ef43-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000446c00)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:13:51.838Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.720Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "af79ef43-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf56"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.720Z	DEBUG	volume/manager.go:244	Deleted task for af79ef43-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.720Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "af79ef43-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf56", volumeID: "488f9311-49ea-4fe0-abef-e9840bc160c9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.720Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk" as container volume with ID: "488f9311-49ea-4fe0-abef-e9840bc160c9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.720Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk" with CNS. VolumeID: "488f9311-49ea-4fe0-abef-e9840bc160c9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.720Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {488f9311-49ea-4fe0-abef-e9840bc160c9      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk 488f9311-49ea-4fe0-abef-e9840bc160c9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.720Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:488f9311-49ea-4fe0-abef-e9840bc160c9 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk VolumeID:488f9311-49ea-4fe0-abef-e9840bc160c9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.732Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:14:06.738Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk", volumeID: "488f9311-49ea-4fe0-abef-e9840bc160c9" mapping in the cache
2020-07-17T06:14:06.738Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:488f9311-49ea-4fe0-abef-e9840bc160c9 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/488f9311-49ea-4fe0-abef-e9840bc160c9 UID:26ef1191-a1a3-480b-8539-9052771ec9c0 ResourceVersion:7157809 Generation:1 CreationTimestamp:2020-07-17 06:14:06 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:14:06 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk VolumeID:488f9311-49ea-4fe0-abef-e9840bc160c9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.738Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {488f9311-49ea-4fe0-abef-e9840bc160c9   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/488f9311-49ea-4fe0-abef-e9840bc160c9 26ef1191-a1a3-480b-8539-9052771ec9c0 7157809 1 2020-07-17 06:14:06 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:14:06 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-60f97b25-ea5d-43b5-9682-9239abfa5277.vmdk 488f9311-49ea-4fe0-abef-e9840bc160c9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.738Z	DEBUG	syncer/util.go:46	FullSync: pv 488f9311-49ea-4fe0-abef-e9840bc160c9 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.738Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.738Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.738Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ab82a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "b85ebd4f-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b1560)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:06.751Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.452Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "b85ebd4f-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf62"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.452Z	DEBUG	volume/manager.go:244	Deleted task for b85ebd4f-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.452Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "b85ebd4f-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf62", volumeID: "e75f9345-82f4-4e9c-bb19-b52cf3f9ff87"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.452Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk" as container volume with ID: "e75f9345-82f4-4e9c-bb19-b52cf3f9ff87"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.452Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk" with CNS. VolumeID: "e75f9345-82f4-4e9c-bb19-b52cf3f9ff87"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.452Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {e75f9345-82f4-4e9c-bb19-b52cf3f9ff87      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk e75f9345-82f4-4e9c-bb19-b52cf3f9ff87}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.452Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e75f9345-82f4-4e9c-bb19-b52cf3f9ff87 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk VolumeID:e75f9345-82f4-4e9c-bb19-b52cf3f9ff87}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.464Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:14:25.466Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e75f9345-82f4-4e9c-bb19-b52cf3f9ff87 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e75f9345-82f4-4e9c-bb19-b52cf3f9ff87 UID:ff0d54dd-3291-492f-b4f5-2a4e2ce16f2a ResourceVersion:7157870 Generation:1 CreationTimestamp:2020-07-17 06:14:25 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:14:25 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk VolumeID:e75f9345-82f4-4e9c-bb19-b52cf3f9ff87}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.467Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk", volumeID: "e75f9345-82f4-4e9c-bb19-b52cf3f9ff87" mapping in the cache
2020-07-17T06:14:25.469Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {e75f9345-82f4-4e9c-bb19-b52cf3f9ff87   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e75f9345-82f4-4e9c-bb19-b52cf3f9ff87 ff0d54dd-3291-492f-b4f5-2a4e2ce16f2a 7157870 1 2020-07-17 06:14:25 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:14:25 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d1569247-2cc2-45ad-9095-c197e28c8b7b.vmdk e75f9345-82f4-4e9c-bb19-b52cf3f9ff87}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.469Z	DEBUG	syncer/util.go:46	FullSync: pv e75f9345-82f4-4e9c-bb19-b52cf3f9ff87 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.469Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.469Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.469Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000ab8460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "c388de96-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dbe9c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:25.487Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.771Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "c388de96-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf7b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.772Z	DEBUG	volume/manager.go:244	Deleted task for c388de96-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.779Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "c388de96-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf7b", volumeID: "4117edef-8a7c-4cc6-8954-8284491ca8cc"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.779Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk" as container volume with ID: "4117edef-8a7c-4cc6-8954-8284491ca8cc"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.779Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk" with CNS. VolumeID: "4117edef-8a7c-4cc6-8954-8284491ca8cc"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.779Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {4117edef-8a7c-4cc6-8954-8284491ca8cc      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk 4117edef-8a7c-4cc6-8954-8284491ca8cc}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.779Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4117edef-8a7c-4cc6-8954-8284491ca8cc GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk VolumeID:4117edef-8a7c-4cc6-8954-8284491ca8cc}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.797Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:14:41.797Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk", volumeID: "4117edef-8a7c-4cc6-8954-8284491ca8cc" mapping in the cache
2020-07-17T06:14:41.798Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4117edef-8a7c-4cc6-8954-8284491ca8cc GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4117edef-8a7c-4cc6-8954-8284491ca8cc UID:42a8618d-e2a1-4e03-b962-542708979d38 ResourceVersion:7157922 Generation:1 CreationTimestamp:2020-07-17 06:14:41 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:14:41 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk VolumeID:4117edef-8a7c-4cc6-8954-8284491ca8cc}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.799Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {4117edef-8a7c-4cc6-8954-8284491ca8cc   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4117edef-8a7c-4cc6-8954-8284491ca8cc 42a8618d-e2a1-4e03-b962-542708979d38 7157922 1 2020-07-17 06:14:41 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:14:41 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e57094b7-02d0-4fab-a980-31219d9fc0a7.vmdk 4117edef-8a7c-4cc6-8954-8284491ca8cc}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.799Z	DEBUG	syncer/util.go:46	FullSync: pv 4117edef-8a7c-4cc6-8954-8284491ca8cc is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.800Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.800Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.806Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468700)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "cd44c397-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0009cec90)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:41.820Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.743Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "cd44c397-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf80"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.744Z	DEBUG	volume/manager.go:244	Deleted task for cd44c397-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.744Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "cd44c397-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cf80", volumeID: "6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.744Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk" as container volume with ID: "6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.744Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk" with CNS. VolumeID: "6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.744Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk 6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.744Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk VolumeID:6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.758Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9 UID:0230be73-d1d9-474a-aa91-f27efb600cd6 ResourceVersion:7157966 Generation:1 CreationTimestamp:2020-07-17 06:14:55 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:14:55 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk VolumeID:6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.758Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9 0230be73-d1d9-474a-aa91-f27efb600cd6 7157966 1 2020-07-17 06:14:55 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:14:55 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk 6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.758Z	DEBUG	syncer/util.go:46	FullSync: pv 6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.758Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.759Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.759Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004688c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "d596abac-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007de390)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:14:55.759Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:14:55.759Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f283ad31-ed67-4bcc-b7bc-7e6ae03c3ca4.vmdk", volumeID: "6bb1a6de-4f5e-4b58-8cc4-af50e9816ae9" mapping in the cache
2020-07-17T06:14:55.776Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.620Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "d596abac-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfbb"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.620Z	DEBUG	volume/manager.go:244	Deleted task for d596abac-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.620Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "d596abac-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfbb", volumeID: "bc5fc683-6a97-449d-94b4-b1a89cc9c93b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.620Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk" as container volume with ID: "bc5fc683-6a97-449d-94b4-b1a89cc9c93b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.620Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk" with CNS. VolumeID: "bc5fc683-6a97-449d-94b4-b1a89cc9c93b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.620Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {bc5fc683-6a97-449d-94b4-b1a89cc9c93b      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk bc5fc683-6a97-449d-94b4-b1a89cc9c93b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.620Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bc5fc683-6a97-449d-94b4-b1a89cc9c93b GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk VolumeID:bc5fc683-6a97-449d-94b4-b1a89cc9c93b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.642Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:15:11.642Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk", volumeID: "bc5fc683-6a97-449d-94b4-b1a89cc9c93b" mapping in the cache
2020-07-17T06:15:11.644Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bc5fc683-6a97-449d-94b4-b1a89cc9c93b GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bc5fc683-6a97-449d-94b4-b1a89cc9c93b UID:4cddae6a-aafb-4151-a469-640c6dd79b33 ResourceVersion:7158015 Generation:1 CreationTimestamp:2020-07-17 06:15:11 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:15:11 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk VolumeID:bc5fc683-6a97-449d-94b4-b1a89cc9c93b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.649Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {bc5fc683-6a97-449d-94b4-b1a89cc9c93b   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bc5fc683-6a97-449d-94b4-b1a89cc9c93b 4cddae6a-aafb-4151-a469-640c6dd79b33 7158015 1 2020-07-17 06:15:11 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:15:11 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-7c159517-1877-4d84-a99d-1d71ee3ce1e7.vmdk bc5fc683-6a97-449d-94b4-b1a89cc9c93b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.649Z	DEBUG	syncer/util.go:46	FullSync: pv bc5fc683-6a97-449d-94b4-b1a89cc9c93b is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.649Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.649Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.649Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "df0f6077-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007df200)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:11.673Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:24.996Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "df0f6077-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfc8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.000Z	DEBUG	volume/manager.go:244	Deleted task for df0f6077-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.000Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "df0f6077-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfc8", volumeID: "47825047-fe5a-4a53-9ccb-721617a05c72"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.004Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk" as container volume with ID: "47825047-fe5a-4a53-9ccb-721617a05c72"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.004Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk" with CNS. VolumeID: "47825047-fe5a-4a53-9ccb-721617a05c72"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.005Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {47825047-fe5a-4a53-9ccb-721617a05c72      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk 47825047-fe5a-4a53-9ccb-721617a05c72}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.005Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:47825047-fe5a-4a53-9ccb-721617a05c72 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk VolumeID:47825047-fe5a-4a53-9ccb-721617a05c72}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.018Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:47825047-fe5a-4a53-9ccb-721617a05c72 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/47825047-fe5a-4a53-9ccb-721617a05c72 UID:6885dd7c-10da-4a8a-adb6-727c2aeef67f ResourceVersion:7158056 Generation:1 CreationTimestamp:2020-07-17 06:15:25 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:15:25 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk VolumeID:47825047-fe5a-4a53-9ccb-721617a05c72}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.018Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {47825047-fe5a-4a53-9ccb-721617a05c72   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/47825047-fe5a-4a53-9ccb-721617a05c72 6885dd7c-10da-4a8a-adb6-727c2aeef67f 7158056 1 2020-07-17 06:15:25 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:15:25 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk 47825047-fe5a-4a53-9ccb-721617a05c72}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.018Z	DEBUG	syncer/util.go:46	FullSync: pv 47825047-fe5a-4a53-9ccb-721617a05c72 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.018Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.018Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.018Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007240e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "e7075022-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00089a990)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:25.018Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:15:25.019Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-63d2b59d-1494-490e-b5bb-7050a3a03b54.vmdk", volumeID: "47825047-fe5a-4a53-9ccb-721617a05c72" mapping in the cache
2020-07-17T06:15:25.037Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.424Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "e7075022-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfca"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.424Z	DEBUG	volume/manager.go:244	Deleted task for e7075022-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.424Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "e7075022-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfca", volumeID: "7fd54bc2-acb0-4ce9-9023-2da57bff7cd7"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.424Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk" as container volume with ID: "7fd54bc2-acb0-4ce9-9023-2da57bff7cd7"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.424Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk" with CNS. VolumeID: "7fd54bc2-acb0-4ce9-9023-2da57bff7cd7"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.424Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {7fd54bc2-acb0-4ce9-9023-2da57bff7cd7      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk 7fd54bc2-acb0-4ce9-9023-2da57bff7cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.425Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7fd54bc2-acb0-4ce9-9023-2da57bff7cd7 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk VolumeID:7fd54bc2-acb0-4ce9-9023-2da57bff7cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.436Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:15:36.441Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk", volumeID: "7fd54bc2-acb0-4ce9-9023-2da57bff7cd7" mapping in the cache
2020-07-17T06:15:36.443Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7fd54bc2-acb0-4ce9-9023-2da57bff7cd7 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7fd54bc2-acb0-4ce9-9023-2da57bff7cd7 UID:513b1763-bfa1-4b99-b82b-5c8731246e73 ResourceVersion:7158094 Generation:1 CreationTimestamp:2020-07-17 06:15:36 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:15:36 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk VolumeID:7fd54bc2-acb0-4ce9-9023-2da57bff7cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.443Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {7fd54bc2-acb0-4ce9-9023-2da57bff7cd7   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7fd54bc2-acb0-4ce9-9023-2da57bff7cd7 513b1763-bfa1-4b99-b82b-5c8731246e73 7158094 1 2020-07-17 06:15:36 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:15:36 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2932253a-6be3-42b5-b4e4-d7865a43096e.vmdk 7fd54bc2-acb0-4ce9-9023-2da57bff7cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.443Z	DEBUG	syncer/util.go:46	FullSync: pv 7fd54bc2-acb0-4ce9-9023-2da57bff7cd7 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.443Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.443Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.443Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "edd6ad9c-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000820db0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:36.463Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.930Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "edd6ad9c-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfd8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.930Z	DEBUG	volume/manager.go:244	Deleted task for edd6ad9c-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.930Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "edd6ad9c-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168cfd8", volumeID: "02c033bb-50bb-49eb-94ae-9717fae47e9a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.930Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk" as container volume with ID: "02c033bb-50bb-49eb-94ae-9717fae47e9a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.930Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk" with CNS. VolumeID: "02c033bb-50bb-49eb-94ae-9717fae47e9a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.930Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {02c033bb-50bb-49eb-94ae-9717fae47e9a      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk 02c033bb-50bb-49eb-94ae-9717fae47e9a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.930Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:02c033bb-50bb-49eb-94ae-9717fae47e9a GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk VolumeID:02c033bb-50bb-49eb-94ae-9717fae47e9a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.943Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:02c033bb-50bb-49eb-94ae-9717fae47e9a GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/02c033bb-50bb-49eb-94ae-9717fae47e9a UID:e88a8c29-745f-445d-b0da-43c106776132 ResourceVersion:7158143 Generation:1 CreationTimestamp:2020-07-17 06:15:51 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:15:51 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk VolumeID:02c033bb-50bb-49eb-94ae-9717fae47e9a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.943Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {02c033bb-50bb-49eb-94ae-9717fae47e9a   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/02c033bb-50bb-49eb-94ae-9717fae47e9a e88a8c29-745f-445d-b0da-43c106776132 7158143 1 2020-07-17 06:15:51 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:15:51 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk 02c033bb-50bb-49eb-94ae-9717fae47e9a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.943Z	DEBUG	syncer/util.go:46	FullSync: pv 02c033bb-50bb-49eb-94ae-9717fae47e9a is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.943Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.944Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.944Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007241c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "f713d08d-c7f4-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b0b10)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:15:51.944Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:15:51.946Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c072fa85-8fa5-460e-a2a0-7a7ec0d53df3.vmdk", volumeID: "02c033bb-50bb-49eb-94ae-9717fae47e9a" mapping in the cache
2020-07-17T06:15:51.960Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.584Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "f713d08d-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168d016"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.584Z	DEBUG	volume/manager.go:244	Deleted task for f713d08d-c7f4-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.584Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "f713d08d-c7f4-11ea-8f7c-9695ff1c5b6f", opId: "9168d016", volumeID: "37272b2d-e852-4ad9-9c35-b9ebd0326922"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.584Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk" as container volume with ID: "37272b2d-e852-4ad9-9c35-b9ebd0326922"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.584Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk" with CNS. VolumeID: "37272b2d-e852-4ad9-9c35-b9ebd0326922"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.584Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {37272b2d-e852-4ad9-9c35-b9ebd0326922      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk 37272b2d-e852-4ad9-9c35-b9ebd0326922}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.584Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:37272b2d-e852-4ad9-9c35-b9ebd0326922 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk VolumeID:37272b2d-e852-4ad9-9c35-b9ebd0326922}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.600Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:37272b2d-e852-4ad9-9c35-b9ebd0326922 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/37272b2d-e852-4ad9-9c35-b9ebd0326922 UID:8be0ffcf-5d7e-43f5-86a8-4b170e1f3039 ResourceVersion:7158191 Generation:1 CreationTimestamp:2020-07-17 06:16:07 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:16:07 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk VolumeID:37272b2d-e852-4ad9-9c35-b9ebd0326922}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.600Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {37272b2d-e852-4ad9-9c35-b9ebd0326922   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/37272b2d-e852-4ad9-9c35-b9ebd0326922 8be0ffcf-5d7e-43f5-86a8-4b170e1f3039 7158191 1 2020-07-17 06:16:07 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:16:07 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk 37272b2d-e852-4ad9-9c35-b9ebd0326922}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.600Z	DEBUG	syncer/util.go:46	FullSync: pv 37272b2d-e852-4ad9-9c35-b9ebd0326922 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.600Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.600Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.601Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "0068ccfc-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002e3440)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:07.602Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:16:07.602Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9ccc6bf9-51c3-4370-b4b2-16280c7d9924.vmdk", volumeID: "37272b2d-e852-4ad9-9c35-b9ebd0326922" mapping in the cache
2020-07-17T06:16:07.625Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.145Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "0068ccfc-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d019"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.145Z	DEBUG	volume/manager.go:244	Deleted task for 0068ccfc-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.145Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "0068ccfc-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d019", volumeID: "01be5fc0-98f2-49d8-b485-270aadfbed56"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.145Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk" as container volume with ID: "01be5fc0-98f2-49d8-b485-270aadfbed56"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.145Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk" with CNS. VolumeID: "01be5fc0-98f2-49d8-b485-270aadfbed56"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.145Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {01be5fc0-98f2-49d8-b485-270aadfbed56      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk 01be5fc0-98f2-49d8-b485-270aadfbed56}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.145Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:01be5fc0-98f2-49d8-b485-270aadfbed56 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk VolumeID:01be5fc0-98f2-49d8-b485-270aadfbed56}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.159Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:01be5fc0-98f2-49d8-b485-270aadfbed56 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/01be5fc0-98f2-49d8-b485-270aadfbed56 UID:9c11c9a6-6195-47be-8886-ec6bae71d3a2 ResourceVersion:7158236 Generation:1 CreationTimestamp:2020-07-17 06:16:22 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:16:22 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk VolumeID:01be5fc0-98f2-49d8-b485-270aadfbed56}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.159Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {01be5fc0-98f2-49d8-b485-270aadfbed56   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/01be5fc0-98f2-49d8-b485-270aadfbed56 9c11c9a6-6195-47be-8886-ec6bae71d3a2 7158236 1 2020-07-17 06:16:22 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:16:22 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk 01be5fc0-98f2-49d8-b485-270aadfbed56}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.160Z	DEBUG	syncer/util.go:46	FullSync: pv 01be5fc0-98f2-49d8-b485-270aadfbed56 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.160Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.160Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.160Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000724380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "09167cc6-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00051ca20)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:22.159Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:16:22.161Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c0323c1b-46b8-483a-8048-73a8b9587067.vmdk", volumeID: "01be5fc0-98f2-49d8-b485-270aadfbed56" mapping in the cache
2020-07-17T06:16:22.184Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.985Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "09167cc6-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d03b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.986Z	DEBUG	volume/manager.go:244	Deleted task for 09167cc6-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.986Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "09167cc6-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d03b", volumeID: "1fcdb888-3603-4bac-a1b6-71bc1fd94cd4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.986Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk" as container volume with ID: "1fcdb888-3603-4bac-a1b6-71bc1fd94cd4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.986Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk" with CNS. VolumeID: "1fcdb888-3603-4bac-a1b6-71bc1fd94cd4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.986Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {1fcdb888-3603-4bac-a1b6-71bc1fd94cd4      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk 1fcdb888-3603-4bac-a1b6-71bc1fd94cd4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.986Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:1fcdb888-3603-4bac-a1b6-71bc1fd94cd4 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk VolumeID:1fcdb888-3603-4bac-a1b6-71bc1fd94cd4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.997Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:16:33.998Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk", volumeID: "1fcdb888-3603-4bac-a1b6-71bc1fd94cd4" mapping in the cache
2020-07-17T06:16:33.998Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:1fcdb888-3603-4bac-a1b6-71bc1fd94cd4 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/1fcdb888-3603-4bac-a1b6-71bc1fd94cd4 UID:400a5e3e-dbc4-44ff-81d5-f1024ebdbfd2 ResourceVersion:7158274 Generation:1 CreationTimestamp:2020-07-17 06:16:33 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:16:33 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk VolumeID:1fcdb888-3603-4bac-a1b6-71bc1fd94cd4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.998Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {1fcdb888-3603-4bac-a1b6-71bc1fd94cd4   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/1fcdb888-3603-4bac-a1b6-71bc1fd94cd4 400a5e3e-dbc4-44ff-81d5-f1024ebdbfd2 7158274 1 2020-07-17 06:16:33 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:16:33 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-49951cbe-8c9e-4194-a537-023949b3721f.vmdk 1fcdb888-3603-4bac-a1b6-71bc1fd94cd4}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.999Z	DEBUG	syncer/util.go:46	FullSync: pv 1fcdb888-3603-4bac-a1b6-71bc1fd94cd4 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.999Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.999Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:33.999Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000724540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "1025039b-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00030b170)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:34.021Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.565Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "1025039b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d03d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.565Z	DEBUG	volume/manager.go:244	Deleted task for 1025039b-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.566Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "1025039b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d03d", volumeID: "2dcde7e8-bb37-4e47-80f1-05926580701f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.566Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk" as container volume with ID: "2dcde7e8-bb37-4e47-80f1-05926580701f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.567Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk" with CNS. VolumeID: "2dcde7e8-bb37-4e47-80f1-05926580701f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.567Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {2dcde7e8-bb37-4e47-80f1-05926580701f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk 2dcde7e8-bb37-4e47-80f1-05926580701f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.567Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2dcde7e8-bb37-4e47-80f1-05926580701f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk VolumeID:2dcde7e8-bb37-4e47-80f1-05926580701f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.585Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2dcde7e8-bb37-4e47-80f1-05926580701f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2dcde7e8-bb37-4e47-80f1-05926580701f UID:e38ab198-90d1-46d9-9c82-8b526c52d874 ResourceVersion:7158319 Generation:1 CreationTimestamp:2020-07-17 06:16:47 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:16:47 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk VolumeID:2dcde7e8-bb37-4e47-80f1-05926580701f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.585Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {2dcde7e8-bb37-4e47-80f1-05926580701f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2dcde7e8-bb37-4e47-80f1-05926580701f e38ab198-90d1-46d9-9c82-8b526c52d874 7158319 1 2020-07-17 06:16:47 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:16:47 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk 2dcde7e8-bb37-4e47-80f1-05926580701f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.585Z	DEBUG	syncer/util.go:46	FullSync: pv 2dcde7e8-bb37-4e47-80f1-05926580701f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.585Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.585Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.585Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468700)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "183e07fc-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000665b30)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:16:47.585Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:16:47.586Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ba9488b9-b4af-4fe4-a308-b41246340d37.vmdk", volumeID: "2dcde7e8-bb37-4e47-80f1-05926580701f" mapping in the cache
2020-07-17T06:16:47.600Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.720Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "183e07fc-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d079"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.720Z	DEBUG	volume/manager.go:244	Deleted task for 183e07fc-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.720Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "183e07fc-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d079", volumeID: "fe189431-6204-4fcb-9fb5-ab0bd2d7c15c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.720Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk" as container volume with ID: "fe189431-6204-4fcb-9fb5-ab0bd2d7c15c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.720Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk" with CNS. VolumeID: "fe189431-6204-4fcb-9fb5-ab0bd2d7c15c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.720Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {fe189431-6204-4fcb-9fb5-ab0bd2d7c15c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk fe189431-6204-4fcb-9fb5-ab0bd2d7c15c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.720Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fe189431-6204-4fcb-9fb5-ab0bd2d7c15c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk VolumeID:fe189431-6204-4fcb-9fb5-ab0bd2d7c15c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.731Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fe189431-6204-4fcb-9fb5-ab0bd2d7c15c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fe189431-6204-4fcb-9fb5-ab0bd2d7c15c UID:74b75719-3d0d-4cfc-9eb9-b2ee534e6127 ResourceVersion:7158363 Generation:1 CreationTimestamp:2020-07-17 06:17:01 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:17:01 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk VolumeID:fe189431-6204-4fcb-9fb5-ab0bd2d7c15c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.732Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {fe189431-6204-4fcb-9fb5-ab0bd2d7c15c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fe189431-6204-4fcb-9fb5-ab0bd2d7c15c 74b75719-3d0d-4cfc-9eb9-b2ee534e6127 7158363 1 2020-07-17 06:17:01 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:17:01 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk fe189431-6204-4fcb-9fb5-ab0bd2d7c15c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.732Z	DEBUG	syncer/util.go:46	FullSync: pv fe189431-6204-4fcb-9fb5-ab0bd2d7c15c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.732Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.732Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.733Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "20acbb5f-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc001181110)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:01.734Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:17:01.735Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-633f4a2d-d3d0-4e18-9094-59671483f195.vmdk", volumeID: "fe189431-6204-4fcb-9fb5-ab0bd2d7c15c" mapping in the cache
2020-07-17T06:17:01.757Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.929Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "20acbb5f-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d07d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.929Z	DEBUG	volume/manager.go:244	Deleted task for 20acbb5f-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.929Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "20acbb5f-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d07d", volumeID: "56572ecf-f46a-4a6e-b376-9f0df8a62f4c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.929Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk" as container volume with ID: "56572ecf-f46a-4a6e-b376-9f0df8a62f4c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.929Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk" with CNS. VolumeID: "56572ecf-f46a-4a6e-b376-9f0df8a62f4c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.929Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {56572ecf-f46a-4a6e-b376-9f0df8a62f4c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk 56572ecf-f46a-4a6e-b376-9f0df8a62f4c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.929Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:56572ecf-f46a-4a6e-b376-9f0df8a62f4c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk VolumeID:56572ecf-f46a-4a6e-b376-9f0df8a62f4c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.939Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:17:14.942Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk", volumeID: "56572ecf-f46a-4a6e-b376-9f0df8a62f4c" mapping in the cache
2020-07-17T06:17:14.945Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:56572ecf-f46a-4a6e-b376-9f0df8a62f4c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/56572ecf-f46a-4a6e-b376-9f0df8a62f4c UID:19f78e2a-1fc8-40d9-8fa2-0f5a255deebd ResourceVersion:7158403 Generation:1 CreationTimestamp:2020-07-17 06:17:14 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:17:14 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk VolumeID:56572ecf-f46a-4a6e-b376-9f0df8a62f4c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.945Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {56572ecf-f46a-4a6e-b376-9f0df8a62f4c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/56572ecf-f46a-4a6e-b376-9f0df8a62f4c 19f78e2a-1fc8-40d9-8fa2-0f5a255deebd 7158403 1 2020-07-17 06:17:14 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:17:14 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-8a37826f-ad68-48db-8c26-aeb4fa33cf7d.vmdk 56572ecf-f46a-4a6e-b376-9f0df8a62f4c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.946Z	DEBUG	syncer/util.go:46	FullSync: pv 56572ecf-f46a-4a6e-b376-9f0df8a62f4c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.946Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.946Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.947Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004682a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "288d07e8-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000475d40)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:14.961Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.753Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "288d07e8-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d07f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.753Z	DEBUG	volume/manager.go:244	Deleted task for 288d07e8-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.753Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "288d07e8-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d07f", volumeID: "14421314-eeb3-416d-8d2a-d517fc6a8ebd"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.753Z		INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk" as container volume with ID: "14421314-eeb3-416d-8d2a-d517fc6a8ebd"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.753Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk" with CNS. VolumeID: "14421314-eeb3-416d-8d2a-d517fc6a8ebd"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.754Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {14421314-eeb3-416d-8d2a-d517fc6a8ebd      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk 14421314-eeb3-416d-8d2a-d517fc6a8ebd}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.754Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:14421314-eeb3-416d-8d2a-d517fc6a8ebd GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk VolumeID:14421314-eeb3-416d-8d2a-d517fc6a8ebd}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.763Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:14421314-eeb3-416d-8d2a-d517fc6a8ebd GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/14421314-eeb3-416d-8d2a-d517fc6a8ebd UID:1e14de7c-086c-4bf3-a5ca-8c8df4990976 ResourceVersion:7158441 Generation:1 CreationTimestamp:2020-07-17 06:17:26 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:17:26 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk VolumeID:14421314-eeb3-416d-8d2a-d517fc6a8ebd}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.763Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {14421314-eeb3-416d-8d2a-d517fc6a8ebd   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/14421314-eeb3-416d-8d2a-d517fc6a8ebd 1e14de7c-086c-4bf3-a5ca-8c8df4990976 7158441 1 2020-07-17 06:17:26 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:17:26 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk 14421314-eeb3-416d-8d2a-d517fc6a8ebd}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.763Z	DEBUG	syncer/util.go:46	FullSync: pv 14421314-eeb3-416d-8d2a-d517fc6a8ebd is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.763Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.763Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.765Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "2f982126-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00051c150)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:26.775Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:17:26.776Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b80be286-32b2-42f1-a85a-e01794b1fe56.vmdk", volumeID: "14421314-eeb3-416d-8d2a-d517fc6a8ebd" mapping in the cache
2020-07-17T06:17:26.789Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.118Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "2f982126-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d08a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.118Z	DEBUG	volume/manager.go:244	Deleted task for 2f982126-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.118Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "2f982126-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d08a", volumeID: "77761643-d783-41c7-bd80-3cbbe29a619d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.118Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk" as container volume with ID: "77761643-d783-41c7-bd80-3cbbe29a619d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.118Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk" with CNS. VolumeID: "77761643-d783-41c7-bd80-3cbbe29a619d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.118Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {77761643-d783-41c7-bd80-3cbbe29a619d      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk 77761643-d783-41c7-bd80-3cbbe29a619d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.118Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:77761643-d783-41c7-bd80-3cbbe29a619d GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk VolumeID:77761643-d783-41c7-bd80-3cbbe29a619d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.130Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:17:38.131Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk", volumeID: "77761643-d783-41c7-bd80-3cbbe29a619d" mapping in the cache
2020-07-17T06:17:38.131Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:77761643-d783-41c7-bd80-3cbbe29a619d GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/77761643-d783-41c7-bd80-3cbbe29a619d UID:0a221b2c-0101-473d-887d-3bb0c6ab6133 ResourceVersion:7158479 Generation:1 CreationTimestamp:2020-07-17 06:17:38 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:17:38 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk VolumeID:77761643-d783-41c7-bd80-3cbbe29a619d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.132Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {77761643-d783-41c7-bd80-3cbbe29a619d   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/77761643-d783-41c7-bd80-3cbbe29a619d 0a221b2c-0101-473d-887d-3bb0c6ab6133 7158479 1 2020-07-17 06:17:38 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:17:38 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-3865da41-92fa-48ed-804a-6c8b0308eb50.vmdk 77761643-d783-41c7-bd80-3cbbe29a619d}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.132Z	DEBUG	syncer/util.go:46	FullSync: pv 77761643-d783-41c7-bd80-3cbbe29a619d is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.132Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.132Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.133Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "365eeeb1-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000571aa0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:38.146Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.023Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "365eeeb1-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d08c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.023Z	DEBUG	volume/manager.go:244	Deleted task for 365eeeb1-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.023Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "365eeeb1-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d08c", volumeID: "46a3fafb-893e-406b-b4d2-a8730c226040"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.023Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk" as container volume with ID: "46a3fafb-893e-406b-b4d2-a8730c226040"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.023Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk" with CNS. VolumeID: "46a3fafb-893e-406b-b4d2-a8730c226040"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.023Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {46a3fafb-893e-406b-b4d2-a8730c226040      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk 46a3fafb-893e-406b-b4d2-a8730c226040}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.023Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:46a3fafb-893e-406b-b4d2-a8730c226040 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk VolumeID:46a3fafb-893e-406b-b4d2-a8730c226040}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.035Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:17:56.040Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk", volumeID: "46a3fafb-893e-406b-b4d2-a8730c226040" mapping in the cache
2020-07-17T06:17:56.041Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:46a3fafb-893e-406b-b4d2-a8730c226040 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/46a3fafb-893e-406b-b4d2-a8730c226040 UID:4318262a-3fce-4b2a-82aa-1592067d45ef ResourceVersion:7158535 Generation:1 CreationTimestamp:2020-07-17 06:17:56 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:17:56 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk VolumeID:46a3fafb-893e-406b-b4d2-a8730c226040}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.041Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {46a3fafb-893e-406b-b4d2-a8730c226040   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/46a3fafb-893e-406b-b4d2-a8730c226040 4318262a-3fce-4b2a-82aa-1592067d45ef 7158535 1 2020-07-17 06:17:56 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:17:56 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f6416398-abc8-44ab-8845-f6065905c895.vmdk 46a3fafb-893e-406b-b4d2-a8730c226040}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.041Z	DEBUG	syncer/util.go:46	FullSync: pv 46a3fafb-893e-406b-b4d2-a8730c226040 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.041Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.041Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.041Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0007140e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "410b9a77-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b0810)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:17:56.056Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.928Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "410b9a77-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0ca"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.928Z	DEBUG	volume/manager.go:244	Deleted task for 410b9a77-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.928Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "410b9a77-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0ca", volumeID: "c17dc0d7-3888-4ad0-8fa2-44c38d8187df"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.928Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk" as container volume with ID: "c17dc0d7-3888-4ad0-8fa2-44c38d8187df"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.928Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk" with CNS. VolumeID: "c17dc0d7-3888-4ad0-8fa2-44c38d8187df"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.928Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {c17dc0d7-3888-4ad0-8fa2-44c38d8187df      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk c17dc0d7-3888-4ad0-8fa2-44c38d8187df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.928Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c17dc0d7-3888-4ad0-8fa2-44c38d8187df GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk VolumeID:c17dc0d7-3888-4ad0-8fa2-44c38d8187df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.946Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c17dc0d7-3888-4ad0-8fa2-44c38d8187df GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c17dc0d7-3888-4ad0-8fa2-44c38d8187df UID:a10fd9ed-cb7b-40db-b79c-e8e8fbc67c32 ResourceVersion:7158578 Generation:1 CreationTimestamp:2020-07-17 06:18:09 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:18:09 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk VolumeID:c17dc0d7-3888-4ad0-8fa2-44c38d8187df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.946Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {c17dc0d7-3888-4ad0-8fa2-44c38d8187df   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c17dc0d7-3888-4ad0-8fa2-44c38d8187df a10fd9ed-cb7b-40db-b79c-e8e8fbc67c32 7158578 1 2020-07-17 06:18:09 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:18:09 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk c17dc0d7-3888-4ad0-8fa2-44c38d8187df}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.946Z	DEBUG	syncer/util.go:46	FullSync: pv c17dc0d7-3888-4ad0-8fa2-44c38d8187df is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.946Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.948Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.952Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004688c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "495566ac-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005e9d70)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:09.953Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:18:09.953Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a019bd48-58f1-49a7-bf97-ce4239ad87c0.vmdk", volumeID: "c17dc0d7-3888-4ad0-8fa2-44c38d8187df" mapping in the cache
2020-07-17T06:18:09.968Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.156Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "495566ac-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0ce"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.156Z	DEBUG	volume/manager.go:244	Deleted task for 495566ac-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.156Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "495566ac-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0ce", volumeID: "12495137-5258-42ed-b367-33e207747f81"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.156Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk" as container volume with ID: "12495137-5258-42ed-b367-33e207747f81"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.156Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk" with CNS. VolumeID: "12495137-5258-42ed-b367-33e207747f81"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.156Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {12495137-5258-42ed-b367-33e207747f81      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk 12495137-5258-42ed-b367-33e207747f81}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.156Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:12495137-5258-42ed-b367-33e207747f81 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk VolumeID:12495137-5258-42ed-b367-33e207747f81}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.175Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:18:24.175Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk", volumeID: "12495137-5258-42ed-b367-33e207747f81" mapping in the cache
2020-07-17T06:18:24.175Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:12495137-5258-42ed-b367-33e207747f81 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/12495137-5258-42ed-b367-33e207747f81 UID:82a144c5-1bb3-4ea6-9615-69c601eb4d56 ResourceVersion:7158623 Generation:1 CreationTimestamp:2020-07-17 06:18:24 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:18:24 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk VolumeID:12495137-5258-42ed-b367-33e207747f81}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.175Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {12495137-5258-42ed-b367-33e207747f81   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/12495137-5258-42ed-b367-33e207747f81 82a144c5-1bb3-4ea6-9615-69c601eb4d56 7158623 1 2020-07-17 06:18:24 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:18:24 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b1884811-1862-429d-91ef-fff76ac74f0e.vmdk 12495137-5258-42ed-b367-33e207747f81}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.175Z	DEBUG	syncer/util.go:46	FullSync: pv 12495137-5258-42ed-b367-33e207747f81 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.175Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.175Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.176Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000714380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "51d08cb9-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00089a600)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:24.195Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.647Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "51d08cb9-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0e6"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.647Z	DEBUG	volume/manager.go:244	Deleted task for 51d08cb9-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.647Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "51d08cb9-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0e6", volumeID: "d3f6dca0-e34b-455a-a980-56eb9c6f06e5"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.647Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk" as container volume with ID: "d3f6dca0-e34b-455a-a980-56eb9c6f06e5"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.647Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk" with CNS. VolumeID: "d3f6dca0-e34b-455a-a980-56eb9c6f06e5"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.647Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {d3f6dca0-e34b-455a-a980-56eb9c6f06e5      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk d3f6dca0-e34b-455a-a980-56eb9c6f06e5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.647Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d3f6dca0-e34b-455a-a980-56eb9c6f06e5 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk VolumeID:d3f6dca0-e34b-455a-a980-56eb9c6f06e5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.657Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d3f6dca0-e34b-455a-a980-56eb9c6f06e5 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d3f6dca0-e34b-455a-a980-56eb9c6f06e5 UID:c7bef2e2-e11e-432e-8cd8-d94333af9e9e ResourceVersion:7158662 Generation:1 CreationTimestamp:2020-07-17 06:18:36 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:18:36 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk VolumeID:d3f6dca0-e34b-455a-a980-56eb9c6f06e5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.659Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {d3f6dca0-e34b-455a-a980-56eb9c6f06e5   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d3f6dca0-e34b-455a-a980-56eb9c6f06e5 c7bef2e2-e11e-432e-8cd8-d94333af9e9e 7158662 1 2020-07-17 06:18:36 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:18:36 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk d3f6dca0-e34b-455a-a980-56eb9c6f06e5}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.661Z	DEBUG	syncer/util.go:46	FullSync: pv d3f6dca0-e34b-455a-a980-56eb9c6f06e5 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.661Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.662Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.662Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468b60)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "5941cda6-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000989590)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:36.657Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:18:36.668Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-6b191d75-1c1c-4930-aa39-36cd26fc6f96.vmdk", volumeID: "d3f6dca0-e34b-455a-a980-56eb9c6f06e5" mapping in the cache
2020-07-17T06:18:36.678Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.591Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "5941cda6-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0f1"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.591Z	DEBUG	volume/manager.go:244	Deleted task for 5941cda6-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.591Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "5941cda6-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d0f1", volumeID: "d7658a4c-6a8f-4271-8bee-3f5328754e7f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.591Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk" as container volume with ID: "d7658a4c-6a8f-4271-8bee-3f5328754e7f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.591Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk" with CNS. VolumeID: "d7658a4c-6a8f-4271-8bee-3f5328754e7f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.591Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {d7658a4c-6a8f-4271-8bee-3f5328754e7f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk d7658a4c-6a8f-4271-8bee-3f5328754e7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.591Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d7658a4c-6a8f-4271-8bee-3f5328754e7f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk VolumeID:d7658a4c-6a8f-4271-8bee-3f5328754e7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.606Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:18:48.606Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk", volumeID: "d7658a4c-6a8f-4271-8bee-3f5328754e7f" mapping in the cache
2020-07-17T06:18:48.611Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:d7658a4c-6a8f-4271-8bee-3f5328754e7f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d7658a4c-6a8f-4271-8bee-3f5328754e7f UID:43ddff4e-0c9e-4431-b567-974be2621209 ResourceVersion:7158701 Generation:1 CreationTimestamp:2020-07-17 06:18:48 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:18:48 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk VolumeID:d7658a4c-6a8f-4271-8bee-3f5328754e7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.612Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {d7658a4c-6a8f-4271-8bee-3f5328754e7f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/d7658a4c-6a8f-4271-8bee-3f5328754e7f 43ddff4e-0c9e-4431-b567-974be2621209 7158701 1 2020-07-17 06:18:48 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:18:48 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0732a14b-a4b2-4993-b8ac-d68e71cef795.vmdk d7658a4c-6a8f-4271-8bee-3f5328754e7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.612Z	DEBUG	syncer/util.go:46	FullSync: pv d7658a4c-6a8f-4271-8bee-3f5328754e7f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.613Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.613Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.614Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468c40)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "60618004-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007dfb90)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:18:48.634Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.941Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "60618004-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d12c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.941Z	DEBUG	volume/manager.go:244	Deleted task for 60618004-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.941Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "60618004-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d12c", volumeID: "f62304f9-3e65-42c3-af27-a1169385d64c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.941Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk" as container volume with ID: "f62304f9-3e65-42c3-af27-a1169385d64c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.941Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk" with CNS. VolumeID: "f62304f9-3e65-42c3-af27-a1169385d64c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.941Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {f62304f9-3e65-42c3-af27-a1169385d64c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk f62304f9-3e65-42c3-af27-a1169385d64c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.941Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f62304f9-3e65-42c3-af27-a1169385d64c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk VolumeID:f62304f9-3e65-42c3-af27-a1169385d64c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.952Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:19:00.956Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk", volumeID: "f62304f9-3e65-42c3-af27-a1169385d64c" mapping in the cache
2020-07-17T06:19:00.956Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:f62304f9-3e65-42c3-af27-a1169385d64c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f62304f9-3e65-42c3-af27-a1169385d64c UID:701f55b4-061d-4326-b14a-59a34f580746 ResourceVersion:7158741 Generation:1 CreationTimestamp:2020-07-17 06:19:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:19:00 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk VolumeID:f62304f9-3e65-42c3-af27-a1169385d64c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.956Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {f62304f9-3e65-42c3-af27-a1169385d64c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/f62304f9-3e65-42c3-af27-a1169385d64c 701f55b4-061d-4326-b14a-59a34f580746 7158741 1 2020-07-17 06:19:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:19:00 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b2a2165b-4fd3-433d-80eb-98d5f1f1c94f.vmdk f62304f9-3e65-42c3-af27-a1169385d64c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.956Z	DEBUG	syncer/util.go:46	FullSync: pv f62304f9-3e65-42c3-af27-a1169385d64c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.956Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.956Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.956Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "67bcda80-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000a3cc90)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:00.978Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.660Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "67bcda80-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d130"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.660Z	DEBUG	volume/manager.go:244	Deleted task for 67bcda80-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.660Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "67bcda80-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d130", volumeID: "6f5db5b1-b4cf-493a-8eae-76c0140dfd7f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.661Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk" as container volume with ID: "6f5db5b1-b4cf-493a-8eae-76c0140dfd7f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.663Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk" with CNS. VolumeID: "6f5db5b1-b4cf-493a-8eae-76c0140dfd7f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.664Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {6f5db5b1-b4cf-493a-8eae-76c0140dfd7f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk 6f5db5b1-b4cf-493a-8eae-76c0140dfd7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.665Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6f5db5b1-b4cf-493a-8eae-76c0140dfd7f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk VolumeID:6f5db5b1-b4cf-493a-8eae-76c0140dfd7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.679Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:19:14.679Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk", volumeID: "6f5db5b1-b4cf-493a-8eae-76c0140dfd7f" mapping in the cache
2020-07-17T06:19:14.680Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:6f5db5b1-b4cf-493a-8eae-76c0140dfd7f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6f5db5b1-b4cf-493a-8eae-76c0140dfd7f UID:af8e86e2-1f42-4f44-b830-a2b933012a90 ResourceVersion:7158784 Generation:1 CreationTimestamp:2020-07-17 06:19:14 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:19:14 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk VolumeID:6f5db5b1-b4cf-493a-8eae-76c0140dfd7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.680Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {6f5db5b1-b4cf-493a-8eae-76c0140dfd7f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/6f5db5b1-b4cf-493a-8eae-76c0140dfd7f af8e86e2-1f42-4f44-b830-a2b933012a90 7158784 1 2020-07-17 06:19:14 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:19:14 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-913ee73c-28c7-4c23-9d45-9c914ee8b5a5.vmdk 6f5db5b1-b4cf-493a-8eae-76c0140dfd7f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.680Z	DEBUG	syncer/util.go:46	FullSync: pv 6f5db5b1-b4cf-493a-8eae-76c0140dfd7f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.680Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.680Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.681Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "6feb03f1-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007df650)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:14.698Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.904Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "6feb03f1-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d132"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.904Z	DEBUG	volume/manager.go:244	Deleted task for 6feb03f1-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.904Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "6feb03f1-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d132", volumeID: "c1b30e0b-61b1-4aac-9ec6-267d483f424b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.905Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk" as container volume with ID: "c1b30e0b-61b1-4aac-9ec6-267d483f424b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.905Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk" with CNS. VolumeID: "c1b30e0b-61b1-4aac-9ec6-267d483f424b"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.905Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {c1b30e0b-61b1-4aac-9ec6-267d483f424b      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk c1b30e0b-61b1-4aac-9ec6-267d483f424b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.906Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c1b30e0b-61b1-4aac-9ec6-267d483f424b GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk VolumeID:c1b30e0b-61b1-4aac-9ec6-267d483f424b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.929Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c1b30e0b-61b1-4aac-9ec6-267d483f424b GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c1b30e0b-61b1-4aac-9ec6-267d483f424b UID:1d947869-de1e-44d7-9344-69bc3d7a3811 ResourceVersion:7158818 Generation:1 CreationTimestamp:2020-07-17 06:19:24 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:19:24 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk VolumeID:c1b30e0b-61b1-4aac-9ec6-267d483f424b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.929Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {c1b30e0b-61b1-4aac-9ec6-267d483f424b   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c1b30e0b-61b1-4aac-9ec6-267d483f424b 1d947869-de1e-44d7-9344-69bc3d7a3811 7158818 1 2020-07-17 06:19:24 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:19:24 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk c1b30e0b-61b1-4aac-9ec6-267d483f424b}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.929Z	DEBUG	syncer/util.go:46	FullSync: pv c1b30e0b-61b1-4aac-9ec6-267d483f424b is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.929Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.929Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.929Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468460)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "7606c619-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000defcb0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:24.929Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:19:24.929Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f4b21f23-ed86-4a9b-b0d6-6ce3b2d6352b.vmdk", volumeID: "c1b30e0b-61b1-4aac-9ec6-267d483f424b" mapping in the cache
2020-07-17T06:19:24.955Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.831Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "7606c619-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d134"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.831Z	DEBUG	volume/manager.go:244	Deleted task for 7606c619-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.831Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "7606c619-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d134", volumeID: "64866606-0d50-4ded-b3dc-54f56a5a960c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.831Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk" as container volume with ID: "64866606-0d50-4ded-b3dc-54f56a5a960c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.831Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk" with CNS. VolumeID: "64866606-0d50-4ded-b3dc-54f56a5a960c"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.831Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {64866606-0d50-4ded-b3dc-54f56a5a960c      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk 64866606-0d50-4ded-b3dc-54f56a5a960c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.831Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:64866606-0d50-4ded-b3dc-54f56a5a960c GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk VolumeID:64866606-0d50-4ded-b3dc-54f56a5a960c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.843Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:19:41.844Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk", volumeID: "64866606-0d50-4ded-b3dc-54f56a5a960c" mapping in the cache
2020-07-17T06:19:41.844Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:64866606-0d50-4ded-b3dc-54f56a5a960c GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/64866606-0d50-4ded-b3dc-54f56a5a960c UID:8bfe4ef1-c5e3-4ade-84ce-0a8ddf064235 ResourceVersion:7158873 Generation:1 CreationTimestamp:2020-07-17 06:19:41 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:19:41 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk VolumeID:64866606-0d50-4ded-b3dc-54f56a5a960c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.845Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {64866606-0d50-4ded-b3dc-54f56a5a960c   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/64866606-0d50-4ded-b3dc-54f56a5a960c 8bfe4ef1-c5e3-4ade-84ce-0a8ddf064235 7158873 1 2020-07-17 06:19:41 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:19:41 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-f1691cfa-f126-4323-81dd-c3258455c9cb.vmdk 64866606-0d50-4ded-b3dc-54f56a5a960c}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.845Z	DEBUG	syncer/util.go:46	FullSync: pv 64866606-0d50-4ded-b3dc-54f56a5a960c is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.845Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.845Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.846Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc00096a0e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "801bf7e4-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dbf4d0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:41.867Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.250Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "801bf7e4-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d142"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.250Z	DEBUG	volume/manager.go:244	Deleted task for 801bf7e4-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.250Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "801bf7e4-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d142", volumeID: "9e03b7f4-62f4-4788-a647-ab4be40279ab"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.250Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk" as container volume with ID: "9e03b7f4-62f4-4788-a647-ab4be40279ab"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.250Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk" with CNS. VolumeID: "9e03b7f4-62f4-4788-a647-ab4be40279ab"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.250Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {9e03b7f4-62f4-4788-a647-ab4be40279ab      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk 9e03b7f4-62f4-4788-a647-ab4be40279ab}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.250Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:9e03b7f4-62f4-4788-a647-ab4be40279ab GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk VolumeID:9e03b7f4-62f4-4788-a647-ab4be40279ab}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.263Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:9e03b7f4-62f4-4788-a647-ab4be40279ab GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/9e03b7f4-62f4-4788-a647-ab4be40279ab UID:968a00ab-286e-49d5-96de-9433ed9cda9a ResourceVersion:7158918 Generation:1 CreationTimestamp:2020-07-17 06:19:56 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:19:56 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk VolumeID:9e03b7f4-62f4-4788-a647-ab4be40279ab}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.263Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {9e03b7f4-62f4-4788-a647-ab4be40279ab   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/9e03b7f4-62f4-4788-a647-ab4be40279ab 968a00ab-286e-49d5-96de-9433ed9cda9a 7158918 1 2020-07-17 06:19:56 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:19:56 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk 9e03b7f4-62f4-4788-a647-ab4be40279ab}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.263Z	DEBUG	syncer/util.go:46	FullSync: pv 9e03b7f4-62f4-4788-a647-ab4be40279ab is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.263Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.264Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.275Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "88b41514-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000479170)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:19:56.275Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:19:56.276Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-e9330103-272e-46bf-be4c-60eade7770f2.vmdk", volumeID: "9e03b7f4-62f4-4788-a647-ab4be40279ab" mapping in the cache
2020-07-17T06:19:56.294Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.044Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "88b41514-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d175"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.044Z	DEBUG	volume/manager.go:244	Deleted task for 88b41514-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.044Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "88b41514-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d175", volumeID: "ec598873-6d51-4feb-ba7b-6a24242f56de"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.044Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk" as container volume with ID: "ec598873-6d51-4feb-ba7b-6a24242f56de"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.044Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk" with CNS. VolumeID: "ec598873-6d51-4feb-ba7b-6a24242f56de"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.044Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {ec598873-6d51-4feb-ba7b-6a24242f56de      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk ec598873-6d51-4feb-ba7b-6a24242f56de}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.044Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:ec598873-6d51-4feb-ba7b-6a24242f56de GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk VolumeID:ec598873-6d51-4feb-ba7b-6a24242f56de}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.058Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:ec598873-6d51-4feb-ba7b-6a24242f56de GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/ec598873-6d51-4feb-ba7b-6a24242f56de UID:67e77642-5620-4edc-82c8-24eca8fda758 ResourceVersion:7158971 Generation:1 CreationTimestamp:2020-07-17 06:20:13 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:20:13 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk VolumeID:ec598873-6d51-4feb-ba7b-6a24242f56de}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.062Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {ec598873-6d51-4feb-ba7b-6a24242f56de   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/ec598873-6d51-4feb-ba7b-6a24242f56de 67e77642-5620-4edc-82c8-24eca8fda758 7158971 1 2020-07-17 06:20:13 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:20:13 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk ec598873-6d51-4feb-ba7b-6a24242f56de}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.062Z	DEBUG	syncer/util.go:46	FullSync: pv ec598873-6d51-4feb-ba7b-6a24242f56de is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.062Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.062Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.062Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004687e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "92b75b86-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0007a2a20)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:13.060Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:20:13.063Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-202b6f6f-67fb-4acd-b586-1d464ee889fe.vmdk", volumeID: "ec598873-6d51-4feb-ba7b-6a24242f56de" mapping in the cache
2020-07-17T06:20:13.096Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:27.986Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "92b75b86-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d179"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:27.987Z	DEBUG	volume/manager.go:244	Deleted task for 92b75b86-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:27.987Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "92b75b86-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d179", volumeID: "186a800b-2022-48cf-b86c-58a15c60ea0a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:27.987Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk" as container volume with ID: "186a800b-2022-48cf-b86c-58a15c60ea0a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:27.987Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk" with CNS. VolumeID: "186a800b-2022-48cf-b86c-58a15c60ea0a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:27.987Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {186a800b-2022-48cf-b86c-58a15c60ea0a      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk 186a800b-2022-48cf-b86c-58a15c60ea0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:27.987Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:186a800b-2022-48cf-b86c-58a15c60ea0a GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk VolumeID:186a800b-2022-48cf-b86c-58a15c60ea0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:28.000Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:20:28.001Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk", volumeID: "186a800b-2022-48cf-b86c-58a15c60ea0a" mapping in the cache
2020-07-17T06:20:28.002Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:186a800b-2022-48cf-b86c-58a15c60ea0a GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/186a800b-2022-48cf-b86c-58a15c60ea0a UID:a7262bef-d0bb-4d4f-b82f-c72e6987a67f ResourceVersion:7159018 Generation:1 CreationTimestamp:2020-07-17 06:20:27 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:20:27 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk VolumeID:186a800b-2022-48cf-b86c-58a15c60ea0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:28.002Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {186a800b-2022-48cf-b86c-58a15c60ea0a   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/186a800b-2022-48cf-b86c-58a15c60ea0a a7262bef-d0bb-4d4f-b82f-c72e6987a67f 7159018 1 2020-07-17 06:20:27 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:20:27 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-609a28d8-2248-4a28-a0bf-12eee2166106.vmdk 186a800b-2022-48cf-b86c-58a15c60ea0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:28.002Z	DEBUG	syncer/util.go:46	FullSync: pv 186a800b-2022-48cf-b86c-58a15c60ea0a is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:28.003Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:28.005Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:28.006Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468a80)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "9b9f4f0c-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0003fccc0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:28.031Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.094Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "9b9f4f0c-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d191"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.094Z	DEBUG	volume/manager.go:244	Deleted task for 9b9f4f0c-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.094Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "9b9f4f0c-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d191", volumeID: "92baea7a-bf0d-4f19-b398-86763f2baf86"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.095Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk" as container volume with ID: "92baea7a-bf0d-4f19-b398-86763f2baf86"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.095Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk" with CNS. VolumeID: "92baea7a-bf0d-4f19-b398-86763f2baf86"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.095Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {92baea7a-bf0d-4f19-b398-86763f2baf86      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk 92baea7a-bf0d-4f19-b398-86763f2baf86}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.095Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:92baea7a-bf0d-4f19-b398-86763f2baf86 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk VolumeID:92baea7a-bf0d-4f19-b398-86763f2baf86}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.105Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:92baea7a-bf0d-4f19-b398-86763f2baf86 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/92baea7a-bf0d-4f19-b398-86763f2baf86 UID:fe39ac59-5000-49d2-9596-4ef994d826d3 ResourceVersion:7159066 Generation:1 CreationTimestamp:2020-07-17 06:20:44 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:20:44 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk VolumeID:92baea7a-bf0d-4f19-b398-86763f2baf86}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.106Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {92baea7a-bf0d-4f19-b398-86763f2baf86   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/92baea7a-bf0d-4f19-b398-86763f2baf86 fe39ac59-5000-49d2-9596-4ef994d826d3 7159066 1 2020-07-17 06:20:44 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:20:44 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk 92baea7a-bf0d-4f19-b398-86763f2baf86}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.106Z	DEBUG	syncer/util.go:46	FullSync: pv 92baea7a-bf0d-4f19-b398-86763f2baf86 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.106Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.110Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.111Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468d20)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "a538e1a9-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00059d7a0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:44.109Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:20:44.111Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-952045e0-f7d5-42a9-91f0-6cd9b64a2a74.vmdk", volumeID: "92baea7a-bf0d-4f19-b398-86763f2baf86" mapping in the cache
2020-07-17T06:20:44.140Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.785Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "a538e1a9-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1d8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.786Z	DEBUG	volume/manager.go:244	Deleted task for a538e1a9-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.786Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "a538e1a9-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1d8", volumeID: "bd27d3bf-6a97-4556-a22d-09c0d3790015"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.786Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk" as container volume with ID: "bd27d3bf-6a97-4556-a22d-09c0d3790015"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.786Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk" with CNS. VolumeID: "bd27d3bf-6a97-4556-a22d-09c0d3790015"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.787Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {bd27d3bf-6a97-4556-a22d-09c0d3790015      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk bd27d3bf-6a97-4556-a22d-09c0d3790015}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.787Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bd27d3bf-6a97-4556-a22d-09c0d3790015 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk VolumeID:bd27d3bf-6a97-4556-a22d-09c0d3790015}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.801Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:20:57.808Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk", volumeID: "bd27d3bf-6a97-4556-a22d-09c0d3790015" mapping in the cache
2020-07-17T06:20:57.807Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bd27d3bf-6a97-4556-a22d-09c0d3790015 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bd27d3bf-6a97-4556-a22d-09c0d3790015 UID:8055f9de-c3d0-44d6-9fc8-1a38fc61046d ResourceVersion:7159110 Generation:1 CreationTimestamp:2020-07-17 06:20:57 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:20:57 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk VolumeID:bd27d3bf-6a97-4556-a22d-09c0d3790015}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.808Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {bd27d3bf-6a97-4556-a22d-09c0d3790015   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bd27d3bf-6a97-4556-a22d-09c0d3790015 8055f9de-c3d0-44d6-9fc8-1a38fc61046d 7159110 1 2020-07-17 06:20:57 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:20:57 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-5c537eac-c193-4f76-baf8-93f3019288e7.vmdk bd27d3bf-6a97-4556-a22d-09c0d3790015}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.808Z	DEBUG	syncer/util.go:46	FullSync: pv bd27d3bf-6a97-4556-a22d-09c0d3790015 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.808Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.808Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.808Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc00096a2a0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "ad63043b-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00117cc90)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:20:57.830Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.116Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "ad63043b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1da"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.117Z	DEBUG	volume/manager.go:244	Deleted task for ad63043b-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.117Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "ad63043b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1da", volumeID: "a5548fcf-96da-463a-a5a5-e9faea2915af"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.117Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk" as container volume with ID: "a5548fcf-96da-463a-a5a5-e9faea2915af"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.117Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk" with CNS. VolumeID: "a5548fcf-96da-463a-a5a5-e9faea2915af"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.117Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {a5548fcf-96da-463a-a5a5-e9faea2915af      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk a5548fcf-96da-463a-a5a5-e9faea2915af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.117Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:a5548fcf-96da-463a-a5a5-e9faea2915af GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk VolumeID:a5548fcf-96da-463a-a5a5-e9faea2915af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.134Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:21:12.140Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk", volumeID: "a5548fcf-96da-463a-a5a5-e9faea2915af" mapping in the cache
2020-07-17T06:21:12.140Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:a5548fcf-96da-463a-a5a5-e9faea2915af GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/a5548fcf-96da-463a-a5a5-e9faea2915af UID:5bca07ae-2339-4f47-bad4-7075d7e62b0c ResourceVersion:7159156 Generation:1 CreationTimestamp:2020-07-17 06:21:12 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:21:12 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk VolumeID:a5548fcf-96da-463a-a5a5-e9faea2915af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.140Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {a5548fcf-96da-463a-a5a5-e9faea2915af   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/a5548fcf-96da-463a-a5a5-e9faea2915af 5bca07ae-2339-4f47-bad4-7075d7e62b0c 7159156 1 2020-07-17 06:21:12 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:21:12 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-22d425c4-6cd1-4002-bc94-08cbd9efc3a1.vmdk a5548fcf-96da-463a-a5a5-e9faea2915af}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.140Z	DEBUG	syncer/util.go:46	FullSync: pv a5548fcf-96da-463a-a5a5-e9faea2915af is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.141Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.141Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.141Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "b5ee00ec-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00069c0f0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:12.160Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.932Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "b5ee00ec-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1de"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.932Z	DEBUG	volume/manager.go:244	Deleted task for b5ee00ec-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.932Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "b5ee00ec-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1de", volumeID: "bcb3384a-128d-4cfb-88cd-9edcfc120bb9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.932Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk" as container volume with ID: "bcb3384a-128d-4cfb-88cd-9edcfc120bb9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.932Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk" with CNS. VolumeID: "bcb3384a-128d-4cfb-88cd-9edcfc120bb9"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.932Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {bcb3384a-128d-4cfb-88cd-9edcfc120bb9      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk bcb3384a-128d-4cfb-88cd-9edcfc120bb9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.932Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bcb3384a-128d-4cfb-88cd-9edcfc120bb9 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk VolumeID:bcb3384a-128d-4cfb-88cd-9edcfc120bb9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.945Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bcb3384a-128d-4cfb-88cd-9edcfc120bb9 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bcb3384a-128d-4cfb-88cd-9edcfc120bb9 UID:d537d524-167a-42f3-9d66-77163f71a3c1 ResourceVersion:7159197 Generation:1 CreationTimestamp:2020-07-17 06:21:24 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:21:24 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk VolumeID:bcb3384a-128d-4cfb-88cd-9edcfc120bb9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.945Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {bcb3384a-128d-4cfb-88cd-9edcfc120bb9   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bcb3384a-128d-4cfb-88cd-9edcfc120bb9 d537d524-167a-42f3-9d66-77163f71a3c1 7159197 1 2020-07-17 06:21:24 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:21:24 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk bcb3384a-128d-4cfb-88cd-9edcfc120bb9}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.945Z	DEBUG	syncer/util.go:46	FullSync: pv bcb3384a-128d-4cfb-88cd-9edcfc120bb9 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.945Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.945Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.948Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "bd8fd3cc-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00030a600)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:24.950Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:21:24.950Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-ad93aef4-253f-4d93-9ea5-8812f1c7e1b7.vmdk", volumeID: "bcb3384a-128d-4cfb-88cd-9edcfc120bb9" mapping in the cache
2020-07-17T06:21:24.964Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.093Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "bd8fd3cc-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1e0"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.093Z	DEBUG	volume/manager.go:244	Deleted task for bd8fd3cc-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.093Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "bd8fd3cc-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1e0", volumeID: "55365f89-bae2-463e-8e9e-a1c59644612e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.093Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk" as container volume with ID: "55365f89-bae2-463e-8e9e-a1c59644612e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.093Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk" with CNS. VolumeID: "55365f89-bae2-463e-8e9e-a1c59644612e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.093Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {55365f89-bae2-463e-8e9e-a1c59644612e      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk 55365f89-bae2-463e-8e9e-a1c59644612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.093Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:55365f89-bae2-463e-8e9e-a1c59644612e GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk VolumeID:55365f89-bae2-463e-8e9e-a1c59644612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.107Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:21:39.107Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk", volumeID: "55365f89-bae2-463e-8e9e-a1c59644612e" mapping in the cache
2020-07-17T06:21:39.111Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:55365f89-bae2-463e-8e9e-a1c59644612e GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/55365f89-bae2-463e-8e9e-a1c59644612e UID:9013e25d-df03-4a6d-a560-d4d35f8edbb7 ResourceVersion:7159241 Generation:1 CreationTimestamp:2020-07-17 06:21:39 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:21:39 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk VolumeID:55365f89-bae2-463e-8e9e-a1c59644612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.111Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {55365f89-bae2-463e-8e9e-a1c59644612e   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/55365f89-bae2-463e-8e9e-a1c59644612e 9013e25d-df03-4a6d-a560-d4d35f8edbb7 7159241 1 2020-07-17 06:21:39 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:21:39 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9b1fb4eb-10dc-4b5d-9472-b13fa45bb6da.vmdk 55365f89-bae2-463e-8e9e-a1c59644612e}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.111Z	DEBUG	syncer/util.go:46	FullSync: pv 55365f89-bae2-463e-8e9e-a1c59644612e is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.111Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.111Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.114Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc00096a000)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "c6016109-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0003fd7a0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:39.144Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.728Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "c6016109-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1e4"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.729Z	DEBUG	volume/manager.go:244	Deleted task for c6016109-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.729Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "c6016109-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d1e4", volumeID: "bb8c789e-5b93-493c-b09e-a0c114b5babf"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.729Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk" as container volume with ID: "bb8c789e-5b93-493c-b09e-a0c114b5babf"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.729Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk" with CNS. VolumeID: "bb8c789e-5b93-493c-b09e-a0c114b5babf"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.729Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {bb8c789e-5b93-493c-b09e-a0c114b5babf      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk bb8c789e-5b93-493c-b09e-a0c114b5babf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.729Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bb8c789e-5b93-493c-b09e-a0c114b5babf GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk VolumeID:bb8c789e-5b93-493c-b09e-a0c114b5babf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.745Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:bb8c789e-5b93-493c-b09e-a0c114b5babf GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bb8c789e-5b93-493c-b09e-a0c114b5babf UID:0cfc10f7-e0e7-428c-9024-2c1f59ac9921 ResourceVersion:7159287 Generation:1 CreationTimestamp:2020-07-17 06:21:53 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:21:53 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk VolumeID:bb8c789e-5b93-493c-b09e-a0c114b5babf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.745Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {bb8c789e-5b93-493c-b09e-a0c114b5babf   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/bb8c789e-5b93-493c-b09e-a0c114b5babf 0cfc10f7-e0e7-428c-9024-2c1f59ac9921 7159287 1 2020-07-17 06:21:53 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:21:53 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk bb8c789e-5b93-493c-b09e-a0c114b5babf}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.745Z	DEBUG	syncer/util.go:46	FullSync: pv bb8c789e-5b93-493c-b09e-a0c114b5babf is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.745Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.745Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.746Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc00096a1c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "ceba4e7b-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002e32c0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:21:53.747Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:21:53.747Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-d2a6b16a-5414-461e-be77-1436d09f5f5b.vmdk", volumeID: "bb8c789e-5b93-493c-b09e-a0c114b5babf" mapping in the cache
2020-07-17T06:21:53.772Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.901Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "ceba4e7b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d221"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.901Z	DEBUG	volume/manager.go:244	Deleted task for ceba4e7b-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.901Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "ceba4e7b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d221", volumeID: "7cf07a65-1c72-4584-b549-baff8cc87010"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.901Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk" as container volume with ID: "7cf07a65-1c72-4584-b549-baff8cc87010"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.901Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk" with CNS. VolumeID: "7cf07a65-1c72-4584-b549-baff8cc87010"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.901Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {7cf07a65-1c72-4584-b549-baff8cc87010      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk 7cf07a65-1c72-4584-b549-baff8cc87010}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.902Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7cf07a65-1c72-4584-b549-baff8cc87010 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk VolumeID:7cf07a65-1c72-4584-b549-baff8cc87010}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.915Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:7cf07a65-1c72-4584-b549-baff8cc87010 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7cf07a65-1c72-4584-b549-baff8cc87010 UID:7d09bab2-3c05-4433-ae19-5e34afbc8989 ResourceVersion:7159337 Generation:1 CreationTimestamp:2020-07-17 06:22:09 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:22:09 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk VolumeID:7cf07a65-1c72-4584-b549-baff8cc87010}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.915Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {7cf07a65-1c72-4584-b549-baff8cc87010   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/7cf07a65-1c72-4584-b549-baff8cc87010 7d09bab2-3c05-4433-ae19-5e34afbc8989 7159337 1 2020-07-17 06:22:09 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:22:09 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk 7cf07a65-1c72-4584-b549-baff8cc87010}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.915Z	DEBUG	syncer/util.go:46	FullSync: pv 7cf07a65-1c72-4584-b549-baff8cc87010 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.916Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.916Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.917Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "d85dc963-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b1920)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:09.919Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:22:09.919Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0d759d0b-2361-40eb-aee4-91d60e0504d2.vmdk", volumeID: "7cf07a65-1c72-4584-b549-baff8cc87010" mapping in the cache
2020-07-17T06:22:09.944Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.484Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "d85dc963-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d225"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.484Z	DEBUG	volume/manager.go:244	Deleted task for d85dc963-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.484Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "d85dc963-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d225", volumeID: "35bfcfe3-0c81-4e78-96fe-3dec5dac9039"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.484Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk" as container volume with ID: "35bfcfe3-0c81-4e78-96fe-3dec5dac9039"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.484Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk" with CNS. VolumeID: "35bfcfe3-0c81-4e78-96fe-3dec5dac9039"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.484Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {35bfcfe3-0c81-4e78-96fe-3dec5dac9039      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk 35bfcfe3-0c81-4e78-96fe-3dec5dac9039}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.484Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:35bfcfe3-0c81-4e78-96fe-3dec5dac9039 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk VolumeID:35bfcfe3-0c81-4e78-96fe-3dec5dac9039}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.494Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:22:22.494Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk", volumeID: "35bfcfe3-0c81-4e78-96fe-3dec5dac9039" mapping in the cache
2020-07-17T06:22:22.505Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:35bfcfe3-0c81-4e78-96fe-3dec5dac9039 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/35bfcfe3-0c81-4e78-96fe-3dec5dac9039 UID:b6633cd9-2459-4de3-90e4-54943e0f750d ResourceVersion:7159381 Generation:1 CreationTimestamp:2020-07-17 06:22:22 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:22:22 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk VolumeID:35bfcfe3-0c81-4e78-96fe-3dec5dac9039}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.505Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {35bfcfe3-0c81-4e78-96fe-3dec5dac9039   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/35bfcfe3-0c81-4e78-96fe-3dec5dac9039 b6633cd9-2459-4de3-90e4-54943e0f750d 7159381 1 2020-07-17 06:22:22 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:22:22 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-cb1b8bbc-b28c-4568-838a-0934898f1522.vmdk 35bfcfe3-0c81-4e78-96fe-3dec5dac9039}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.505Z	DEBUG	syncer/util.go:46	FullSync: pv 35bfcfe3-0c81-4e78-96fe-3dec5dac9039 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.505Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.505Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.505Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "dfdebf5b-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dd3170)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:22.529Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.237Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "dfdebf5b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d23d"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.237Z	DEBUG	volume/manager.go:244	Deleted task for dfdebf5b-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.237Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "dfdebf5b-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d23d", volumeID: "e2f2ae90-964e-4241-acde-9386418cfd7a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.237Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk" as container volume with ID: "e2f2ae90-964e-4241-acde-9386418cfd7a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.238Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk" with CNS. VolumeID: "e2f2ae90-964e-4241-acde-9386418cfd7a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.238Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {e2f2ae90-964e-4241-acde-9386418cfd7a      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk e2f2ae90-964e-4241-acde-9386418cfd7a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.242Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e2f2ae90-964e-4241-acde-9386418cfd7a GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk VolumeID:e2f2ae90-964e-4241-acde-9386418cfd7a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.250Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:22:35.253Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk", volumeID: "e2f2ae90-964e-4241-acde-9386418cfd7a" mapping in the cache
2020-07-17T06:22:35.253Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:e2f2ae90-964e-4241-acde-9386418cfd7a GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e2f2ae90-964e-4241-acde-9386418cfd7a UID:5f50b747-84b2-4f35-90ab-e811a27e7b00 ResourceVersion:7159419 Generation:1 CreationTimestamp:2020-07-17 06:22:35 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:22:35 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk VolumeID:e2f2ae90-964e-4241-acde-9386418cfd7a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.253Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {e2f2ae90-964e-4241-acde-9386418cfd7a   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/e2f2ae90-964e-4241-acde-9386418cfd7a 5f50b747-84b2-4f35-90ab-e811a27e7b00 7159419 1 2020-07-17 06:22:35 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:22:35 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-720f43e0-dd13-4b4a-90d9-14f998f53f89.vmdk e2f2ae90-964e-4241-acde-9386418cfd7a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.253Z	DEBUG	syncer/util.go:46	FullSync: pv e2f2ae90-964e-4241-acde-9386418cfd7a is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.253Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.253Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.253Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc00096a540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "e777ff80-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0009881e0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:35.272Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.771Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "e777ff80-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d23f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.772Z	DEBUG	volume/manager.go:244	Deleted task for e777ff80-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.772Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "e777ff80-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d23f", volumeID: "0d87ba78-59ed-42f4-af6e-f31c9f4078b3"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.772Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk" as container volume with ID: "0d87ba78-59ed-42f4-af6e-f31c9f4078b3"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.772Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk" with CNS. VolumeID: "0d87ba78-59ed-42f4-af6e-f31c9f4078b3"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.772Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {0d87ba78-59ed-42f4-af6e-f31c9f4078b3      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk 0d87ba78-59ed-42f4-af6e-f31c9f4078b3}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.772Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:0d87ba78-59ed-42f4-af6e-f31c9f4078b3 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk VolumeID:0d87ba78-59ed-42f4-af6e-f31c9f4078b3}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.787Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:22:48.787Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk", volumeID: "0d87ba78-59ed-42f4-af6e-f31c9f4078b3" mapping in the cache
2020-07-17T06:22:48.787Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:0d87ba78-59ed-42f4-af6e-f31c9f4078b3 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/0d87ba78-59ed-42f4-af6e-f31c9f4078b3 UID:0a0e701e-d26e-46ab-bd3e-4e3779a03337 ResourceVersion:7159461 Generation:1 CreationTimestamp:2020-07-17 06:22:48 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:22:48 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk VolumeID:0d87ba78-59ed-42f4-af6e-f31c9f4078b3}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.787Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {0d87ba78-59ed-42f4-af6e-f31c9f4078b3   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/0d87ba78-59ed-42f4-af6e-f31c9f4078b3 0a0e701e-d26e-46ab-bd3e-4e3779a03337 7159461 1 2020-07-17 06:22:48 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:22:48 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-91faf067-d5a4-4db5-82a1-38dd9e1a2f8a.vmdk 0d87ba78-59ed-42f4-af6e-f31c9f4078b3}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.787Z	DEBUG	syncer/util.go:46	FullSync: pv 0d87ba78-59ed-42f4-af6e-f31c9f4078b3 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.788Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.788Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.788Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004687e0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "ef893b50-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc001180750)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:22:48.807Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.175Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "ef893b50-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d27e"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.176Z	DEBUG	volume/manager.go:244	Deleted task for ef893b50-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.177Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "ef893b50-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d27e", volumeID: "4f42f6b2-15cd-42e2-813b-7582c1810c0a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.177Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk" as container volume with ID: "4f42f6b2-15cd-42e2-813b-7582c1810c0a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.180Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk" with CNS. VolumeID: "4f42f6b2-15cd-42e2-813b-7582c1810c0a"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.180Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {4f42f6b2-15cd-42e2-813b-7582c1810c0a      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk 4f42f6b2-15cd-42e2-813b-7582c1810c0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.180Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4f42f6b2-15cd-42e2-813b-7582c1810c0a GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk VolumeID:4f42f6b2-15cd-42e2-813b-7582c1810c0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.201Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:23:00.201Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk", volumeID: "4f42f6b2-15cd-42e2-813b-7582c1810c0a" mapping in the cache
2020-07-17T06:23:00.201Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:4f42f6b2-15cd-42e2-813b-7582c1810c0a GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4f42f6b2-15cd-42e2-813b-7582c1810c0a UID:01ada7b5-2d26-41f4-aca7-41bcfb4068bc ResourceVersion:7159498 Generation:1 CreationTimestamp:2020-07-17 06:23:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:23:00 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk VolumeID:4f42f6b2-15cd-42e2-813b-7582c1810c0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.203Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {4f42f6b2-15cd-42e2-813b-7582c1810c0a   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/4f42f6b2-15cd-42e2-813b-7582c1810c0a 01ada7b5-2d26-41f4-aca7-41bcfb4068bc 7159498 1 2020-07-17 06:23:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:23:00 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-11901778-8131-4b4e-9ea9-d05065c69743.vmdk 4f42f6b2-15cd-42e2-813b-7582c1810c0a}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.203Z	DEBUG	syncer/util.go:46	FullSync: pv 4f42f6b2-15cd-42e2-813b-7582c1810c0a is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.203Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.203Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.204Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc0004681c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "f6571527-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00117d170)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:00.221Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.620Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "f6571527-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d280"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.620Z	DEBUG	volume/manager.go:244	Deleted task for f6571527-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.620Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "f6571527-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d280", volumeID: "39455e77-471d-4c5d-b8d4-28075ba9c843"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.620Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk" as container volume with ID: "39455e77-471d-4c5d-b8d4-28075ba9c843"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.620Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk" with CNS. VolumeID: "39455e77-471d-4c5d-b8d4-28075ba9c843"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.620Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {39455e77-471d-4c5d-b8d4-28075ba9c843      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk 39455e77-471d-4c5d-b8d4-28075ba9c843}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.620Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:39455e77-471d-4c5d-b8d4-28075ba9c843 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk VolumeID:39455e77-471d-4c5d-b8d4-28075ba9c843}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.654Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:23:15.654Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk", volumeID: "39455e77-471d-4c5d-b8d4-28075ba9c843" mapping in the cache
2020-07-17T06:23:15.684Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:39455e77-471d-4c5d-b8d4-28075ba9c843 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/39455e77-471d-4c5d-b8d4-28075ba9c843 UID:76fb807d-da96-4d8b-82b9-6d8e0757e556 ResourceVersion:7159547 Generation:1 CreationTimestamp:2020-07-17 06:23:15 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:23:15 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk VolumeID:39455e77-471d-4c5d-b8d4-28075ba9c843}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.690Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {39455e77-471d-4c5d-b8d4-28075ba9c843   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/39455e77-471d-4c5d-b8d4-28075ba9c843 76fb807d-da96-4d8b-82b9-6d8e0757e556 7159547 1 2020-07-17 06:23:15 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:23:15 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c98bc487-820c-4307-b978-a89843c606a3.vmdk 39455e77-471d-4c5d-b8d4-28075ba9c843}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.690Z	DEBUG	syncer/util.go:46	FullSync: pv 39455e77-471d-4c5d-b8d4-28075ba9c843 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.690Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.690Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.691Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468380)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "ff92328d-c7f5-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0009343f0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:15.715Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.841Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "ff92328d-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d283"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.841Z	DEBUG	volume/manager.go:244	Deleted task for ff92328d-c7f5-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.841Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "ff92328d-c7f5-11ea-8f7c-9695ff1c5b6f", opId: "9168d283", volumeID: "00869e00-39fb-493a-bf7c-2432eff9aa0f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.841Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk" as container volume with ID: "00869e00-39fb-493a-bf7c-2432eff9aa0f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.841Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk" with CNS. VolumeID: "00869e00-39fb-493a-bf7c-2432eff9aa0f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.841Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {00869e00-39fb-493a-bf7c-2432eff9aa0f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk 00869e00-39fb-493a-bf7c-2432eff9aa0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.841Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:00869e00-39fb-493a-bf7c-2432eff9aa0f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk VolumeID:00869e00-39fb-493a-bf7c-2432eff9aa0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.851Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:23:30.851Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk", volumeID: "00869e00-39fb-493a-bf7c-2432eff9aa0f" mapping in the cache
2020-07-17T06:23:30.855Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:00869e00-39fb-493a-bf7c-2432eff9aa0f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/00869e00-39fb-493a-bf7c-2432eff9aa0f UID:8f1af512-3f22-4d48-b40d-6ab8399c13df ResourceVersion:7159595 Generation:1 CreationTimestamp:2020-07-17 06:23:30 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:23:30 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk VolumeID:00869e00-39fb-493a-bf7c-2432eff9aa0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.857Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {00869e00-39fb-493a-bf7c-2432eff9aa0f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/00869e00-39fb-493a-bf7c-2432eff9aa0f 8f1af512-3f22-4d48-b40d-6ab8399c13df 7159595 1 2020-07-17 06:23:30 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:23:30 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-b4081390-0914-4924-9512-b425142cc166.vmdk 00869e00-39fb-493a-bf7c-2432eff9aa0f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.858Z	DEBUG	syncer/util.go:46	FullSync: pv 00869e00-39fb-493a-bf7c-2432eff9aa0f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.858Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.858Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.858Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468540)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "089c9972-c7f6-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0009899b0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:30.873Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.151Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "089c9972-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d286"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.151Z	DEBUG	volume/manager.go:244	Deleted task for 089c9972-c7f6-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.151Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "089c9972-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d286", volumeID: "8821badd-2834-44e9-ac9f-1328d116193f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.151Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk" as container volume with ID: "8821badd-2834-44e9-ac9f-1328d116193f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.151Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk" with CNS. VolumeID: "8821badd-2834-44e9-ac9f-1328d116193f"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.151Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {8821badd-2834-44e9-ac9f-1328d116193f      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk 8821badd-2834-44e9-ac9f-1328d116193f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.151Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:8821badd-2834-44e9-ac9f-1328d116193f GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk VolumeID:8821badd-2834-44e9-ac9f-1328d116193f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.161Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:23:50.162Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk", volumeID: "8821badd-2834-44e9-ac9f-1328d116193f" mapping in the cache
2020-07-17T06:23:50.164Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:8821badd-2834-44e9-ac9f-1328d116193f GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/8821badd-2834-44e9-ac9f-1328d116193f UID:26aa97a3-707c-466f-b876-33f659c8f9cf ResourceVersion:7159656 Generation:1 CreationTimestamp:2020-07-17 06:23:50 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:23:50 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk VolumeID:8821badd-2834-44e9-ac9f-1328d116193f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.165Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {8821badd-2834-44e9-ac9f-1328d116193f   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/8821badd-2834-44e9-ac9f-1328d116193f 26aa97a3-707c-466f-b876-33f659c8f9cf 7159656 1 2020-07-17 06:23:50 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:23:50 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-a4d3fe27-6625-4440-9c4e-e514a79c2c03.vmdk 8821badd-2834-44e9-ac9f-1328d116193f}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.165Z	DEBUG	syncer/util.go:46	FullSync: pv 8821badd-2834-44e9-ac9f-1328d116193f is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.165Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.165Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.165Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468620)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "141eabd7-c7f6-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc000dbf4a0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:23:50.173Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.125Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "141eabd7-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2bc"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.126Z	DEBUG	volume/manager.go:244	Deleted task for 141eabd7-c7f6-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.126Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "141eabd7-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2bc", volumeID: "2e804728-f050-4c2f-9f44-fa2c0c593704"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.127Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk" as container volume with ID: "2e804728-f050-4c2f-9f44-fa2c0c593704"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.130Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk" with CNS. VolumeID: "2e804728-f050-4c2f-9f44-fa2c0c593704"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.130Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {2e804728-f050-4c2f-9f44-fa2c0c593704      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk 2e804728-f050-4c2f-9f44-fa2c0c593704}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.130Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2e804728-f050-4c2f-9f44-fa2c0c593704 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk VolumeID:2e804728-f050-4c2f-9f44-fa2c0c593704}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.141Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:24:03.142Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk", volumeID: "2e804728-f050-4c2f-9f44-fa2c0c593704" mapping in the cache
2020-07-17T06:24:03.142Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:2e804728-f050-4c2f-9f44-fa2c0c593704 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2e804728-f050-4c2f-9f44-fa2c0c593704 UID:35cba646-4cdf-4a04-b389-4add320530cf ResourceVersion:7159696 Generation:1 CreationTimestamp:2020-07-17 06:24:03 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:24:03 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk VolumeID:2e804728-f050-4c2f-9f44-fa2c0c593704}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.142Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {2e804728-f050-4c2f-9f44-fa2c0c593704   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/2e804728-f050-4c2f-9f44-fa2c0c593704 35cba646-4cdf-4a04-b389-4add320530cf 7159696 1 2020-07-17 06:24:03 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:24:03 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-9d090dcd-c275-411c-afca-6708c205bd2f.vmdk 2e804728-f050-4c2f-9f44-fa2c0c593704}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.143Z	DEBUG	syncer/util.go:46	FullSync: pv 2e804728-f050-4c2f-9f44-fa2c0c593704 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.143Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.144Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.144Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468700)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "1bdb025d-c7f6-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0004b06f0)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:03.162Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.109Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "1bdb025d-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2c8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.109Z	DEBUG	volume/manager.go:244	Deleted task for 1bdb025d-c7f6-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.110Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "1bdb025d-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2c8", volumeID: "c6c9f547-b429-4bda-8f11-8484b2638cd7"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.110Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk" as container volume with ID: "c6c9f547-b429-4bda-8f11-8484b2638cd7"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.110Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk" with CNS. VolumeID: "c6c9f547-b429-4bda-8f11-8484b2638cd7"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.110Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {c6c9f547-b429-4bda-8f11-8484b2638cd7      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk c6c9f547-b429-4bda-8f11-8484b2638cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.110Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c6c9f547-b429-4bda-8f11-8484b2638cd7 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk VolumeID:c6c9f547-b429-4bda-8f11-8484b2638cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.121Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c6c9f547-b429-4bda-8f11-8484b2638cd7 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c6c9f547-b429-4bda-8f11-8484b2638cd7 UID:35ac62ea-b057-482d-8f3d-d70fba2983ad ResourceVersion:7159737 Generation:1 CreationTimestamp:2020-07-17 06:24:16 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:24:16 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk VolumeID:c6c9f547-b429-4bda-8f11-8484b2638cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.121Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {c6c9f547-b429-4bda-8f11-8484b2638cd7   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c6c9f547-b429-4bda-8f11-8484b2638cd7 35ac62ea-b057-482d-8f3d-d70fba2983ad 7159737 1 2020-07-17 06:24:16 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:24:16 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk c6c9f547-b429-4bda-8f11-8484b2638cd7}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.121Z	DEBUG	syncer/util.go:46	FullSync: pv c6c9f547-b429-4bda-8f11-8484b2638cd7 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.121Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.121Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.121Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000a521c0)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "23972dd7-c7f6-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0002e3350)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:16.122Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:24:16.122Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-2ca1e549-4b69-49bd-ab23-b5ca806bff70.vmdk", volumeID: "c6c9f547-b429-4bda-8f11-8484b2638cd7" mapping in the cache
2020-07-17T06:24:16.131Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.135Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "23972dd7-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2ca"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.135Z	DEBUG	volume/manager.go:244	Deleted task for 23972dd7-c7f6-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.135Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "23972dd7-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2ca", volumeID: "fd539680-1ba0-4ad8-a3bc-7de7a84a3873"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.135Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk" as container volume with ID: "fd539680-1ba0-4ad8-a3bc-7de7a84a3873"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.135Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk" with CNS. VolumeID: "fd539680-1ba0-4ad8-a3bc-7de7a84a3873"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.135Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {fd539680-1ba0-4ad8-a3bc-7de7a84a3873      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk fd539680-1ba0-4ad8-a3bc-7de7a84a3873}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.136Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fd539680-1ba0-4ad8-a3bc-7de7a84a3873 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk VolumeID:fd539680-1ba0-4ad8-a3bc-7de7a84a3873}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.142Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:fd539680-1ba0-4ad8-a3bc-7de7a84a3873 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fd539680-1ba0-4ad8-a3bc-7de7a84a3873 UID:bb9af44e-4723-4bb3-8519-f1bd09feb58b ResourceVersion:7159797 Generation:1 CreationTimestamp:2020-07-17 06:24:35 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:24:35 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk VolumeID:fd539680-1ba0-4ad8-a3bc-7de7a84a3873}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.144Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:24:35.145Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk", volumeID: "fd539680-1ba0-4ad8-a3bc-7de7a84a3873" mapping in the cache
2020-07-17T06:24:35.145Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {fd539680-1ba0-4ad8-a3bc-7de7a84a3873   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/fd539680-1ba0-4ad8-a3bc-7de7a84a3873 bb9af44e-4723-4bb3-8519-f1bd09feb58b 7159797 1 2020-07-17 06:24:35 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:24:35 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-0244069d-1d75-4c05-8d52-60345973640e.vmdk fd539680-1ba0-4ad8-a3bc-7de7a84a3873}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.145Z	DEBUG	syncer/util.go:46	FullSync: pv fd539680-1ba0-4ad8-a3bc-7de7a84a3873 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.145Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.146Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.147Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468b60)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "2eee2e93-c7f6-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc00051d890)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:35.157Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.022Z	INFO	volume/manager.go:201	CreateVolume: VolumeName: "2eee2e93-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2f8"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.022Z	DEBUG	volume/manager.go:244	Deleted task for 2eee2e93-c7f6-11ea-8f7c-9695ff1c5b6f from volumeTaskMap for statically provisioned volume	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.022Z	INFO	volume/manager.go:249	CreateVolume: Volume created successfully. VolumeName: "2eee2e93-c7f6-11ea-8f7c-9695ff1c5b6f", opId: "9168d2f8", volumeID: "c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.022Z	INFO	migration/migration.go:378	successfully registered volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk" as container volume with ID: "c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.022Z	INFO	migration/migration.go:174	successfully registered volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk" with CNS. VolumeID: "c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.023Z	DEBUG	migration/migration.go:182	Saving cnsvSphereVolumeMigration CR: {{ } {c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718      0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] nil []  []} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.023Z	INFO	migration/migration.go:253	creating CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718 GenerateName: Namespace: SelfLink: UID: ResourceVersion: Generation:0 CreationTimestamp:0001-01-01 00:00:00 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk VolumeID:c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.035Z	DEBUG	migration/migration.go:131	received add event for VolumeMigration CR!
2020-07-17T06:24:47.035Z	DEBUG	migration/migration.go:135	successfully added volumePath: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk", volumeID: "c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718" mapping in the cache
2020-07-17T06:24:47.036Z	INFO	migration/migration.go:263	successfully created CR for cnsVSphereVolumeMigration: &{TypeMeta:{Kind: APIVersion:} ObjectMeta:{Name:c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718 GenerateName: Namespace: SelfLink:/apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718 UID:919d0be7-0be6-4d0a-bb10-7050dfcca428 ResourceVersion:7159837 Generation:1 CreationTimestamp:2020-07-17 06:24:47 +0000 UTC DeletionTimestamp:<nil> DeletionGracePeriodSeconds:<nil> Labels:map[] Annotations:map[] OwnerReferences:[] Initializers:nil Finalizers:[] ClusterName: ManagedFields:[{Manager:vsphere-syncer Operation:Update APIVersion:cns.vmware.com/v1alpha1 Time:2020-07-17 06:24:47 +0000 UTC Fields:nil}]} Spec:{VolumePath:[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk VolumeID:c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.036Z	INFO	migration/migration.go:188	successfully saved cnsvSphereVolumeMigration CR: {{ } {c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718   /apis/cns.vmware.com/v1alpha1/cnsvspherevolumemigrations/c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718 919d0be7-0be6-4d0a-bb10-7050dfcca428 7159837 1 2020-07-17 06:24:47 +0000 UTC <nil> <nil> map[] map[] [] nil []  [{vsphere-syncer Update cns.vmware.com/v1alpha1 2020-07-17 06:24:47 +0000 UTC nil}]} {[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-c4a3fd4f-32c8-4c88-a11d-1387fe11ad47.vmdk c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718}}	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.036Z	DEBUG	syncer/util.go:46	FullSync: pv c1b8a9bb-bc8b-46a6-a1ff-6db5ba6cc718 is in state Bound	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.036Z	INFO	migration/migration.go:168	Could not retrieve VolumeID from cache for Volume Path: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-28ae305d-45ae-4ed3-a4c8-5e92c1439f40.vmdk". volume may not be registered. Registering Volume with CNS	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.036Z	INFO	migration/migration.go:354	Registering volume: "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-28ae305d-45ae-4ed3-a4c8-5e92c1439f40.vmdk" using backingDiskURLPath :"https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-28ae305d-45ae-4ed3-a4c8-5e92c1439f40.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.037Z	DEBUG	migration/migration.go:369	vSphere CNS driver registering volume "[vsanDatastore] 9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-28ae305d-45ae-4ed3-a4c8-5e92c1439f40.vmdk" with create spec (*types.CnsVolumeCreateSpec)(0xc000468d20)({
 DynamicData: (types.DynamicData) {
 },
 Name: (string) (len=36) "3604729f-c7f6-11ea-8f7c-9695ff1c5b6f",
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
 BackingObjectDetails: (*types.CnsBlockBackingDetails)(0xc0005dca80)({
  CnsBackingObjectDetails: (types.CnsBackingObjectDetails) {
   DynamicData: (types.DynamicData) {
   },
   CapacityInMb: (int64) 0
  },
  BackingDiskId: (string) "",
  BackingDiskUrlPath: (string) (len=194) "https://10.161.154.134/folder/9c01ed5e-fa70-24c1-b120-0200466f7ac2/kubernetes-dynamic-pvc-28ae305d-45ae-4ed3-a4c8-5e92c1439f40.vmdk?dcPath=test-vpx-1592251113-13014-hostpool&dsName=vsanDatastore"
 }),
 Profile: ([]types.BaseVirtualMachineProfileSpec) <nil>,
 CreateSpec: (types.BaseCnsBaseCreateSpec) <nil>
})
	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T06:24:47.045Z	DEBUG	volume/manager.go:167	Update VSphereUser from Administrator@vsphere.local to VSPHERE.LOCAL\Administrator	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}


</pre>


* Number of volumes registered in first 30 minutes: ~128
<pre>
root@kube-master:~# kubectl get cnsvspherevolumemigrations.cns.vmware.com -A | grep - | wc -l
128
</pre>

* Observed that full sync is not terminated based on time interval(30 minutes). Full sync continues with processing the data in the first cycle
