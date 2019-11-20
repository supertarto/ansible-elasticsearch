# Absible Elasticsearch
[![Build Status](https://travis-ci.org/supertarto/ansible-elasticsearch.svg?branch=master)](https://travis-ci.org/supertarto/ansible-elasticsearch)

Install and configure Elasticsearch with java.
This role is meant for my small need. It's inspired in part by the official role. If you need a more complete one, you can find it here:
https://github.com/elastic/ansible-elasticsearch


## Requirements
You need a recent version of Java. You can use my supertarto.openjdk role.

## Tested plateform
* Debian 9 (Stretch)
* Debian 10 (Buster)

## Role variables
The elasticsearch package version
```yml
elasticsearch_version: "7.4.1"
```
For now, it's only possible to install elasticsearch on Debian based distribution. It's also only possible ton install it via repository.
The **elasticsearch_major_version** value is automatically set in a task, and is based on **elasticsearch_version**
```yml
elasticsearch_repo_base: "https://artifacts.elastic.co"
elasticsearch_apt_key: "{{ elasticsearch_repo_base }}/GPG-KEY-elasticsearch"
elasticsearch_apt_key_id: "46095ACC8548582C1A2699A9D27D666CD88E42B4"
elasticsearch_apt_url: "deb {{ elasticsearch_repo_base }}/packages/{{ elasticsearch_major_version }}/apt stable main"
elasticsearch_package_name: "elasticsearch"
```
Path used by Elasticsearch. The **elasticsearch_snapshot_path** is optional. You can set it to **""** if you don't want to create snapshots on a daily basis in a cron task.
```yml
elasticsearch_conf_dir: "/etc/elasticsearch"
elasticsearch_pid_dir: "/var/run/elasticsearch"
elasticsearch_log_dir: "/var/log/elasticsearch"
elasticsearch_data_dirs:
  - /var/lib/elasticsearch
elasticsearch_snapshot_path: /var/local/elasticsearch
elasticsearch_default_file: "/etc/default/elasticsearch"
elasticsearch_home: "/usr/share/elasticsearch"
```
Lock the elasticsearch version. Its use, on debian, to mark the elasticsearch package on "hold" and to avoid unwanted upgrade.
```yml
elasticsearch_version_lock: false
```
The elasticsearch system user and group
```yml
elasticsearch_user: elasticsearch
elasticsearch_group: elasticsearch
```
Allow the elasticsearch service to be (re)started
```yml
elasticsearch_start_service: true
```
The elasticsearch host and port
```yml
elasticsearch_api_host: localhost
elasticsearch_api_port: 9200
```
The elasticsearch snapshot name. this value is optional, and can be set to **""**. But, if a value is set, **elasticsearch_snapshot_path** must be set to.
```yml
elasticsearch_snapshot_name: backup_es
```
Java heap size. Not defined by default and automatically set to 2g. Can be changed here.
```yml
elasticsearch_heap_size_xms: ""
elasticsearch_heap_size_xmx: ""
```
Default values of some JVM options. Can be changed.
```yml
elasticsearch_max_open_files: 65536
elasticsearch_max_map_count: 262144
elasticsearch_max_threads: 8192
```
Variables used in **elastisearch.yml**. By default, the cluster name is **elasticsearch** and the node name is **default_node**. Chane thoses to your liking.
```yml
elasticsearch_config_cluster_name: "elasticsearch"
elasticsearch_config_node_name: "default_node"
elasticsearch_discovery_seed_hosts: '["127.0.0.1"]'
elasticsearch_cluster_initial_master_nodes: '["master_node"]'
```
## Examples
```yml
---
- hosts: somehost
  roles:
    - supertarto.elasticsearch
  vars:
    elasticsearch_config_cluster_name: supercluster
    elasticsearch_config_node_name: supernode
    elasticsearch_heap_size_xms: "4g"
    elasticsearch_heap_size_xmx: "4g" 
```
## Installation
```
ansible-galaxy install supertarto.elasticsearch
```
## License
GPL V3.0
