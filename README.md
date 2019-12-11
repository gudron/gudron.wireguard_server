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

Role Variables
--------------

### General variables
  * `boot_via: string`
    Startup wireguard via init system. Supported `systemd`, `interfaces`, `manual`
    
  * `interface_name: string` 
    The dictionary with wireguard network interfaces. Like `wg0`, `wg1`.

  * `server_params: dict` 
    Parameters of wireguard server.
    
    * `address: string` 
      Domain name or IP address of wireguard server.

    * `mask: string` 
      Peer IP-mask. In full-notation like `255.255.255.0`.

    * `port: string` 
      Wireguard server port

  * `peer_params: list` 
    Parameters of wireguard server.
    
    * `address: string` 
      Peer IP address

    * `public_key: string` 
      Public key path.

    * `preshared_key: string` 
      Public key path.

    * `allowed_ips: list`
      List of peers address parameters

      * `address: string`
        Peer IP-address

  Full example: [defaults/main.yml](defaults/main.yml).

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

              - public_key: /example/project/wireguard/secrets/example_client_2/public.key
                preshared_key: /example/project/wireguard/secrets/example_client_2/preshared.key
                allowed_ips:
                  - address: 172.16.0.4

License
-------

Apache 2.0