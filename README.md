Ansible Role - aap_requirements_check
====================================

This role was designed to run rhdg (operator and cluster) deploy/destroy tasks against OCP. 

Limitations
-----------

 * Does not support Ansible Check Mode (Dry Run).

Requirements
------------

* Ansible: ansible 6.3 or higher
* Python: 3.9 or later 
* pip (kubernetes,requests-oauthlib and requests)
* OCP Cluster Administrator perms to deploy the RHDG operator

Role Variables
--------------

| Name                                           | Default value                                           |                                                               |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------------|
| ocp_url                                        | MANDATORY                                               | Specify OCP cluster url                                       |
| ocp_admin_username                             | MANDATORY                                               | Specify OCP username                                          |
| ocp_admin_password                             | MANDATORY                                               | Specify OCP password                                          |
| validate_certs                                 | true                                                    | Specify if OCP has a valid certificate                        |
| operator_namespace                             | openshift-operators                                     | Specify the default operator namespace on the cluster         |
| install_plan_approval                          | Automatic                                               | Specify the install plan to use                               |
| rhdg_operator_state                            | present                                                 | Specify if rhdg operator is present or absent                 |
| rhdg_operator_cluster_service_version_running  | MANDATORY                                               | Mandatory when removing operator                              |
| rhdg_cluster_state                             | present                                                 | Specify if rhdg cluster is present or absent                  |
| env                                            | lab                                                     | Specify environment to deploy rhdg cluster                    |
| cluster_namespace                              | rhdg-{{ env }}                                          | Specify the namespace to deploy rhdg cluster                  |
| cluster_name                                   | rhdg-{{ env }}                                          | Specify cluster name                                          |
| extra_jvm_opts                                 | -Xlog:gc*=info:file=/tmp/gc.log:time,level,tags,uptimemillis:filecount=10,filesize=1m -XX:-UseParallelOldGC -XX:+UseG1GC -XX:MaxGCPauseMillis=400"                                                                                  | Define jvm opts                                               |

Example Playbook
----------------

playbook.yml

```yaml
cat << eof > playbook.yml
---
- name: RHDG Cluster operator tasks
  hosts: localhost
  connection: local
  gather_facts: false
  roles:
    - rhdg_cluster

eof
```

Example Secret File
-------------------

secrets.yml

Run the command below to create a Ansible vault file:

```command
$ ansible-vault create secrets.yml
```
Create the variables, for instance:

```command
ocp_admin_username: user-admin
ocp_admin_password: user-admin-password
```

Example Extra Vars File
-----------------------

extraVars.yml

```command
cat << eof > extraVars.yml
ocp_url: https://api.cluster.localdomain:6443
eof
```

Example Ansible Playbook Command:
---------------------------------

```command
ansible-playbook -e @./extraVars.yml -e @secrets.yml --ask-vault-password playbook.yml
```

[![asciicast](https://asciinema.org/a/dmhZ2msxMD0ZyVpzjLJtpSoO8.svg)](https://asciinema.org/a/dmhZ2msxMD0ZyVpzjLJtpSoO8)

License
-------

BSD

Author Information
------------------

paulo@redhat.com
