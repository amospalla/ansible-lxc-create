# ansible-lxc-create
* * *

## Description

lxc-create: creates lxc guests upon a set of defined variables.

Most of the tasks are delegated to _lxc_host_ host (this is, the host containing LXC guests).

First a helper script is copied to the host, which is executed to get the status of the containers (if these exists or are running).

If a public ssh key file is present on the files folder it is copied to the /root/.ssh/authorized_keys of the container.

Once containers are created and started raw commands are executed to install python or python-simplejson to allow Ansible run on them.

Finally a root password is set on the containers.

When _lxc_nameserver_ variable is defined /etc/resolv.conf is replaced with the single line "nameserver={{lxc_nameserver}}".

## Assumptions

LXC containers rootfs reside on /var/lib/lxc/<container>.

## Variables

host variables:

- _lxc_host_: fqdn of LXC host server.

guest variables:

- _lxc_ip_: guest network ip.
- _lxc_netmask_: guest network netmask.
- _lxc_gateway_: guest network gateway.
- _lxc_arch_: amd64 (debian/ubuntu), x86_64 (centos).
- _lxc_template_: ubuntu/debian/centos.
- _lxc_release_: (debian) wheezy/jessie, (ubuntu) lucid/precise/trusty/xenial, (centos) 5,6,7.
- _lxc_opts_: example "--mirror=http://apt-cacher-ng:3142/ftp.debian.org/debian".
- _lxc_container_config_: list of lxc directives, an example:
```
lxc_container_config:
  - "lxc.start.auto = 1"
  - "lxc.network.type = veth"
  - "lxc.network.flags = up"
  - "lxc.network.link = some-bridge-interface-name"
  - "lxc.network.name = eth0"
```
- _lxc_password_hash_: SHA512 password hash for root user (create with mkpasswd --method=SHA-512).

## Files

- files/_id_rsa.pub_: public key to insert on root's authorized_keys.
