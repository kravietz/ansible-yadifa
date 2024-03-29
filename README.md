Role Name
=========

Install and configure Yadifa, a DNSSEC-enabled authoritative DNS nameserver, including zone configuration.

Role Variables
--------------

```
# Reference: https://cdn.yadifa.eu/sites/default/files/YRM239.pdf

yadifa_hostname: "{{ ansible_hostname }}"
yadifa_nsid: "{{ ansible_hostname }}"
yadifa_log_queries: no
yadifa_daemon: no
yadifa_statistics: yes

# Show DS records for all domains at the end of play run
yadifa_show_ds: no

yadifa_data_dir: /var/lib/yadifa
yadifa_masters_dir: "{{ yadifa_data_dir }}/masters"

yadifa_listen:
- "0.0.0.0"
- "::"

yadifa_zones: []
# - type: master
#   domain: webcookies.eu
#   file: dns/master/webcookies.eu.zone
#   allow_transfer: cloudns
#   allow_update: none
#   allow_update_fw: none
#   dnssec_policy: normal-policy

yadifa_dnssec_policies:
- id: normal-policy
  description: Default DNSSEC policy with ZSK and KSK
  denial: nsec3-random
  key_suites:
  - zsk-1024
  - ksk-2048

yadifa_keys: []
# Generate T-SIG keys with `openssl rand -base64 32`
# - name: controller-key
#   algorithm: hmac-sha256
#   secret: LYHjUHOIAvHXnHa3kUPf7w==

# Configuration for yadifa *client* command-line utility
# These must match `yadifa_keys` entry used in `yadifa_acls` for controller
# yadifa_cli_server: "::1"
# yadifa_cli_algo: hmac-sha256
# yadifa_cli_secret: LYHjUHOIAvHXnHa3kUPf7w==

yadifa_acls: []
# - name: controller
#   value: key controller-key
# - name: cloudns
#   value: >
#     2a00:1768:1001:9::31:1, 2605:fe80:2100:a013:7::1,
#     2a0b:1640:1:1:1:1:8ec:5a47, 2a06:fb00:1::1:66,
#     2a06:fb00:1::2:66, 2a06:fb00:1::3:66, 2a06:fb00:1::4:66,
#     2a0b:1640:1:3::1, 2001:41d0:305:2100::32b4

# names of the ACLs to allow specific features from
# these *must* match the ACL names defined in `yadifa_acls`
# or be one of `none` or `any`
yadifa_global_allow_control: none
yadifa_global_allow_query: any
yadifa_global_allow_update: none
yadifa_global_allow_transfer: none
yadifa_global_allow_notify: none

# yadifa_nsec3_salt: BA5EBA11
yadifa_nsec3_optout: yes
yadifa_nsec3_salt_len: 32
yadifa_nsec3_salt_iter: 10
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
- hosts: servers
  roles:
     - yadifa
```

Issues
------

Report issues [bitbucket.org/kravietz/ansible-yadifa/issues](https://bitbucket.org/kravietz/ansible-yadifa/issues?status=new&status=open)

License
-------

GPL-3

Author Information
------------------

-	Pawel Krawczyk
