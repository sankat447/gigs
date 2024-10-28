To test an Azure connection with Ansible, you can use the `azure.azcollection.azure_rm_resource_info` module to verify the connection. Before running this playbook, ensure that:

1. You have the necessary credentials configured in your Azure account, either through an environment variable or Azure CLI login.
2. The `azure.azcollection` Ansible collection is installed. You can install it by running:
   ```bash
   ansible-galaxy collection install azure.azcollection
   ```

Here’s a simple playbook to test the connection to your Azure subscription:

```yaml
---
- name: Test Azure Connection
  hosts: localhost
  gather_facts: no
  vars:
    resource_group_name: "<your_resource_group_name>"  # Update with a real resource group name
    azure_subscription_id: "<your_subscription_id>"    # Update with your subscription ID

  tasks:
    - name: Retrieve resource group info
      azure.azcollection.azure_rm_resource_info:
        api_version: "2021-04-01"
        resource_group: "{{ resource_group_name }}"
        resource_type: "resourceGroups"
        subscription_id: "{{ azure_subscription_id }}"
      register: result

    - name: Display the result
      debug:
        var: result
```

### Explanation:

- **azure.azcollection.azure_rm_resource_info**: This module fetches details about Azure resources, and in this example, it checks if a specific resource group exists.
- **resource_group_name** and **azure_subscription_id**: Set these variables to your actual Azure resource group name and subscription ID to test connectivity.
- **debug**: The debug task outputs the result, which will help you confirm whether the connection to Azure is successful.

### Running the Playbook

Run this playbook with:
```bash
ansible-playbook test_azure_connection.yml
```

If the connection is successful, you’ll see the details of the resource group in the output. If there are connection issues, Ansible will provide error details for troubleshooting.
