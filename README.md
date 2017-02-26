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
```
freebsd_jail_template_default_pkg:
-"python"
```
The default packages that should always be part of the template. For example, curl or sudo. Python is chosen here because it's required by Ansible.
Custom packages can be provided with:

```
freebsd_jail_template_pkg: []
```
The **freebsd\_jail\_template\_default\_pkg** and **freebsd\_jail\_template\_pkg** are combined and installed in one go into the template.

```
freebsd_jails_template_if:          "em0"
freebsd_jails_template_ip:          "10.0.1.30"
```

**freebsd\_jails\_template\_if** sets the interface used for installing packages into the template (so it's only used for install)  
**freebsd\_jails\_template\_ip** select an available IP that can download packages.

```
freebsd_jails_template_pkgs_prop:   "org:freebsd:packages"
```
This variable allows storing the installed packages and their versions as a ZFS property. 

```
freebsd_jails_location:             "/usr/local/jails"
freebsd_jails_dataset:              "zroot/jails"
freebsd_jails_template_dataset:     "zroot/jails/templates"
freebsd_jails_template_mount:       "{{ freebsd_jails_location }}/templates"
freebsd_jails_template_download:    "/tmp/jails-template-releases"
freebsd_downloadsite:               "ftp://ftp.freebsd.org/pub/FreeBSD/releases/amd64/"
```

**freebsd\_jails\_location** where to mount the jails onto the host filesystem.  
**freebsd\_jails\_dataset** which ZFS dataset to place the jails under.  
**freebsd\_jails\_template\_dataset** which ZFS dataset to store the resulting templates under.  
**freebsd\_jails\_template\_mount** where to mount the templates under the host filesystem.  
**freebsd\_jails\_template\_download** where to store the FreeBSD txz files on the host.  
**freebsd\_downloadsite** where to grab the FreeBSD txz files from (defaults to the project)  

```
freebsd_jails_default_install:
- file: "base.txz"
  creates: "bin/"
- file: "lib32.txz"
  creates: "libexec/ld-elf32.so.1"
```

**freebsd\_jails\_default\_install** determines which bits to download for the FreeBSD base install; The creates folder specifies a file or folder that will be present after the initial unpack so that we don't repeat the process.

```
freebsd_jails_minimal_conf:
- "etc/resolv.conf"
- "etc/localtime"
```
**freebsd\_jails\_minimal\_conf** specifies which files to include into the template. These are copied from the host.  

```
freebsd_jails_release:              "11.0-RELEASE"
freebsd_jails_base_template_name:   "base110"
freebsd_jails_template_version:     "{{ ansible_date_time.iso8601_micro }}"
```
**freebsd\_jails\_release** pretty obvious; Don't use a newer version than the host.  
**freebsd\_jails\_base\_template\_name** the name of the template. Needs to be unique.  
**freebsd\_jails\_template\_version** whenever anything changes a new snapshot will be made according to what is in this variable. The default uses an iso8601 timestamp.  

**freebsd\_jails\_template\_user** set the username of your ssh user.  
**freebsd\_jails\_template\_user\_keys** provide a github like url that contains the public SSH keys to be added in _authorized\_keys_ for the support user.  
**freebsd\_jails\_template\_user\_sudo** determine if sudo should be installed into the jail and the wheel group added by default (_TODO: make this more flexible_)

```
freebsd_jails_base_template_noupdate: no
```

Setting **freebsd\_jails\_base\_template\_noupdate** to yes basically skips the role if the template already exists; This allows iterating quicker over development.

Dependencies
------------

None

Example Playbook
----------------

This example creates a few different templates. Only the software is installed, further configuration should be done by other playbooks.  

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
