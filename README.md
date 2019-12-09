gudron.wireguard_server
=======================

Ansible role for install wireguad vpn server and create config files. 

Instalation
-----------

Add **gudron.shells_preparer** role to your *requirements* file.

```yaml
  - src: git@github.com:gudron/gudron.wireguard_server.git
    scm: git
    version: master
```

Install roles via **ansible-galaxy** tool.

```bash
ansible-galaxy install -p roles -r requirements.yml
```

Example Playbook
----------------

    - hosts: example_vpn_server
      any_errors_fatal: "{{ any_errors_fatal | default(true) }}"
      gather_facts: yes

      roles:
        - name: gudron.wireguard_server
          vars: 
            boot_via: systemd
            interfaces_name: wg0
            server_params: 
              address: 172.16.0.1
              mask: 255.255.255.0
              port: 51820
              private_key: /example/project/wireguard/secrets/server/private.key
              public_key: /example/project/wireguard/secrets/server/public.key
              preshared_key: /example/project/wireguard/secrets/server/preshared.key
            peer_params: 
              - public_key: /example/project/wireguard/secrets/example_client_1/public.key
                preshared_key: /example/project/wireguard/secrets/example_client_1/preshared.key
                allowed_ips:
                  - address: 172.16.0.3
                    mask: 255.255.255.0

              - public_key: /example/project/wireguard/secrets/example_client_2/public.key
                preshared_key: /example/project/wireguard/secrets/example_client_2/preshared.key
                allowed_ips:
                  - address: 172.16.0.4
                    mask: 255.255.255.0

License
-------

Apache 2.0