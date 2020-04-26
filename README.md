![](https://img.shields.io/badge/Ansible-alerta-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_alerta.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [alerta Versions](#alerta-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
This Ansible role installs Alerta on linux operating system, including establishing a filesystem structure and server configuration with some common operational features.

## Requirements
### Operating systems
This role will work on the following operating systems:

  * CentOS 7

### alerta versions

The following list of supported the alerta releases:

* alerta 7.5+

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:
##### General parameters
* `alerta_admin_user`: Alerta Superuser name.
* `alerta_admin_pass`: Alerta Superuser password.
* `alerta_dist_path`: Specify the Alerta web ui folder.
* `alerta_cluster`: Cluster name of servers that implements distribution performance.

##### Role dependencies
* `alerta_mongod_dept`: A boolean value, whether MongoDB database use the same environment.

##### Listen port
* `alerta_port_arg.api`: The port number of API.
* `alerta_port_arg.webui`: The port number of Web UI.

##### MongoDB parameters
* `alerta_mongod_auth`: A boolean value, Enable or Disable MongoDB authentication.
* `alerta_mongod_hosts`: Group of MongoDB hosts Graylog should connect to.
* `alerta_mongod_node_role`: Member role for ReplicaSet.
* `alerta_mongod_path`: Specify the MongoDB data directory.
* `alerta_mongod_port`: The MongoDB instance port.
* `alerta_mongod_replset`: MongoDB ReplicaSet name.
* `alerta_mongod_sa_pass`: MongoDB Superuser password.
* `alerta_mongod_sa_user`: MongoDB Superuser name.
* `alerta_mongod_user`: MongoDB Graylog username.
* `alerta_mongod_pass`: MongoDB Graylog password.
* `alerta_mongod_version`: Specify the MongoDB version.
* `alerta_mongod_backupset_arg`: Defines backup parameters.

##### System Variables
* `alerta_arg.user`: System user name for running Alerta services.
* `alerta_arg.auth_provider`: Defines authentication providers.
* `alerta_arg.base_url`: API served on a path or behind a proxy use it to fix relative links.
* `alerta_arg.use_proxyfix`: API served behind SSL terminating proxy.
* `alerta_arg.signup_enabled`: Whether prevent sign-up of new users via the web UI.
* `site_logo_url`: URL of company logo to replace "alerta" in navigation bar.
* `alerta_arg.log_max_mib`: Maximum mebibyte size of log file before rollover.
* `alerta_arg.log_backup_count`: Number of rollover files before older files are deleted.
* `alerta_arg.log_format`: Defines log file formatter.

##### Plugins Variables
* `alerta_plugins`: Define additional plugins to install.

##### Service Mesh
* `environments`: Define the service environment.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: false Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
- Ansible versions >= 2.8 are supported.
- Python >= 2.7.5 
- [MongoDB](https://github.com/goldstrike77/ansible-role-linux-mongodb.git) 

## Example

### Hosts inventory file
See tests/inventory for an example.

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: ansible-role-linux-alerta

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

    alerta_admin_user: 'admin'
    alerta_admin_pass: 'changeme'
    alerta_dist_path: '/data'
    alerta_cluster: 'alerta'
    alerta_mongod_dept: true
    alerta_port_arg:
      api: '19199'
      webui: '19198'
    alerta_mongod_auth: true
    alerta_mongod_hosts: 'localhost'
    alerta_mongod_node_role: 'replica'
    alerta_mongod_path: '{{ alerta_dist_path }}'
    alerta_mongod_port: '27017'
    alerta_mongod_replset: '{{ alerta_cluster }}'
    alerta_mongod_sa_pass: 'changeme'
    alerta_mongod_sa_user: 'sa'
    alerta_mongod_user: 'alerta'
    alerta_mongod_pass: 'changeme'
    alerta_mongod_version: '36'
    alerta_mongod_backupset_arg:
      keep: '7'
      encryptkey: 'wEX6nE3eHAJp'
      cloud_rsync: true
      cloud_drive: 'azureblob'
      cloud_bwlimit: '10M'
      cloud_event: 'sync'
      cloud_config:
        account: 'blobuser'
        key: 'base64encodedkey=='
        endpoint: 'blob.core.chinacloudapi.cn'
    alerta_arg:
      user: 'alerta'
      auth_provider: 'basic'
      base_url: ''
      use_proxyfix: 'False'
      signup_enabled: 'False'
      site_logo_url: ''
      log_max_mib: '25'
      log_backup_count: '10'
      log_format: 'verbose'
    alerta_plugins:
      prometheus:
        - alertmanager_api_url: 'http://MONITOR-Production-alertmanager.service.dc01.local:9093'
          alertmanager_silence_days: '2'
    environments: 'Development'
    tags:
      subscription: 'default'
      owner: 'nobody'
      department: 'Infrastructure'
      organization: 'The Company'
      region: 'IDC01'
    exporter_is_install: false
    consul_public_register: false
    consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
    consul_public_http_prot: 'https'
    consul_public_http_port: '8500'
    consul_public_clients:
      - '127.0.0.1'

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
