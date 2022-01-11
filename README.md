# PMM Server

An Ansible role that installs, configures and manages PMM-Server 2 for EL 8.

Firewalld
---------

As firewalld and Docker don't mix, firewalld is stopped by default. Enabling firewalld might break networking and other parts.

SSL
---

Self-signed certificates are automatically created:

```yaml
pmm_certs_dir: /etc/pmm-certs/
```

PMM-server is reachable from port 80 and 443.

If desired, place your own certificates at that location, named `server.key` and `server.crt`.

Disk
----

[Please consider the following](https://www.percona.com/doc/percona-monitoring-and-management/2.x/install/docker.html):

Metrics collection consumes disk space. PMM needs approximately 1GB of storage for each monitored database node with data retention set to one week. (By default, data retention is 30 days.) To reduce the size of the Prometheus database, you can consider disabling table statistics.

Although the minimum amount of memory is 2GB for one monitored database node, memory usage does not grow in proportion to the number of nodes. For example, 16GB is adequate for 20 nodes.

Latest
------

Using the [latest tag](https://forums.percona.com/discussion/56223/trying-to-install-v2-but-latest-docker-image-is-always-v1#latest) doesn't work. Tag #2 works, which actually refers to the latest v2.

Credentials
-----------

The username is always admin. The password must be changed:

```yaml
pmm_server_pass: let_me_in
```

The credentials are set [via a docker command](https://forums.percona.com/discussion/56228/set-username-and-password-of-pmm-server-via-docker-env#latest), instead of specifying env's.

By default, Docker can only be run by root. Specify users which can also execute docker with:

```yaml
pmm_docker_users:
  - my_user
  - another_user
```

Example playbook
----------------

```yaml
---
- hosts: my_host
  become: true
  vars:
    pmm_server_pass: something
    pmm_docker_users:
      - my-user
  roles:
    - ansible-pmm-server
```
