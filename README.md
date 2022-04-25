# LVM and Mounting

This role will create an LVM from a specified physical disk and and mount it to a specific disk.

This role was originally designed for longhorn but its approach is one of the most reinvented flow in VM's. The Default's values are set for setting up an ext4 LVM and attach it to the default disk space for the longhorn application deployed via Helm v3.
## Requirements

* Ansible 2.10+

## Tested on

* Rocky Linux 8
* Ubuntu 20.04 LTS

## Role Variables

This is a copy of `defaults/main.yml`

```yaml
---

lvm_device_id: /dev/sdc

partition_id: 1

virtual_group: "longhorn-vg"

logical_volume: "longhorn-lv"

lv_size: "100%FREE"

fs_type: "ext4"

mount: true

mount_dir: "/var/lib/longhorn"

```

## Inventory file example


```ini
[all:children]
bastion
masters
workers

[bastion]
bastion

[masters]
master-01 
master-02 
master-03 

[workers]
worker-01 
worker-02 
worker-03 

[k8s_cluster:children]
masters
workers

[all:vars]
ansible_ssh_common_args="-o ProxyCommand='ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -W %h:%p USER@127.0.0.1'"
```

## Playbook example



```yaml
- name: PreSetup Longhorn
  hosts: k8s_cluster
  become: yes
  roles:
     - role: pre-longhorn

```


## License

MIT

## Author Information

Created in 2022 by Seferon
