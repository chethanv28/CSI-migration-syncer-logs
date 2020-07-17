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
....
....
....


</pre>


* Number of volumes registered in first 30 minutes: ~128
<pre>
root@kube-master:~# kubectl get cnsvspherevolumemigrations.cns.vmware.com -A | grep - | wc -l
128
</pre>

* Observed that full sync is not terminated based on time interval(30 minutes). Full sync continues with processing the data in the first cycle
<pre>
root@kube-master:~# kubectl logs vsphere-csi-controller-7cc4b4689d-rl2gk -c vsphere-syncer -n kube-system | grep "triggered"
2020-07-17T05:54:49.011Z	INFO	syncer/metadatasyncer.go:269	fullSync is triggered	{"TraceId": "502f2638-77a6-4644-b5f6-793a13c4a084"}
2020-07-17T08:49:45.858Z	INFO	syncer/metadatasyncer.go:269	fullSync is triggered	{"TraceId": "fb94b202-888d-41d0-867e-8875cb0c227f"}
</pre>

* 1 full sync cycle took 2hrs 55 minutes
<pre>
  Time spent registering 500 volumes = 2 hours (15 seconds for each volume)
  Time spent for volume metadata update = 50 minutes (6 seconds per volume)
</pre>
  
CNS UI after full sync: [CNS UI](https://github.com/chethanv28/CSI-migration-syncer-logs/blob/master/Full-sync-large.png)
*Note: 6 volumes existed before this test. So ignore the extra volumes on the UI. They were present before the beginning of this test
