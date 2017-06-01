[student@workstation ~]$ ssh stack@director
Last login: Thu Jun  1 03:16:07 2017 from workstation.lab.example.com
[stack@director ~]$ sudo yum install -y rhosp-director-images rhosp-director-images-ipa-8.0-20160415.1.el7ost.noarch 
Loaded plugins: langpacks, search-disabled-repos
Resolving Dependencies
--> Running transaction check
---> Package rhosp-director-images.noarch 0:8.0-20160415.1.el7ost will be installed
---> Package rhosp-director-images-ipa.noarch 0:8.0-20160415.1.el7ost will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================================================================
 Package                                                      Arch                                      Version                                                  Repository                                   Size
===================================================================================================================================================================================================================
Installing:
 rhosp-director-images                                        noarch                                    8.0-20160415.1.el7ost                                    director                                    1.0 G
 rhosp-director-images-ipa                                    noarch                                    8.0-20160415.1.el7ost                                    director                                    330 M

Transaction Summary
===================================================================================================================================================================================================================
Install  2 Packages

Total download size: 1.3 G
Installed size: 1.3 G
Downloading packages:
(1/2): rhosp-director-images-ipa-8.0-20160415.1.el7ost.noarch.rpm                                                                                                                           | 330 MB  00:00:09     
(2/2): rhosp-director-images-8.0-20160415.1.el7ost.noarch.rpm                                                                                                                               | 1.0 GB  00:00:23     
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                               56 MB/s | 1.3 GB  00:00:23     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : rhosp-director-images-8.0-20160415.1.el7ost.noarch                                                                                                                                              1/2 
  Installing : rhosp-director-images-ipa-8.0-20160415.1.el7ost.noarch                                                                                                                                          2/2 
  Verifying  : rhosp-director-images-ipa-8.0-20160415.1.el7ost.noarch                                                                                                                                          1/2 
  Verifying  : rhosp-director-images-8.0-20160415.1.el7ost.noarch                                                                                                                                              2/2 

Installed:
  rhosp-director-images.noarch 0:8.0-20160415.1.el7ost                                                   rhosp-director-images-ipa.noarch 0:8.0-20160415.1.el7ost                                                  

Complete!
[stack@director ~]$ mkdir images
[stack@director ~]$ cp /usr/share/rhosp-director-images/overcloud-full-latest-8.0.tar images/
[stack@director ~]$ cp /usr/share/rhosp-director-images/ironic-python-agent-latest-8.0.tar images/
[stack@director ~]$ ls
images  stackrc  tempest-deployer-input.conf  undercloud.conf  undercloud-passwords.conf
[stack@director ~]$ cd images/
[stack@director images]$ ls
ironic-python-agent-latest-8.0.tar  overcloud-full-latest-8.0.tar
[stack@director images]$ tar xf ironic-python-agent-latest-8.0.tar 
[stack@director images]$ tar xf overcloud-full-latest-8.0.tar 
[stack@director images]$ source ../stackrc 
[stack@director images]$ openstack overcloud image upload --image-path ~/images
Image "overcloud-full-vmlinuz" was uploaded.
+--------------------------------------+------------------------+-------------+---------+--------+
|                  ID                  |          Name          | Disk Format |   Size  | Status |
+--------------------------------------+------------------------+-------------+---------+--------+
| 7085e180-5a6e-4039-afb0-6eab60084c23 | overcloud-full-vmlinuz |     aki     | 5153408 | active |
+--------------------------------------+------------------------+-------------+---------+--------+
Image "overcloud-full-initrd" was uploaded.
+--------------------------------------+-----------------------+-------------+----------+--------+
|                  ID                  |          Name         | Disk Format |   Size   | Status |
+--------------------------------------+-----------------------+-------------+----------+--------+
| 539e8ffb-4f2c-4052-808e-eff91e75fc96 | overcloud-full-initrd |     ari     | 40324447 | active |
+--------------------------------------+-----------------------+-------------+----------+--------+
Image "overcloud-full" was uploaded.
+--------------------------------------+----------------+-------------+------------+--------+
|                  ID                  |      Name      | Disk Format |    Size    | Status |
+--------------------------------------+----------------+-------------+------------+--------+
| 950efd70-1f02-41d2-a335-678bb863699d | overcloud-full |    qcow2    | 1028305920 | active |
+--------------------------------------+----------------+-------------+------------+--------+
Image "bm-deploy-kernel" was uploaded.
+--------------------------------------+------------------+-------------+---------+--------+
|                  ID                  |       Name       | Disk Format |   Size  | Status |
+--------------------------------------+------------------+-------------+---------+--------+
| 9944fe63-3d76-4717-b912-a9c1fef9a8ac | bm-deploy-kernel |     aki     | 5153408 | active |
+--------------------------------------+------------------+-------------+---------+--------+
Image "bm-deploy-ramdisk" was uploaded.
+--------------------------------------+-------------------+-------------+-----------+--------+
|                  ID                  |        Name       | Disk Format |    Size   | Status |
+--------------------------------------+-------------------+-------------+-----------+--------+
| 3ca3660e-d502-4cb4-9029-67a939b3d439 | bm-deploy-ramdisk |     ari     | 344421623 | active |
+--------------------------------------+-------------------+-------------+-----------+--------+
[stack@director images]$ openstack image list
+--------------------------------------+------------------------+
| ID                                   | Name                   |
+--------------------------------------+------------------------+
| 3ca3660e-d502-4cb4-9029-67a939b3d439 | bm-deploy-ramdisk      |
| 9944fe63-3d76-4717-b912-a9c1fef9a8ac | bm-deploy-kernel       |
| 950efd70-1f02-41d2-a335-678bb863699d | overcloud-full         |
| 7085e180-5a6e-4039-afb0-6eab60084c23 | overcloud-full-vmlinuz |
| 539e8ffb-4f2c-4052-808e-eff91e75fc96 | overcloud-full-initrd  |
+--------------------------------------+------------------------+
[stack@director images]$ cd
[stack@director ~]$ neutron subnet-list
+--------------------------------------+------+-----------------+----------------------------------------------------+
| id                                   | name | cidr            | allocation_pools                                   |
+--------------------------------------+------+-----------------+----------------------------------------------------+
| 1f5b174a-9066-41d8-9ade-0d70478e4124 |      | 172.25.250.0/24 | {"start": "172.25.250.20", "end": "172.25.250.30"} |
+--------------------------------------+------+-----------------+----------------------------------------------------+
[stack@director ~]$ neutron subnet-update 1f5b174a-9066-41d8-9ade-0d70478e4124 --dns-namserver 172.25.250.254
Unrecognized attribute(s) 'dns_namserver'
[stack@director ~]$ neutron subnet-update 1f5b174a-9066-41d8-9ade-0d70478e4124 --dns-nameserver 172.25.250.254
Updated subnet: 1f5b174a-9066-41d8-9ade-0d70478e4124
[stack@director ~]$ wget http://materials.example.com/instackenv-twonodes.json
--2017-06-01 04:29:03--  http://materials.example.com/instackenv-twonodes.json
Resolving materials.example.com (materials.example.com)... 172.25.254.254
Connecting to materials.example.com (materials.example.com)|172.25.254.254|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 626 [application/json]
Saving to: ‘instackenv-twonodes.json’

100%[=========================================================================================================================================================================>] 626         --.-K/s   in 0s      

2017-06-01 04:29:03 (150 MB/s) - ‘instackenv-twonodes.json’ saved [626/626]

[stack@director ~]$ cat instackenv-twonodes.json 
{
  "nodes": [
    {
      "pm_user": "admin",
      "arch": "x86_64",
      "name": "controller",
      "pm_addr": "192.168.1.111",
      "pm_password": "password",
      "pm_type": "pxe_ipmitool",
      "mac": [
        "52:54:00:00:fa:0b"
      ],
      "cpu": "2",
      "memory": "6144",
      "disk": "40"
    },
    {
      "pm_user": "admin",
      "arch": "x86_64",
      "name": "compute1",
      "pm_addr": "192.168.1.112",
      "pm_type": "pxe_ipmitool",
      "pm_password": "password",
      "mac": [
        "52:54:00:00:fa:0c"
      ],
      "cpu": "2",
      "memory": "6144",
      "disk": "40"
    }
  ]
}
[stack@director ~]$ openstack baremetal import --json instackenv-twonodes.json 
[stack@director ~]$ openstack baremetal configure boot
[stack@director ~]$ openstack image list
+--------------------------------------+------------------------+
| ID                                   | Name                   |
+--------------------------------------+------------------------+
| 3ca3660e-d502-4cb4-9029-67a939b3d439 | bm-deploy-ramdisk      |
| 9944fe63-3d76-4717-b912-a9c1fef9a8ac | bm-deploy-kernel       |
| 950efd70-1f02-41d2-a335-678bb863699d | overcloud-full         |
| 7085e180-5a6e-4039-afb0-6eab60084c23 | overcloud-full-vmlinuz |
| 539e8ffb-4f2c-4052-808e-eff91e75fc96 | overcloud-full-initrd  |
+--------------------------------------+------------------------+
[stack@director ~]$ for i in $(ironic node-list | grep -v UUID | awk '{print $2}')
> do
> ironic node-show $i | grep deploy
> echo
> done
|                        | u'ipmi_username': u'admin', u'deploy_kernel':                       |
|                        | u'9944fe63-3d76-4717-b912-a9c1fef9a8ac', u'deploy_ramdisk':         |

|                        | u'ipmi_username': u'admin', u'deploy_kernel':                       |
|                        | u'9944fe63-3d76-4717-b912-a9c1fef9a8ac', u'deploy_ramdisk':         |

[stack@director ~]$ ironic node-list
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
| UUID                                 | Name       | Instance UUID | Power State | Provisioning State | Maintenance |
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
| 822d3310-8fb5-4e1d-a218-5c5c6d4f4489 | controller | None          | power off   | available          | False       |
| 11cc41d0-1efd-41e6-84cf-034cbecb587d | compute1   | None          | power off   | available          | False       |
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
[stack@director ~]$ openstack baremetal introspection bulk start
Setting nodes for introspection to manageable...
Starting introspection of node: 822d3310-8fb5-4e1d-a218-5c5c6d4f4489
Starting introspection of node: 11cc41d0-1efd-41e6-84cf-034cbecb587d
Waiting for introspection to finish...
Introspection for UUID 822d3310-8fb5-4e1d-a218-5c5c6d4f4489 finished successfully.
Introspection for UUID 11cc41d0-1efd-41e6-84cf-034cbecb587d finished successfully.
Setting manageable nodes to available...
Node 822d3310-8fb5-4e1d-a218-5c5c6d4f4489 has been set to available.
Node 11cc41d0-1efd-41e6-84cf-034cbecb587d has been set to available.
Introspection completed.
[stack@director ~]$ openstack flavor create --id auto --ram 2048 --disk 10 --vcpus 2 baremetal
+----------------------------+--------------------------------------+
| Field                      | Value                                |
+----------------------------+--------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                |
| OS-FLV-EXT-DATA:ephemeral  | 0                                    |
| disk                       | 10                                   |
| id                         | 7ad16a8b-00f0-4dd3-9a00-e0a551c2b5a6 |
| name                       | baremetal                            |
| os-flavor-access:is_public | True                                 |
| ram                        | 2048                                 |
| rxtx_factor                | 1.0                                  |
| swap                       |                                      |
| vcpus                      | 2                                    |
+----------------------------+--------------------------------------+
[stack@director ~]$ openstack flavor create --id auto --ram 6144 --disk 20 --vcpus 2 compute
+----------------------------+--------------------------------------+
| Field                      | Value                                |
+----------------------------+--------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                |
| OS-FLV-EXT-DATA:ephemeral  | 0                                    |
| disk                       | 20                                   |
| id                         | 9f25ff54-05a8-4c7e-957a-2a000f9d8790 |
| name                       | compute                              |
| os-flavor-access:is_public | True                                 |
| ram                        | 6144                                 |
| rxtx_factor                | 1.0                                  |
| swap                       |                                      |
| vcpus                      | 2                                    |
+----------------------------+--------------------------------------+
[stack@director ~]$ openstack flavor create --id auto --ram 6144 --disk 30 --vcpus 2 control
+----------------------------+--------------------------------------+
| Field                      | Value                                |
+----------------------------+--------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                |
| OS-FLV-EXT-DATA:ephemeral  | 0                                    |
| disk                       | 30                                   |
| id                         | 8641a232-891d-464f-a61c-06764b0733e9 |
| name                       | control                              |
| os-flavor-access:is_public | True                                 |
| ram                        | 6144                                 |
| rxtx_factor                | 1.0                                  |
| swap                       |                                      |
| vcpus                      | 2                                    |
+----------------------------+--------------------------------------+
[stack@director ~]$ openstack flavor set --property "cpu_arch"="x86_64" --property "capabilities:boot_option"="local" baremetal
+----------------------------+-----------------------------------------------------+
| Field                      | Value                                               |
+----------------------------+-----------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                               |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                   |
| disk                       | 10                                                  |
| id                         | 7ad16a8b-00f0-4dd3-9a00-e0a551c2b5a6                |
| name                       | baremetal                                           |
| os-flavor-access:is_public | True                                                |
| properties                 | capabilities:boot_option='local', cpu_arch='x86_64' |
| ram                        | 2048                                                |
| rxtx_factor                | 1.0                                                 |
| swap                       |                                                     |
| vcpus                      | 2                                                   |
+----------------------------+-----------------------------------------------------+
[stack@director ~]$ openstack flavor set --property "cpu_arch"="x86_64" --property "capabilities:profile"="compute" --property "capabilities:boot_option"="local" compute
+----------------------------+-------------------------------------------------------------------------------------+
| Field                      | Value                                                                               |
+----------------------------+-------------------------------------------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                                                               |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                                                   |
| disk                       | 20                                                                                  |
| id                         | 9f25ff54-05a8-4c7e-957a-2a000f9d8790                                                |
| name                       | compute                                                                             |
| os-flavor-access:is_public | True                                                                                |
| properties                 | capabilities:boot_option='local', capabilities:profile='compute', cpu_arch='x86_64' |
| ram                        | 6144                                                                                |
| rxtx_factor                | 1.0                                                                                 |
| swap                       |                                                                                     |
| vcpus                      | 2                                                                                   |
+----------------------------+-------------------------------------------------------------------------------------+
[stack@director ~]$ openstack flavor set --property "cpu_arch"="x86_64" --property "capabilities:profile"="control" --property "capabilities:boot_option"="local" control
+----------------------------+-------------------------------------------------------------------------------------+
| Field                      | Value                                                                               |
+----------------------------+-------------------------------------------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                                                               |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                                                   |
| disk                       | 30                                                                                  |
| id                         | 8641a232-891d-464f-a61c-06764b0733e9                                                |
| name                       | control                                                                             |
| os-flavor-access:is_public | True                                                                                |
| properties                 | capabilities:boot_option='local', capabilities:profile='control', cpu_arch='x86_64' |
| ram                        | 6144                                                                                |
| rxtx_factor                | 1.0                                                                                 |
| swap                       |                                                                                     |
| vcpus                      | 2                                                                                   |
+----------------------------+-------------------------------------------------------------------------------------+
[stack@director ~]$ ironic node-list
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
| UUID                                 | Name       | Instance UUID | Power State | Provisioning State | Maintenance |
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
| 822d3310-8fb5-4e1d-a218-5c5c6d4f4489 | controller | None          | power off   | available          | False       |
| 11cc41d0-1efd-41e6-84cf-034cbecb587d | compute1   | None          | power off   | available          | False       |
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
[stack@director ~]$ ironic node-update 822d3310-8fb5-4e1d-a218-5c5c6d4f4489 add properties/capabilities='profile:control,boot_option:local'
+------------------------+-----------------------------------------------------------------------+
| Property               | Value                                                                 |
+------------------------+-----------------------------------------------------------------------+
| target_power_state     | None                                                                  |
| extra                  | {u'hardware_swift_object': u'extra_hardware-                          |
|                        | 822d3310-8fb5-4e1d-a218-5c5c6d4f4489'}                                |
| last_error             | None                                                                  |
| updated_at             | 2017-06-01T08:33:02+00:00                                             |
| maintenance_reason     | None                                                                  |
| provision_state        | available                                                             |
| clean_step             | {}                                                                    |
| uuid                   | 822d3310-8fb5-4e1d-a218-5c5c6d4f4489                                  |
| console_enabled        | False                                                                 |
| target_provision_state | None                                                                  |
| provision_updated_at   | 2017-06-01T08:33:02+00:00                                             |
| maintenance            | False                                                                 |
| inspection_started_at  | None                                                                  |
| inspection_finished_at | None                                                                  |
| power_state            | power off                                                             |
| driver                 | pxe_ipmitool                                                          |
| reservation            | None                                                                  |
| properties             | {u'memory_mb': u'6144', u'cpu_arch': u'x86_64', u'local_gb': u'39',   |
|                        | u'cpus': u'2', u'capabilities': u'profile:control,boot_option:local'} |
| instance_uuid          | None                                                                  |
| name                   | controller                                                            |
| driver_info            | {u'ipmi_password': u'******', u'ipmi_address': u'192.168.1.111',      |
|                        | u'ipmi_username': u'admin', u'deploy_kernel':                         |
|                        | u'9944fe63-3d76-4717-b912-a9c1fef9a8ac', u'deploy_ramdisk':           |
|                        | u'3ca3660e-d502-4cb4-9029-67a939b3d439'}                              |
| created_at             | 2017-06-01T08:29:42+00:00                                             |
| driver_internal_info   | {}                                                                    |
| chassis_uuid           |                                                                       |
| instance_info          | {}                                                                    |
+------------------------+-----------------------------------------------------------------------+
[stack@director ~]$ ironic node-update 11cc41d0-1efd-41e6-84cf-034cbecb587d add properties/capabilities='profile:compute,boot_option:local'
+------------------------+-----------------------------------------------------------------------+
| Property               | Value                                                                 |
+------------------------+-----------------------------------------------------------------------+
| target_power_state     | None                                                                  |
| extra                  | {u'hardware_swift_object': u'extra_hardware-11cc41d0-1efd-41e6-84cf-  |
|                        | 034cbecb587d'}                                                        |
| last_error             | None                                                                  |
| updated_at             | 2017-06-01T08:33:02+00:00                                             |
| maintenance_reason     | None                                                                  |
| provision_state        | available                                                             |
| clean_step             | {}                                                                    |
| uuid                   | 11cc41d0-1efd-41e6-84cf-034cbecb587d                                  |
| console_enabled        | False                                                                 |
| target_provision_state | None                                                                  |
| provision_updated_at   | 2017-06-01T08:33:02+00:00                                             |
| maintenance            | False                                                                 |
| inspection_started_at  | None                                                                  |
| inspection_finished_at | None                                                                  |
| power_state            | power off                                                             |
| driver                 | pxe_ipmitool                                                          |
| reservation            | None                                                                  |
| properties             | {u'memory_mb': u'6144', u'cpu_arch': u'x86_64', u'local_gb': u'39',   |
|                        | u'cpus': u'2', u'capabilities': u'profile:compute,boot_option:local'} |
| instance_uuid          | None                                                                  |
| name                   | compute1                                                              |
| driver_info            | {u'ipmi_password': u'******', u'ipmi_address': u'192.168.1.112',      |
|                        | u'ipmi_username': u'admin', u'deploy_kernel':                         |
|                        | u'9944fe63-3d76-4717-b912-a9c1fef9a8ac', u'deploy_ramdisk':           |
|                        | u'3ca3660e-d502-4cb4-9029-67a939b3d439'}                              |
| created_at             | 2017-06-01T08:29:42+00:00                                             |
| driver_internal_info   | {}                                                                    |
| chassis_uuid           |                                                                       |
| instance_info          | {}                                                                    |
+------------------------+-----------------------------------------------------------------------+
[stack@director ~]$ mkdir templates
[stack@director ~]$ cp -rf /usr/share/openstack-tripleo-heat-templates/* templates/
[stack@director ~]$ curl -s -O http://materials.example.com/overcloud-heat-templates/templates.tar.bz2
[stack@director ~]$ ls -lrt
total 36
-rw-rw-r--.  1 stack stack  626 Jun 10  2016 instackenv-twonodes.json
-rw-rw-r--.  1 stack stack  341 Jul 25  2016 undercloud.conf
-rw-rw-r--.  1 stack stack 1377 Jul 25  2016 undercloud-passwords.conf
-rw-------.  1 stack stack  291 Jul 25  2016 stackrc
-rw-rw-r--.  1 stack stack  233 Jul 25  2016 tempest-deployer-input.conf
drwxrwxr-x.  2 stack stack 4096 Jun  1 04:22 images
drwxrwxr-x. 10 stack stack 4096 Jun  1 04:40 templates
-rw-rw-r--.  1 stack stack 5116 Jun  1 04:42 templates.tar.bz2
[stack@director ~]$ tar xvfjp templates.tar.bz2 
templates/
templates/single-nic-vlans/
templates/single-nic-vlans/controller-v6.yaml
templates/single-nic-vlans/compute.yaml
templates/single-nic-vlans/swift-storage.yaml
templates/single-nic-vlans/README.md
templates/single-nic-vlans/controller-no-external.yaml
templates/single-nic-vlans/controller.yaml
templates/single-nic-vlans/cinder-storage.yaml
templates/single-nic-vlans/ceph-storage.yaml
templates/environments/
templates/environments/storage-environment.yaml
templates/firstboot/
templates/firstboot/wipe-disks.yaml
templates/firstboot/wipe-disk.sh
templates/compute-extraconfig.yaml
templates/pre-config-fix.yaml
templates/network-environment.yaml
templates/cloud-init-fix.yaml
[stack@director ~]$ ironic node-list
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
| UUID                                 | Name       | Instance UUID | Power State | Provisioning State | Maintenance |
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
| 822d3310-8fb5-4e1d-a218-5c5c6d4f4489 | controller | None          | power off   | available          | False       |
| 11cc41d0-1efd-41e6-84cf-034cbecb587d | compute1   | None          | power off   | available          | False       |
+--------------------------------------+------------+---------------+-------------+--------------------+-------------+
[stack@director ~]$ source stackrc 
[stack@director ~]$ openstack overcloud deploy --templates ~/templates --control-scale 1 --compute-scale 1 --control-flavor control --compute-flavor compute --neutron-tunnel-types vxlan --neutron-network-type vxlan -e ~/templates/compute-extraconfig.yaml -e ~/templates/environments/network-isolation.yaml -e ~/templates/network-environment.yaml -e ~/templates/pre-config-fix.yaml 
Deploying templates in the directory /home/stack/templates
2017-06-01 08:46:24 [overcloud]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:24 [VipConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:24 [MysqlClusterUniquePart]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:24 [PcsdPassword]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:24 [RabbitCookie]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:24 [overcloud-VipConfig-dts63wneub4y]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:24 [VipConfigImpl]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:24 [VipConfigImpl]: CREATE_COMPLETE  state changed
2017-06-01 08:46:25 [Networks]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:25 [HeatAuthEncryptionKey]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:25 [HorizonSecret]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:25 [MysqlRootPassword]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:25 [MysqlRootPassword]: CREATE_COMPLETE  state changed
2017-06-01 08:46:25 [HorizonSecret]: CREATE_COMPLETE  state changed
2017-06-01 08:46:25 [VipConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:46:25 [overcloud-Networks-ivj7i2hdisjw]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:25 [StorageNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:25 [ManagementNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:25 [overcloud-Networks-ivj7i2hdisjw-StorageNetwork-mjaxzrqhopct]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:25 [overcloud-VipConfig-dts63wneub4y]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:26 [PcsdPassword]: CREATE_COMPLETE  state changed
2017-06-01 08:46:26 [RabbitCookie]: CREATE_COMPLETE  state changed
2017-06-01 08:46:26 [MysqlClusterUniquePart]: CREATE_COMPLETE  state changed
2017-06-01 08:46:26 [HeatAuthEncryptionKey]: CREATE_COMPLETE  state changed
2017-06-01 08:46:26 [StorageMgmtNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:26 [TenantNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:26 [overcloud-Networks-ivj7i2hdisjw-ManagementNetwork-tnqfi4cq7nrm]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:26 [overcloud-Networks-ivj7i2hdisjw-ManagementNetwork-tnqfi4cq7nrm]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:26 [overcloud-Networks-ivj7i2hdisjw-StorageMgmtNetwork-6m4jm6peycvp]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:26 [StorageMgmtNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:26 [StorageNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:26 [StorageNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:26 [StorageSubnet]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:27 [ExternalNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:27 [InternalNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:27 [ManagementNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:27 [overcloud-Networks-ivj7i2hdisjw-InternalNetwork-v7q45gt4auao]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:27 [StorageMgmtNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:27 [StorageMgmtSubnet]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:27 [overcloud-Networks-ivj7i2hdisjw-TenantNetwork-3jays6ttw7hj]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:27 [TenantNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:27 [TenantNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:27 [TenantSubnet]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:27 [overcloud-Networks-ivj7i2hdisjw-ExternalNetwork-cgvocgevasev]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:27 [ExternalNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:28 [InternalApiNetwork]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:28 [InternalApiNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:28 [InternalApiSubnet]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:28 [StorageMgmtSubnet]: CREATE_COMPLETE  state changed
2017-06-01 08:46:28 [overcloud-Networks-ivj7i2hdisjw-StorageMgmtNetwork-6m4jm6peycvp]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:28 [StorageSubnet]: CREATE_COMPLETE  state changed
2017-06-01 08:46:28 [overcloud-Networks-ivj7i2hdisjw-StorageNetwork-mjaxzrqhopct]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:28 [ExternalNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:28 [ExternalSubnet]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:29 [StorageNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:29 [TenantSubnet]: CREATE_COMPLETE  state changed
2017-06-01 08:46:29 [overcloud-Networks-ivj7i2hdisjw-TenantNetwork-3jays6ttw7hj]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:29 [ExternalSubnet]: CREATE_COMPLETE  state changed
2017-06-01 08:46:29 [overcloud-Networks-ivj7i2hdisjw-ExternalNetwork-cgvocgevasev]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:30 [TenantNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:30 [ExternalNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:30 [InternalNetwork]: CREATE_COMPLETE  state changed
2017-06-01 08:46:30 [overcloud-Networks-ivj7i2hdisjw]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:30 [InternalApiSubnet]: CREATE_COMPLETE  state changed
2017-06-01 08:46:30 [overcloud-Networks-ivj7i2hdisjw-InternalNetwork-v7q45gt4auao]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:31 [Networks]: CREATE_COMPLETE  state changed
2017-06-01 08:46:31 [CephStorage]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:32 [overcloud-CephStorage-a6axtrxm2tcz]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:32 [overcloud-CephStorage-a6axtrxm2tcz]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:33 [ControlVirtualIP]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:33 [ObjectStorage]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:33 [overcloud-ObjectStorage-w2qf6caezhi2]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:33 [overcloud-CephStorage-a6axtrxm2tcz]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:46:33 [overcloud-CephStorage-a6axtrxm2tcz]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:46:34 [overcloud-ObjectStorage-w2qf6caezhi2]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:35 [overcloud-ObjectStorage-w2qf6caezhi2]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:46:35 [overcloud-ObjectStorage-w2qf6caezhi2]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:46:36 [ControlVirtualIP]: CREATE_COMPLETE  state changed
2017-06-01 08:46:36 [CephStorage]: CREATE_COMPLETE  state changed
2017-06-01 08:46:36 [ObjectStorage]: CREATE_COMPLETE  state changed
2017-06-01 08:46:36 [StorageVirtualIP]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:37 [PublicVirtualIP]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:37 [StorageMgmtVirtualIP]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:37 [InternalApiVirtualIP]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:37 [overcloud-StorageVirtualIP-ux4fzt3blyux]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:37 [StoragePort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:37 [StoragePort]: CREATE_COMPLETE  state changed
2017-06-01 08:46:37 [overcloud-StorageVirtualIP-ux4fzt3blyux]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:37 [overcloud-StorageMgmtVirtualIP-cvausjkgmsio]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:37 [StorageMgmtPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:37 [overcloud-PublicVirtualIP-3bejypdkz5sk]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:37 [ExternalPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:37 [ExternalPort]: CREATE_COMPLETE  state changed
2017-06-01 08:46:37 [overcloud-PublicVirtualIP-3bejypdkz5sk]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:38 [RedisVirtualIP]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:38 [overcloud-InternalApiVirtualIP-d7jm6ma6ynni]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:38 [InternalApiPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:38 [InternalApiPort]: CREATE_COMPLETE  state changed
2017-06-01 08:46:38 [overcloud-InternalApiVirtualIP-d7jm6ma6ynni]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:38 [overcloud-RedisVirtualIP-gaidfaljghtu]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:38 [VipPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:38 [StorageMgmtPort]: CREATE_COMPLETE  state changed
2017-06-01 08:46:38 [overcloud-StorageMgmtVirtualIP-cvausjkgmsio]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:39 [InternalApiVirtualIP]: CREATE_COMPLETE  state changed
2017-06-01 08:46:39 [PublicVirtualIP]: CREATE_COMPLETE  state changed
2017-06-01 08:46:39 [VipPort]: CREATE_COMPLETE  state changed
2017-06-01 08:46:39 [overcloud-RedisVirtualIP-gaidfaljghtu]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:40 [StorageVirtualIP]: CREATE_COMPLETE  state changed
2017-06-01 08:46:40 [StorageMgmtVirtualIP]: CREATE_COMPLETE  state changed
2017-06-01 08:46:40 [RedisVirtualIP]: CREATE_COMPLETE  state changed
2017-06-01 08:46:40 [VipMap]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:40 [overcloud-VipMap-br6253f5qjjs]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:40 [overcloud-VipMap-br6253f5qjjs]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:41 [VipMap]: CREATE_COMPLETE  state changed
2017-06-01 08:46:41 [EndpointMap]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:42 [overcloud-EndpointMap-ijqtxmegh3g7]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:42 [overcloud-EndpointMap-ijqtxmegh3g7]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:43 [EndpointMap]: CREATE_COMPLETE  state changed
2017-06-01 08:46:43 [BlockStorage]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:43 [overcloud-BlockStorage-zwvc5gxxuxzs]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:43 [overcloud-BlockStorage-zwvc5gxxuxzs]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:45 [Compute]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:45 [overcloud-BlockStorage-zwvc5gxxuxzs]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:46:45 [overcloud-BlockStorage-zwvc5gxxuxzs]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:46:45 [overcloud-Compute-c5ezlcqlcblu]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:45 [overcloud-Compute-c5ezlcqlcblu]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:47 [Controller]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:47 [overcloud-Controller-uz3qjfpa7vbo]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:47 [overcloud-Controller-uz3qjfpa7vbo]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:46:47 [overcloud-Compute-c5ezlcqlcblu]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:46:47 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:48 [overcloud-Compute-c5ezlcqlcblu-0-42qmmqqvfvvd]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:48 [NovaComputeConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:49 [overcloud-Controller-uz3qjfpa7vbo]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:46:49 [NodeAdminUserData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:49 [NodeUserData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:49 [UpdateConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:50 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:50 [NovaComputeConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:46:51 [NodeAdminUserData]: CREATE_COMPLETE  state changed
2017-06-01 08:46:51 [NodeUserData]: CREATE_COMPLETE  state changed
2017-06-01 08:46:51 [UpdateConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:46:52 [overcloud-Controller-uz3qjfpa7vbo-0-kxbglgiwux5w]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:52 [NodeUserData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:52 [UserData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:52 [overcloud-Controller-uz3qjfpa7vbo-0-kxbglgiwux5w]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:46:52 [NodeUserData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:52 [UpdateConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:53 [NodeAdminUserData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:53 [UpdateConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:46:53 [NodeUserData]: CREATE_COMPLETE  state changed
2017-06-01 08:46:53 [UserData]: CREATE_COMPLETE  state changed
2017-06-01 08:46:53 [NovaCompute]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:56 [NodeAdminUserData]: CREATE_COMPLETE  state changed
2017-06-01 08:46:56 [UserData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:46:57 [UserData]: CREATE_COMPLETE  state changed
2017-06-01 08:46:57 [Controller]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:15 [Controller]: CREATE_COMPLETE  state changed
2017-06-01 08:52:15 [NovaCompute]: CREATE_COMPLETE  state changed
2017-06-01 08:52:15 [InternalApiPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:16 [ManagementPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:16 [UpdateDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:16 [ManagementPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:17 [StoragePort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:18 [StorageMgmtPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:18 [ExternalPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:19 [InternalApiPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:19 [ExternalPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:19 [TenantPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:20 [UpdateDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:21 [StorageMgmtPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:22 [TenantPort]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:23 [InternalApiPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:23 [ManagementPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:24 [ExternalPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:24 [ManagementPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:24 [ExternalPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:24 [TenantPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:25 [StorageMgmtPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:25 [InternalApiPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:25 [TenantPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:25 [StorageMgmtPort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:25 [StoragePort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:26 [StoragePort]: CREATE_COMPLETE  state changed
2017-06-01 08:52:26 [NetworkConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:27 [NetworkConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:27 [NetIpMap]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:28 [NetIpSubnetMap]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:29 [NetIpMap]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:30 [NetworkConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:52:30 [NetIpMap]: CREATE_COMPLETE  state changed
2017-06-01 08:52:30 [NetworkDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:31 [NetIpSubnetMap]: CREATE_COMPLETE  state changed
2017-06-01 08:52:31 [NetIpMap]: CREATE_COMPLETE  state changed
2017-06-01 08:52:32 [NetworkConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:52:32 [NetworkDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:50 [UpdateDeployment]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:52:51 [UpdateDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:52:51 [NetworkDeployment]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:52:52 [NetworkDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:52:52 [NovaComputeDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:57 [UpdateDeployment]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:52:58 [NetworkDeployment]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:52:58 [UpdateDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:52:59 [NetworkDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:52:59 [NodeTLSCAData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:52:59 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:00 [NodeTLSCAData]: CREATE_COMPLETE  state changed
2017-06-01 08:53:00 [NodeTLSData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:00 [NovaComputeDeployment]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:53:00 [NovaComputeDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:53:00 [NodeTLSCAData]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:01 [ComputeExtraConfigPre]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:02 [NodeTLSData]: CREATE_COMPLETE  state changed
2017-06-01 08:53:02 [ControllerConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:02 [NodeTLSCAData]: CREATE_COMPLETE  state changed
2017-06-01 08:53:03 [ControllerConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:03 [ControllerDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:03 [ComputeExtraConfigPre]: CREATE_COMPLETE  state changed
2017-06-01 08:53:03 [NodeExtraConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:03 [ControllerConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:03 [ControllerDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:18 [ControllerDeployment]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:53:18 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:19 [ControllerDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:53:19 [ControllerExtraConfigPre]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:19 [NodeExtraConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:19 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:19 [overcloud-Compute-c5ezlcqlcblu-0-42qmmqqvfvvd]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:19 [NovaComputeDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:20 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:53:20 [overcloud-Compute-c5ezlcqlcblu]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:53:21 [ControllerExtraConfigPre]: CREATE_COMPLETE  state changed
2017-06-01 08:53:21 [NodeExtraConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:21 [Compute]: CREATE_COMPLETE  state changed
2017-06-01 08:53:35 [NodeExtraConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:35 [overcloud-Controller-uz3qjfpa7vbo-0-kxbglgiwux5w]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:36 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:53:36 [overcloud-Controller-uz3qjfpa7vbo]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:53:36 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:36 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:37 [Controller]: CREATE_COMPLETE  state changed
2017-06-01 08:53:38 [AllNodesValidationConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:38 [VipDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:38 [overcloud-AllNodesValidationConfig-f5we4coawoja]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:39 [overcloud-VipDeployment-sygsnkd5mo5g]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:39 [overcloud-VipDeployment-sygsnkd5mo5g]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:39 [AllNodesValidationsImpl]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:39 [AllNodesValidationsImpl]: CREATE_COMPLETE  state changed
2017-06-01 08:53:39 [overcloud-AllNodesValidationConfig-f5we4coawoja]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:40 [ControllerIpListMap]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:40 [overcloud-VipDeployment-sygsnkd5mo5g]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:53:41 [UpdateWorkflow]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:41 [SwiftDevicesAndProxyConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:41 [overcloud-UpdateWorkflow-yhh7xnmnym54]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:41 [overcloud-UpdateWorkflow-yhh7xnmnym54]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:41 [overcloud-ControllerIpListMap-f7lptppjbwu6]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:41 [overcloud-ControllerIpListMap-f7lptppjbwu6]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:41 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:42 [ControllerClusterConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:42 [ControllerBootstrapNodeConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:42 [overcloud-SwiftDevicesAndProxyConfig-hjoowgrsksm4]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:42 [SwiftDevicesAndProxyConfigImpl]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:42 [SwiftDevicesAndProxyConfigImpl]: CREATE_COMPLETE  state changed
2017-06-01 08:53:42 [overcloud-SwiftDevicesAndProxyConfig-hjoowgrsksm4]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:42 [overcloud-ControllerBootstrapNodeConfig-gosshcwgkjyj]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:42 [BootstrapNodeConfigImpl]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:42 [BootstrapNodeConfigImpl]: CREATE_COMPLETE  state changed
2017-06-01 08:53:42 [overcloud-ControllerBootstrapNodeConfig-gosshcwgkjyj]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:43 [UpdateWorkflow]: CREATE_COMPLETE  state changed
2017-06-01 08:53:43 [AllNodesValidationConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:43 [ControllerIpListMap]: CREATE_COMPLETE  state changed
2017-06-01 08:53:44 [SwiftDevicesAndProxyConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:44 [ControllerClusterConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:44 [ControllerBootstrapNodeConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:44 [CephClusterConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:44 [allNodesConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:44 [ControllerSwiftDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:44 [overcloud-allNodesConfig-uboc6rsz6nw4]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:44 [allNodesConfigImpl]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:44 [overcloud-CephClusterConfig-5ucsaomyoexz]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:44 [CephClusterConfigImpl]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:44 [CephClusterConfigImpl]: CREATE_COMPLETE  state changed
2017-06-01 08:53:44 [overcloud-CephClusterConfig-5ucsaomyoexz]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:45 [allNodesConfigImpl]: CREATE_COMPLETE  state changed
2017-06-01 08:53:45 [overcloud-allNodesConfig-uboc6rsz6nw4]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:45 [overcloud-ControllerSwiftDeployment-6jy42tovug6k]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:45 [overcloud-ControllerSwiftDeployment-6jy42tovug6k]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:46 [overcloud-ControllerSwiftDeployment-6jy42tovug6k]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:53:47 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:47 [overcloud-ControllerClusterDeployment-hno4qmrh2yip]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:47 [overcloud-ControllerClusterDeployment-hno4qmrh2yip]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:48 [ObjectStorageSwiftDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:48 [overcloud-ControllerClusterDeployment-hno4qmrh2yip]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:53:48 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:49 [ControllerBootstrapNodeDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:49 [overcloud-ObjectStorageSwiftDeployment-etyh3w6gnelr]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:49 [overcloud-ObjectStorageSwiftDeployment-etyh3w6gnelr]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:49 [overcloud-ObjectStorageSwiftDeployment-etyh3w6gnelr]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:53:50 [overcloud-ObjectStorageSwiftDeployment-etyh3w6gnelr]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:53:50 [overcloud-ControllerBootstrapNodeDeployment-676yw4mtqlj5]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:50 [overcloud-ControllerBootstrapNodeDeployment-676yw4mtqlj5]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:51 [overcloud-ControllerBootstrapNodeDeployment-676yw4mtqlj5]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:53:52 [CephClusterConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:52 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:53 [allNodesConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:53:53 [ObjectStorageSwiftDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:53:53 [CephStorageCephDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:53 [overcloud-CephStorageCephDeployment-d46k7veogx24]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:53 [overcloud-CephStorageCephDeployment-d46k7veogx24]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:54 [ControllerCephDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:54 [overcloud-CephStorageCephDeployment-d46k7veogx24]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:53:55 [overcloud-ControllerCephDeployment-hkzkzk2vrfpm]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:53:55 [overcloud-ControllerCephDeployment-hkzkzk2vrfpm]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:53:55 [overcloud-CephStorageCephDeployment-d46k7veogx24]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:53:56 [overcloud-ControllerCephDeployment-hkzkzk2vrfpm]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:53:57 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:53:57 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:57 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:53:58 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:53:58 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:53:59 [overcloud-VipDeployment-sygsnkd5mo5g]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:53:59 [overcloud-BlockStorageAllNodesDeployment-c6bf3ua4khj7]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:00 [ObjectStorageAllNodesDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:00 [overcloud-BlockStorageAllNodesDeployment-c6bf3ua4khj7]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:00 [overcloud-ObjectStorageAllNodesDeployment-ntjybg2imfvg]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:54:00 [overcloud-ObjectStorageAllNodesDeployment-ntjybg2imfvg]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:54:01 [ComputeCephDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:01 [overcloud-ObjectStorageAllNodesDeployment-ntjybg2imfvg]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:01 [overcloud-ObjectStorageAllNodesDeployment-ntjybg2imfvg]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:01 [overcloud-ComputeCephDeployment-ujl22ob4mypy]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:54:01 [overcloud-ComputeCephDeployment-ujl22ob4mypy]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:54:02 [ComputeAllNodesDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:02 [overcloud-ComputeAllNodesDeployment-sduysyyjmkcl]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:54:02 [overcloud-ComputeAllNodesDeployment-sduysyyjmkcl]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:54:02 [overcloud-ComputeCephDeployment-ujl22ob4mypy]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:02 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:04 [ControllerAllNodesDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:04 [overcloud-ComputeAllNodesDeployment-sduysyyjmkcl]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:04 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:04 [overcloud-ControllerAllNodesDeployment-ikwg75bpigpq]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:54:04 [overcloud-ControllerAllNodesDeployment-ikwg75bpigpq]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:54:05 [overcloud-ControllerAllNodesDeployment-ikwg75bpigpq]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:06 [VipDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:06 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:07 [CephStorageAllNodesDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:07 [ObjectStorageAllNodesDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:07 [BlockStorageAllNodesDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:07 [CephStorageCephDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:07 [BlockStorageAllNodesValidationDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:08 [overcloud-BlockStorageAllNodesValidationDeployment-hnllg5isp4sc]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:54:08 [overcloud-BlockStorageAllNodesValidationDeployment-hnllg5isp4sc]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:54:09 [overcloud-BlockStorageAllNodesValidationDeployment-hnllg5isp4sc]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:10 [overcloud-BlockStorageAllNodesValidationDeployment-hnllg5isp4sc]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:13 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:54:14 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:54:15 [overcloud-ControllerBootstrapNodeDeployment-676yw4mtqlj5]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:15 [ControllerBootstrapNodeDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:15 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:54:16 [CephStorageAllNodesValidationDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:16 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:54:16 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:16 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:17 [ControllerCephDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:17 [ControllerClusterDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:17 [ControllerSwiftDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:17 [overcloud-ControllerSwiftDeployment-6jy42tovug6k]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:17 [ControllerCephDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:17 [ControllerClusterDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:17 [ControllerSwiftDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:33 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:54:33 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:54:33 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:54:34 [overcloud-ComputeAllNodesDeployment-sduysyyjmkcl]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:34 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:34 [NovaComputeDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:34 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:54:34 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:54:34 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:54:35 [overcloud-ControllerAllNodesDeployment-ikwg75bpigpq]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:35 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:35 [overcloud-ComputeCephDeployment-ujl22ob4mypy]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:36 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:36 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:36 [ControllerAllNodesDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:54:36 [ComputeAllNodesValidationDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:36 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:36 [overcloud-ComputeAllNodesValidationDeployment-ggwocdndksst]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:54:36 [overcloud-ComputeAllNodesValidationDeployment-ggwocdndksst]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:54:37 [ControllerAllNodesValidationDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:37 [overcloud-ControllerAllNodesValidationDeployment-zcbk4io6xznq]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:54:37 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:37 [overcloud-ComputeAllNodesValidationDeployment-ggwocdndksst]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:37 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:38 [overcloud-ControllerAllNodesValidationDeployment-zcbk4io6xznq]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:54:38 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:38 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:39 [overcloud-ControllerAllNodesValidationDeployment-zcbk4io6xznq]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:54:39 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:54:49 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:49 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:54:50 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:54:50 [overcloud-ComputeAllNodesValidationDeployment-ggwocdndksst]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:50 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:51 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:51 [NovaComputeDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:58 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:54:58 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:54:58 [overcloud-ControllerAllNodesValidationDeployment-zcbk4io6xznq]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:54:58 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:59 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:54:59 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:00 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:00 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:00 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:01 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:01 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:02 [ObjectStorageNodesPostDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:02 [ControllerNodesPostDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:02 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:02 [ComputeArtifactsConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:03 [ComputePuppetConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:03 [ComputePuppetConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:03 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ComputeArtifactsConfig-roi577ufwdtu]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:03 [DeployArtifacts]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:03 [DeployArtifacts]: CREATE_COMPLETE  state changed
2017-06-01 08:55:03 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ComputeArtifactsConfig-roi577ufwdtu]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:03 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:03 [StorageArtifactsConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:03 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageArtifactsConfig-k3oqjwmnp6mm]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:04 [ComputeArtifactsConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:04 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:04 [ControllerPrePuppet]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:04 [ControllerArtifactsConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:04 [StorageRingbuilderPuppetConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:04 [StoragePuppetConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:04 [StorageRingbuilderPuppetConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:04 [StorageArtifactsConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:04 [StoragePuppetConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:04 [DeployArtifacts]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:04 [DeployArtifacts]: CREATE_COMPLETE  state changed
2017-06-01 08:55:04 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageArtifactsConfig-k3oqjwmnp6mm]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:05 [ComputeArtifactsDeploy]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:05 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ComputeArtifactsDeploy-v6rql2bl2vsj]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:05 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ComputeArtifactsDeploy-v6rql2bl2vsj]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:05 [ControllerPuppetConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:05 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerArtifactsConfig-lmmfmefryiht]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:05 [DeployArtifacts]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:05 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerPrePuppet-qkqdhjtocr3k]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:05 [ControllerPrePuppetMaintenanceModeConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:05 [ControllerPrePuppetMaintenanceModeConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:05 [ControllerPrePuppetMaintenanceModeDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:05 [StorageArtifactsDeploy]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:05 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageArtifactsDeploy-clrsihs7rbod]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:05 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageArtifactsDeploy-clrsihs7rbod]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:06 [ControllerRingbuilderPuppetConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:06 [ControllerArtifactsConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:06 [ControllerRingbuilderPuppetConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:06 [DeployArtifacts]: CREATE_COMPLETE  state changed
2017-06-01 08:55:06 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerArtifactsConfig-lmmfmefryiht]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:06 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerPuppetConfig-g43gfe432lwz]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:06 [ControllerPuppetConfigImpl]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:06 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageArtifactsDeploy-clrsihs7rbod]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:55:07 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ComputeArtifactsDeploy-v6rql2bl2vsj]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:55:07 [ControllerArtifactsDeploy]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:07 [ControllerPuppetConfigImpl]: CREATE_COMPLETE  state changed
2017-06-01 08:55:07 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerPuppetConfig-g43gfe432lwz]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:07 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerArtifactsDeploy-ozdhw7arx7qp]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:07 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageArtifactsDeploy-clrsihs7rbod]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:55:08 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:08 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerArtifactsDeploy-ozdhw7arx7qp]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:08 [StorageArtifactsDeploy]: CREATE_COMPLETE  state changed
2017-06-01 08:55:08 [StorageDeployment_Step1]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:09 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerArtifactsDeploy-ozdhw7arx7qp]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:55:09 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageDeployment_Step1-uqib6l5amr6t]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:09 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageDeployment_Step1-uqib6l5amr6t]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:10 [ControllerPuppetConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:10 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:10 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageDeployment_Step1-uqib6l5amr6t]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:55:11 [StorageDeployment_Step1]: CREATE_COMPLETE  state changed
2017-06-01 08:55:11 [StorageRingbuilderDeployment_Step2]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:11 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageDeployment_Step1-uqib6l5amr6t]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:55:12 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageRingbuilderDeployment_Step2-7xhydp4ut3jp]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:12 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageRingbuilderDeployment_Step2-7xhydp4ut3jp]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:13 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageRingbuilderDeployment_Step2-7xhydp4ut3jp]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:55:14 [StorageRingbuilderDeployment_Step2]: CREATE_COMPLETE  state changed
2017-06-01 08:55:14 [ExtraConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:14 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-StorageRingbuilderDeployment_Step2-7xhydp4ut3jp]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:55:15 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-ExtraConfig-qic47zfzbxns]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:15 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s-ExtraConfig-qic47zfzbxns]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:16 [ExtraConfig]: CREATE_COMPLETE  state changed
2017-06-01 08:55:16 [overcloud-ObjectStorageNodesPostDeployment-mi5cx34v2k6s]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:18 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:55:18 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:55:19 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:19 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:20 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:21 [NovaComputeDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:21 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:28 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:55:29 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:55:29 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerArtifactsDeploy-ozdhw7arx7qp]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:55:30 [ControllerArtifactsDeploy]: CREATE_COMPLETE  state changed
2017-06-01 08:55:30 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:30 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:31 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:31 [ControllerPrePuppetMaintenanceModeDeployment]: CREATE_COMPLETE  state changed
2017-06-01 08:55:31 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerPrePuppet-qkqdhjtocr3k]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:31 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:32 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:32 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:32 [ControllerPrePuppet]: CREATE_COMPLETE  state changed
2017-06-01 08:55:32 [ControllerLoadBalancerDeployment_Step1]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:55:32 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerLoadBalancerDeployment_Step1-jua4jvr6x7y2]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:55:33 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:33 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:55:33 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerLoadBalancerDeployment_Step1-jua4jvr6x7y2]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:55:33 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerLoadBalancerDeployment_Step1-jua4jvr6x7y2]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:55:33 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:56:38 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:56:39 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:56:40 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:56:40 [ControllerLoadBalancerDeployment_Step1]: CREATE_COMPLETE  state changed
2017-06-01 08:56:40 [ControllerServicesBaseDeployment_Step2]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:56:40 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerServicesBaseDeployment_Step2-qu6bzn2qclzy]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:56:41 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:56:41 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:56:41 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerServicesBaseDeployment_Step2-qu6bzn2qclzy]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:56:41 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:56:42 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:56:42 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:56:42 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerServicesBaseDeployment_Step2-qu6bzn2qclzy]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:56:42 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:56:43 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:36 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:57:37 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:57:37 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerServicesBaseDeployment_Step2-qu6bzn2qclzy]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:57:37 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:38 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:38 [ControllerServicesBaseDeployment_Step2]: CREATE_COMPLETE  state changed
2017-06-01 08:57:38 [ControllerRingbuilderDeployment_Step3]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:57:38 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerRingbuilderDeployment_Step3-svoc6e3ikk3e]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:57:38 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:38 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:39 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:39 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:39 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerRingbuilderDeployment_Step3-svoc6e3ikk3e]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:57:40 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:40 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:57:40 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerRingbuilderDeployment_Step3-svoc6e3ikk3e]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:57:41 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:58:05 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 08:58:06 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:58:07 [0]: CREATE_COMPLETE  state changed
2017-06-01 08:58:07 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerRingbuilderDeployment_Step3-svoc6e3ikk3e]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 08:58:07 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:58:08 [ControllerRingbuilderDeployment_Step3]: CREATE_COMPLETE  state changed
2017-06-01 08:58:08 [ControllerOvercloudServicesDeployment_Step4]: CREATE_IN_PROGRESS  state changed
2017-06-01 08:58:08 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:58:09 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:58:09 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:58:09 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step4-ubpyaecwpqhw]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 08:58:09 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step4-ubpyaecwpqhw]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 08:58:10 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:58:10 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 08:58:10 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step4-ubpyaecwpqhw]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 08:58:11 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:00:56 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 09:00:56 [0]: CREATE_COMPLETE  state changed
2017-06-01 09:00:57 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step4-ubpyaecwpqhw]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 09:00:57 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:00:57 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:00:58 [ControllerOvercloudServicesDeployment_Step4]: CREATE_COMPLETE  state changed
2017-06-01 09:00:58 [ControllerOvercloudServicesDeployment_Step5]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:00:58 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step5-qfgaw7x5hfac]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:00:59 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step5-qfgaw7x5hfac]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:00:59 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:00:59 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:00:59 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:00:59 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step5-qfgaw7x5hfac]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:01:00 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:01:00 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:01:00 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step5-qfgaw7x5hfac]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 09:01:00 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:01:00 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:50 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 09:02:51 [0]: CREATE_COMPLETE  state changed
2017-06-01 09:02:52 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:53 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:53 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:53 [ControllerOvercloudServicesDeployment_Step5]: CREATE_COMPLETE  state changed
2017-06-01 09:02:53 [ControllerOvercloudServicesDeployment_Step6]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:02:54 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step6-fmn76dslczpu]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:02:54 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step6-fmn76dslczpu]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:02:54 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerOvercloudServicesDeployment_Step6-fmn76dslczpu]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 09:02:54 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:55 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:55 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:55 [0]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:02:56 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:02:56 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:21 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 09:04:21 [0]: CREATE_COMPLETE  state changed
2017-06-01 09:04:22 [ComputePuppetDeployment]: CREATE_COMPLETE  state changed
2017-06-01 09:04:22 [ExtraConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:04:22 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ComputePuppetDeployment-ux7pn75frnk6]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 09:04:22 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ExtraConfig-q22rudvxxesl]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:04:22 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:23 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e-ExtraConfig-q22rudvxxesl]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:04:23 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:23 [NovaComputeDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:23 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:24 [ExtraConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:04:24 [overcloud-ComputeNodesPostDeployment-y62fbo3hea7e]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:04:25 [ComputeNodesPostDeployment]: CREATE_COMPLETE  state changed
2017-06-01 09:04:33 [0]: SIGNAL_IN_PROGRESS  Signal: deployment succeeded
2017-06-01 09:04:34 [0]: CREATE_COMPLETE  state changed
2017-06-01 09:04:34 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:35 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:36 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:36 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerPostPuppet-7xzde4enrkva]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:04:36 [ControllerPostPuppetMaintenanceModeConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:04:36 [ControllerPostPuppetRestartConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:04:36 [ControllerPostPuppetMaintenanceModeConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:04:36 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:37 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:37 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:37 [ControllerPostPuppetRestartConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:04:37 [ControllerPostPuppetMaintenanceModeDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:04:38 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:04:38 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:02 [ControllerPostPuppetMaintenanceModeDeployment]: CREATE_COMPLETE  state changed
2017-06-01 09:05:02 [ControllerPostPuppetRestartDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:02 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:03 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:04 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:05 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:05 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:05 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:06 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:30 [ControllerPostPuppetRestartDeployment]: CREATE_COMPLETE  state changed
2017-06-01 09:05:30 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ControllerPostPuppet-7xzde4enrkva]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:30 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:30 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:31 [ControllerPostPuppet]: CREATE_COMPLETE  state changed
2017-06-01 09:05:31 [ExtraConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:31 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ExtraConfig-34zcblq4lofh]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:31 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6-ExtraConfig-34zcblq4lofh]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:32 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:32 [ControllerDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:32 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:33 [NetworkDeployment]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:33 [ExtraConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:05:33 [overcloud-ControllerNodesPostDeployment-dfspx47zxwq6]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:34 [0]: SIGNAL_COMPLETE  Unknown
2017-06-01 09:05:34 [CephStorageNodesPostDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:34 [BlockStorageNodesPostDeployment]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:34 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:35 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:35 [VolumeArtifactsConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:35 [CephStorageArtifactsConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:35 [CephStoragePuppetConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:35 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageArtifactsConfig-fipzyc7vgohm]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:36 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeArtifactsConfig-6s7nzcjgxfkj]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:36 [DeployArtifacts]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:36 [DeployArtifacts]: CREATE_COMPLETE  state changed
2017-06-01 09:05:36 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeArtifactsConfig-6s7nzcjgxfkj]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:36 [CephStoragePuppetConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:05:36 [DeployArtifacts]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:36 [DeployArtifacts]: CREATE_COMPLETE  state changed
2017-06-01 09:05:36 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageArtifactsConfig-fipzyc7vgohm]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:37 [VolumeArtifactsConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:05:37 [VolumeArtifactsDeploy]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:37 [CephStorageArtifactsConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:05:37 [CephStorageArtifactsDeploy]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:38 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeArtifactsDeploy-dxxjryu77y77]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:38 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeArtifactsDeploy-dxxjryu77y77]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:38 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageArtifactsDeploy-gae75w7idtlu]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:38 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageArtifactsDeploy-gae75w7idtlu]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:38 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageArtifactsDeploy-gae75w7idtlu]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 09:05:39 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeArtifactsDeploy-dxxjryu77y77]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 09:05:39 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageArtifactsDeploy-gae75w7idtlu]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 09:05:40 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeArtifactsDeploy-dxxjryu77y77]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 09:05:40 [CephStorageArtifactsDeploy]: CREATE_COMPLETE  state changed
2017-06-01 09:05:40 [CephStorageDeployment_Step1]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:40 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageDeployment_Step1-sukzupddybax]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:40 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageDeployment_Step1-sukzupddybax]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:41 [VolumeArtifactsDeploy]: CREATE_COMPLETE  state changed
2017-06-01 09:05:41 [VolumePuppetConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:41 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageDeployment_Step1-sukzupddybax]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 09:05:41 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-CephStorageDeployment_Step1-sukzupddybax]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 09:05:42 [VolumePuppetConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:05:42 [VolumeDeployment_Step1]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:42 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeDeployment_Step1-kb75gzmue234]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:42 [CephStorageDeployment_Step1]: CREATE_COMPLETE  state changed
2017-06-01 09:05:42 [ExtraConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:42 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-ExtraConfig-hnibooj5ff2a]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:42 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5-ExtraConfig-hnibooj5ff2a]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:43 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeDeployment_Step1-kb75gzmue234]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:43 [ExtraConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:05:44 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeDeployment_Step1-kb75gzmue234]: UPDATE_IN_PROGRESS  Stack UPDATE started
2017-06-01 09:05:44 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-VolumeDeployment_Step1-kb75gzmue234]: UPDATE_COMPLETE  Stack UPDATE completed successfully
2017-06-01 09:05:44 [overcloud-CephStorageNodesPostDeployment-ro4dq4e5hag5]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:45 [CephStorageNodesPostDeployment]: CREATE_COMPLETE  state changed
2017-06-01 09:05:45 [VolumeDeployment_Step1]: CREATE_COMPLETE  state changed
2017-06-01 09:05:45 [ExtraConfig]: CREATE_IN_PROGRESS  state changed
2017-06-01 09:05:45 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-ExtraConfig-77zxjkptdjax]: CREATE_IN_PROGRESS  Stack CREATE started
2017-06-01 09:05:46 [ExtraConfig]: CREATE_COMPLETE  state changed
2017-06-01 09:05:46 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag-ExtraConfig-77zxjkptdjax]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:47 [BlockStorageNodesPostDeployment]: CREATE_COMPLETE  state changed
2017-06-01 09:05:47 [overcloud]: CREATE_COMPLETE  Stack CREATE completed successfully
2017-06-01 09:05:47 [overcloud-BlockStorageNodesPostDeployment-swmbetv4ypag]: CREATE_COMPLETE  Stack CREATE completed successfully
Stack overcloud CREATE_COMPLETE
/home/stack/.ssh/known_hosts updated.
Original contents retained as /home/stack/.ssh/known_hosts.old
PKI initialization in init-keystone is deprecated and will be removed.
Warning: Permanently added '172.25.250.25' (ECDSA) to the list of known hosts.
No handlers could be found for logger "oslo_config.cfg"
2017-06-01 09:05:57.506 13805 WARNING keystone.cmd.cli [-] keystone-manage pki_setup is not recommended for production use.
The following cert files already exist, use --rebuild to remove the existing files before regenerating:
/etc/keystone/ssl/certs/ca.pem already exists
/etc/keystone/ssl/private/signing_key.pem already exists
/etc/keystone/ssl/certs/signing_cert.pem already exists
Connection to 172.25.250.25 closed.
Overcloud Endpoint: http://192.168.0.11:5000/v2.0
Overcloud Deployed
