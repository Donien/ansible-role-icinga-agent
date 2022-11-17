icinga-agent
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

- `ca_node`: The Ansible inventory hostname of your icinga CA (for creating tickets and getting the trusted certificate).
- `trusted_crt`: The trusted certificate (on ca node: `icinga2 pki save-cert --host $(hostname -f) --trustedcert /tmp/trusted.crt`). If passed directly, `ca_node` does not need to be defined.

Variables that you might wanna change:  

- `force_setup`: Whether the `icinga2 node setup` should be forced (default: `false`)
- `install_check_plugins`: Whether or not to also install monitoring-plugins / nagios-plugins (default: `true`)
- `global_zones`: A list of additional global zones to deploy onto the agent.
- `icinga2_port`: The port icinga2 should use (default: `5665`)
- `accept_config`: Whether or not the agent should accept configuration from its parent (default: `true`)
- `accept_commands`: Whether or not the agent should accept commands from its parent (default: `true`)
- `disable_confd`: Whether or not to disable the inclusion of the `/etc/icinga2/conf.d` directory (default: `true`)
- `use_tickets`: Whether or not tickets for your agents should be created. Otherwise requests have to be signed manually. Requires `ca_node` to be defined (default: `false`)

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
    trusted_crt: |
        -----BEGIN CERTIFICATE-----
        MIIE1zCCAr+gAwIBAgIVALl0nT4cYQkMtrePtbG7Dn4V/cdTMA0GCSqGSIb3DQEB
        CwUAMBQxEjAQBgNVBAMMCUljaW5nYSBDQTAeFw0yMjA5MzAxODIzNDlaFw0yMzEx
        MDExODIzNDlaMBExDzANBgNVBAMMBmljaW5nYTCCAiIwDQYJKoZIhvcNAQEBBQAD
        ggIPADCCAgoCggIBAMXPc9GEqk3KhyJTlWAqsKn9oV/K1mLgi7Gg0c+wfrl3uw5u
        cFVFW4U8KurlGVadRwThuHM1p1S7OIvS+5876ywIOOekGOjAbkSV3zzM0K5zWvxZ
        ISxd3Hk9D/kZ1jN1y6IacnsesF5ol2EyyfCTPNG5TMxzxdwkHI3L4cyRciIv9AAp
        YwXprJUBpMz4lhbcp0dUxLZ11Wdy6Yqi+Eqis5tJXdoImkeENaUEyqz8q51CSNZF
        unzm2Sq1Kyos19oZpLoxpZBAgX4i3UFeImPbdm/egFHVS+60YlUUQds6Er9Iiut+
        CZ1WgOz5jj5OU0IkngvERFizW6CiqZSrokV2RVzx9qVgtWaZm3VFQy7b/7hwoDFC
        oPCGL1JoTPwa9P8c61sRyd4PjGoOVla+w+ZznSMc2ZLbWXXPFUS9TIen1U1+rhpL
        lcMA0rTAqJS0QvKpoefoR7pvEcQgvWpqnLiO0ewo2mPDXpWUlXulYuCLqKSSmoFh
        H1pMoqNXtLLxK5lDuboBxQ2JbaKtnRG6+aytBnSySTORWTPCteMDQ3GGLGTcNNGp
        fwWI8rpSVRnTR0h8AjtZB8ikyU1khejHZbphkaiVO6xHEobcv5gsW8G6ykz0hRbI
        6QnnwiqpVRwKXe70mxzFrVc1GgBAD1McgfgpsNuMe1qflrFnjZTn7+u4hgWbAgMB
        AAGjIzAhMAwGA1UdEwEB/wQCMAAwEQYDVR0RBAowCIIGaWNpbmdhMA0GCSqGSIb3
        DQEBCwUAA4ICAQCwtGxgiAO8cjNN/73h6pNKhCfiCtGY0yf7mzcCz7R9TFqu1igi
        GLT7NySC66Iu54hl/Jz+V15Ji7+o2/In5tzAYHnYWifwy3R6XGTJeZKjlAuoy9hM
        xxQTK6p3E0ooV6Wva7igsgSlS48TjoiwlnlC8Y9hBVkKdVrE0u48i7g+0OMIE/xC
        8CfCVE7As83TD5brIiH/65reHsDLrqLCzse4ZSh6jfgCACPoVvJBXaFnZW9pJziY
        GUR6/4OrLq7nh3zLakF2i8wmq3fuQDDQjbT9DNddkEe6pfWrH6SagPiGb4KRR/pk
        KM2itpWHgGHhs0FiwgDf9GVyUxlZP12KFWdR+RvmEtX4rmJeGrFOjJ9SSnRg8+Pd
        h//vhctX8mAJJ0BA6kyC7KyfU1PijM9CcSsku05YqSnEzKvoPPC6fBzpV3hfFTUz
        AatV/1A6LYV7zvANlug1bR+Fy+AFn2Ynk/sEQaIywl7HEse9ysRlPTpP7hXQFTV4
        pMEfBaZBHJmdEmRaFo7Ku2BikJZDSRvIaCg9uttXiEzia1Fr1BAPH3kCnpPaQB8P
        DDSyN/7Wm2zOacGuvRhcEXF0tWHcrKT1fToD1UZ53NKlC2guqTqPZDuAdh6X3OXb
        fJzclpxYRLgUraNg/euvuk/+G3336J4Zli26K2F8dgusmTmf4a+hTjxKyw==
        -----END CERTIFICATE-----

  hosts:
    - icinga-parent
    - ansible-vm-ubuntu
    - ansible-vm-debian

  roles:
    - icinga-agent
```

The `parents` dictionary's entries need to at least have a `name` defined. This must be Ansible's inventory hostname for that node.  
This role will fetch that host's **fqdn** and **ip address** automatically if it is not provided.  
