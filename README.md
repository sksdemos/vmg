

# Ansible role `vmg` v0.2 (VM Manifest Generator)

Ansible role that generates manifests for the libvirt_setup and cloud_init roles. Although manual creation of these manifests offer additional customizations ther are prone to errors. This role uses a bareminimal list of variables that can be used to override the role defaults for both these roles.

## Requirements
This role has the following requirements:
 - No specific requirements

## Role Variables
This role requires a set of variables along with the dictionary  **cloud_vms_info**  in role/defaults that require to be overridden to create a set of VMs. The dictionary uses a key that acts as the VMs name and a set of VM related parameters to be used on a per VM basis. 

### vmg_vms_infra.<VM_NAME> dictionary variables
This dictionary is used initialize when a new network is required for VMs for a specific deployment that need to use a specific IP address range that does not interfere with existing applications.
| Variable | Comments |
| :---  | :--- |
| vm_image_link | URL for a specific cloud image
| vm_local_hostname | Hostname assigned to the VM during the cloud-init process.
| vm_fqdn | Fully qualified domain name of the Virtual machine. Used by cloud-init
| vm_root_passwd | Password to set for the root user during the cloud-init process
| vm_os_type | Type of the OS being installed. i.e. **windows** or **linux**
| vm_os_variant | Variant of the OS being installed. Run the **virt-install --os-variant list** command to see a set of supported OS variants.
| vm_memory | Memory required by the VM.|
| vm_cpu | Number of vCPUs required by the VM|
| vm_root_disk_size | Total size of the block device|
| vm_static_ip | Static IP to be assigned to the VM. (optional)|
| vm_netmask | Subnet mask to be used with the static IP. Must be set if static IP is being used|
| vm_gateway | The default gateway that the VM will use to access external networks. Must be set if static IP is used|
| vm_dns_search_domain | DNS search domain to be used in ethernet script of the VM |
| vm_dns1 | IP address of the first DNS server. **vm_dns2** and **vm_dns3** can be used to hold IP address of secondary and tertiary DNS servers.|
| vm_nm_controlled_network: 'yes'| Can have values '**yes**' or '**no**'. 'Yes' is used compulsorily for RHEL8 family as they only use the NetworkManager service. For RHEL7 family you can set this to 'no'|
| vm_additional_nics | Defines additional network interface required by the VM. This is defined as a list where each element is the name of the libvirt network to use. There should not be more than 3 elements in this list.|
| vm_additional_disks | A list that defines additional block devices. Each list element indicates the capacity if the block device |

### Other role variables
|Variable|Comments  |
|--|--|
| vm_recreate | Defaults to a value of **False** for safety reasons so that VMs are not recreated if they are already existing. This causes your role to stall with a message. Can be set to **True** to delete the VM and recreate it again|
| cloud_images_dir | Defaults to the name **cloud_images**. Will be created in the same place as the playbook that calls the role, if it doesn't exist. If this folder is already existing then the role tries to rsync the folder content to the **/var/lib/libvirt/images** path |
| cloud_storage_path | Path where VM boot disk and other block device based resources are stored. |
| cloud_network | Libvirt network to be used by cloud images. This defaults to the default libvirt network called **default**. |
| cloud_image_download_timeout| Max time to download cloud image. Defaults to 300 seconds. Increase this value for slower internet connections. |
| cloud_image_download_poll | Polls for task completion every 10 seconds by default. |


#### Example. 


## Dependencies
No role dependencies exist. However, you are encouraged to use the **libvirt_setup** role to work effectively with the **cloud_init** role.

## License
BSD

## Author
**Sanjay Singh**
sanjay.kr.singh@gmail.com
> Written with [StackEdit](https://stackedit.io/).

