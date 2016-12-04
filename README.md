bvansomeren.freebsd-jail-template
==================================

This role creates a template that can be used to clone from for creating FreeBSD jails.  
A base install is created inside a dataset, created as a temporary template jail.  
Packages can be installed into this template.
Default behaviour of the role is to only create a new snapshot if either the base install or pkgs see changes.

Requirements
------------

FreeBSD with ZFS

Role Variables
--------------

TODO: Check the defaults

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
   - hosts: all
     sudo: true
     roles:
     - role: bvansomeren.freebsd-jail-template
       freebsd_jails_base_template_name: base-11-0
       freebsd_jail_template_pkg:
       - "curl"
      - "wget"
      - "vim-lite"
      - "ncdu"
     - role: bvansomeren.freebsd-jail-template
       freebsd_jails_base_template_name: java8-11-0
       freebsd_jail_template_pkg:
       - "openjdk8"
     - role: bvansomeren.freebsd-jail-template
       freebsd_jails_base_template_name: gf41-11-0
       freebsd_jail_template_pkg:
       - "openjdk8"
       - "glassfish-4.1"
     - role: bvansomeren.freebsd-jail-template
       freebsd_jails_base_template_name: php70-11-0
       freebsd_jail_template_pkg:
       - "php70"
       - "php70-json"
       - "php70-openssl"
       - "php70-mysqli"
       - "php70-opcache" 

```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
