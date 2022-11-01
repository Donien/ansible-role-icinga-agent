Role Name
=========

This Ansible role aims to install Icinga 2 on agent machines, automatically connect them to their parent(s), and build trust.

Requirements
------------

Currently only Ansible's builtin modules are needed.

Role Variables
--------------

Variables that you **must** define:

- `parent_zone`: Your parent's zone's name
- `parents`: Have a look into the **example playbook** below

Variables you **should** define:

- `ca_node`: The Ansible inventory hostname of your icinga CA (for creating tickets)

Variables that you might wanna change:  

- `force_setup`: Whether the `icinga2 node setup` should be forced (default: `false`)
- `install_check_plugins`: Whether or not to also install monitoring-plugins / nagios-plugins (default: `true`)
- `global_zones`: A list of additional global zones to deploy onto the agent.
- `icinga2_port`: The port icinga2 should use (default: `5665`)
- `accept_config`: Whether or not the agent should accept configuration from its parent (default: `true`)
- `accept_commands`: Whether or not the agent should accept commands from its parent (default: `true`)
- `disable_confd`: Whether or not to disable the inclusion of the `/etc/icinga2/conf.d` directory (default: `true`)

Dependencies
------------

**None yet**

Example Playbook
----------------

```yml
- name: Install Icinga 2 Agent
  vars:
    parent_zone: "master"
    ca_node:
      name: icinga-parent
    parents:
      - name: icinga-parent
        #fqdn: "icinga"
        #ip: "192.168.122.222"
        #port: 4269

  hosts:
    - icinga-parent
    - ansible-vm-ubuntu
    - ansible-vm-debian

  roles:
    - icinga-agent
```

The `parents` dictionary's entries need to at least have a `name` defined. This must be Ansibles inventory hostname for that node.  
This role will fetch that host's **fqdn** and **ip address** automatically if it is not provided.  
