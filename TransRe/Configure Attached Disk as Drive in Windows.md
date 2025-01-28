- name: Configure Attached Disk as Drive in Windows
  hosts: windows
  gather_facts: no
  tasks:

    - name: Get disk information
      ansible.windows.win_disk_facts:
      register: disk_info

    - name: Identify the newly attached disk
      set_fact:
        new_disk_number: "{{ item.number }}"
      when: item.partition_style == 'raw'
      loop: "{{ disk_info.ansible_disks }}"

    - name: Initialize the disk
      ansible.windows.win_disk:
        disk_number: "{{ new_disk_number }}"
        partition_style: gpt
      when: new_disk_number is defined

    - name: Create a partition
      ansible.windows.win_partition:
        disk_number: "{{ new_disk_number }}"
        partition_size: 100%
      when: new_disk_number is defined
      register: partition_info

    - name: Format the partition as NTFS
      ansible.windows.win_format:
        drive_letter: "{{ partition_info.partition.drive_letter }}"
        file_system: NTFS
      when: partition_info.partition.drive_letter is defined

    - name: Assign drive letter (if not already assigned)
      ansible.windows.win_partition:
        disk_number: "{{ new_disk_number }}"
        partition_number: 1
        drive_letter: "E"
      when: new_disk_number is defined

