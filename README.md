pllr.kibana-install
=========

Install kibana.

Requirements
------------

None.

Role Variables
--------------

None.

Dependencies
------------

None.

Example Playbook
----------------

```yaml
---
- name: "Install and configure Elasticsearch"
  hosts: elasticsearch-master
  roles:
  - { role: pllr.elasticsearch }
```

License
-------

Apache-2

Author Information
------------------

Check me on [LinkedIn](www.linkedin.com/in/phil-ranzato-47b8bb194)

Tasks Analysis
------------------

`01-install.yml`

```yaml
---
# rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
- name: "Import elasticsearch GPG key"
  rpm_key:
    state: present
    key: https://artifacts.elastic.co/GPG-KEY-elasticsearch

# cat << EOF > /etc/yum.repos.d/kibana.repo
# [kibana-7.x]
# name=Kibana repository for 7.x packages
# baseurl=https://artifacts.elastic.co/packages/7.x/yum
# gpgcheck=1
# gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
# enabled=1
# autorefresh=1
# type=rpm-md
# EOF
- name: "Configure Kibana repository"
  copy:
    src: "kibana.repo"
    dest: "/etc/yum.repos.d/kibana.repo"

# yum install -y --enablerepo=kibana kibana 
- name: "Install Kibana"
  yum:
    name: kibana
    enable_repo: kibana
    state: latest
  # systemctl daemon-reload
  # systemctl enable kibana
  # systemctl start kibana
  notify: "Start Kibana"
```
