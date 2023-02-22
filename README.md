![Ansible Lint](https://github.com/johanneskastl/ansible-role-install_and_configure_knotresolver/workflows/Ansible%20Lint/badge.svg)

install_and_configure_knotresolver
=========

Install and configure a knot-resolver

Requirements
------------

None.

Role Variables
--------------

By default, this role sets up a knot-resolver with regular (unencrypted) DNS and DNS-over-TLS  enabled for both IPv4 and IPv6. DNS-over-HTTPS is disabled for both IPv4 and IPv6.

## DNS resolution types

You can enable or disable all of these with the following Boolean variables:

- `knotresolver_enable_dns_ipv4`: (Boolean) Whether to enable (unencrypted) DNS for IPv4 (Default value: `true`)
- `knotresolver_enable_tls_ipv4`: (Boolean) Whether to enable DNS-over-TLS for IPv4 (Default value: `true`)
- `knotresolver_enable_doh2_ipv4`: (Boolean) Whether to enable DNS-over-HTTPS for IPv4 (Default value: `false`)
- `knotresolver_enable_dns_ipv6`: (Boolean) Whether to enable (unencrypted) DNS for IPv6 (Default value: `true`)
- `knotresolver_enable_tls_ipv6`: (Boolean) Whether to enable DNS-over-TLS for IPv6 (Default value: `true`)
- `knotresolver_enable_doh2_ipv6`: (Boolean) Whether to enable DNS-over-HTTPS for IPv6 (Default value: `false`)

## Allowing queries from other machines

Also by default, this role configures knotresolver so it only listens on the loopback interface, i.e. it is not reachable from other machines.
In case you want to change that, set the following variable to `false`:

- `knotresolver_loopback_only`: (Boolean) Whether to only listen on the loopback interface or on externally reachable interfaces. Defaults to `true` aka "loopback only".

If you do so, this role will automatically use the machine's primary IPv4 address to listen on. (Do not set `knotresolver_ipv4_interface` manually, as that will get overwritten by the role).

In case you enabled any of the IPv6 DNS types, then it will also listen on the machine's primary IPv6 address. (Again, do not set `knotresolver_ipv6_interface`, as that will get overwritten by the role).

## TLS settings

In case you want to use DNS-over-TLS or DNS-over-HTTPS, you need to configure a valid TLS certificate using the following variables:

- `knotresolver_tls_crt_file_path`: (String) Path to the machine's certificate's crt file
- `knotresolver_tls_key_file_path`: (String) Path to the machine's certificate's private key file

## Forwarding of internal domains

In case you want to forward requests for one (or more) internal domains to your internal authoriative DNS servers, you can do so.

- `internal_domains`: (List of strings) List of internal domains
- `internal_forwarders`: (List of strings) List of IPs for your internal authoriative DNS servers
- `internal_forwarding_flags`: (List of strings) In case you require special DNS flags (like `NO_CACHE` or `NO_EDNS` or similar), you can set them in this variable.
- `validate_dnssec_on_internal_domains`: (Boolean) By default, DNSSEC validation is enabled also for internal domains. Set this to `false` to disable it.

## Miscellaneous

- `knotresolver_cache_size_megabytes`: (Integer) By default, the cache size is set to `100` MB. If you want to change this, this variable is for you.

Dependencies
------------

None

Example Playbook for a simple validating resolver with (unencrypted) DNS and DNS-over-TLS
----------------

    - hosts: servers
      roles:
        - role: 'johanneskastl.install_and_configure_knotresolver'

Example Playbook for a validating resolver with (unencrypted) DNS, DNS-over-TLS, DNS-over-HTPPS and an internal domain to be resolved internally
----------------

    - hosts: servers
      roles:
        - role: 'johanneskastl.install_and_configure_knotresolver'
          internal_domains:
            - example.com
            - example.org
          internal_forwarders:
            - 192.0.2.6
            - 192.0.2.7
            - 192.0.2.8
          validate_dnssec_on_internal_domains: false

License
-------

BSD-3-Clause

Author Information
------------------

I am Johannes Kastl, reachable via kastl@b1-systems.de.
