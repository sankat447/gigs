- name: Get Azure VM Disk Count
  hosts: localhost
  gather_facts: no
  tasks:
    - name: Get VM Info
      azure.azcollection.azure_rm_virtualmachine_info:
        resource_group: "myResourceGroup"
        name: "myVM"
      register: vm_info

    - name: Extract and display disk count
      debug:
        msg: "Number of disks attached: {{ vm_info.vms[0].storage_profile.data_disks | length }}"
