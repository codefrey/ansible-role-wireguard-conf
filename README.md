# ansible-role-wireguard-conf
Ansible role to create wg and/or wg-quick compatible configuration
files. This role only creates the configuration files and does neither
install WireGuard nor activate connections.

## Variables

### wireguard_config_directory
Directory where the WireGuard config files will be created in.

If this directory does not exist, it will be created.

Defaults to `/etc/wireguard`.

### wireguard_interfaces
Defines the interfaces for which configuration files should be
created (state: present) / deleted (state: absent).

Example:

```
wireguard_interfaces:
    - name: wg0
      state: present
      address: "10.0.0.1/24"
      private_key: "hUKC2Yb07ovbQb+ijQp1VgGAcj60hDaqIakRe4S7VKE="
      listen_port: 51820
      post_up: "echo 1 > /proc/sys/net/ipv4/ip_forward"
      post_down: "echo 0 > /proc/sys/net/ipv4/ip_forward"
      mtu: 1348
      peers:
        - name: laptop
          public_key: "kx+CiZJFhoE+AUXF1ZTMDdYFXpChl3nJw9zDjScPd3I="
          preshared_key: "YnlMQ3MrIIKlZdCXW7SLPpoyzph+SKb3UTjfWW9IHLc="
          allowed_ips: "10.0.0.2/32"
```

would create a file `wg0.conf` in `wireguard_config_directory` with
one peer.

All configuration keys found in `man wg` and `man wg-quick` should be
supported, but note that they are in snake_case instead of CamelCase.

If `wireguard_interfaces` is not defined, no configuration files are
changed.
