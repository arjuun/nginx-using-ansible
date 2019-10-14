# nginx-using-ansible
running an nginx with virtual host using ansible

> running nginx using ansible in Ubuntu

*This repo has a playbook used for running nginx in ubuntu versions*

```
---
- name : "nginx installation in Ubuntu"
  hosts: all
  become: true
  
## enter your domain name to create configuration file, root directory with the domain name
  vars_prompt:
    - name: domain
      prompt: "Enter domain name"
      private: no
  
  tasks:
## nginx installation
    - name: "Installing nginx"
      apt:
        name: nginx
        state: present
        update_cache: yes
        
## removing default virtual host to avoid duplicating  "port 80" with the virtual host of our domain
    - name: "Remove default site"
      file:
        path: /etc/nginx/site-enabled/default
        state: absent

## creating a root directory with the domain name
    - name: "Create root directory"
      file:
        path: "/var/www/{{ domain }}"
        state: directory
        mode: '0755'

## creating an index.html file to test and give appropriate permissions
    - name: "Content in index.html"
      copy:
        content: <h1> test file </h1>
        dest: "/var/www/{{ domain }}/index.html"
        owner: www-data
        group: www-data

## creating virtual host configuration file with the domain name
    - name: "Virtual hosts"
      template:
        src: devopsarju.ml.conf.tml
        dest: "/etc/nginx/conf.d/{{ domain }}.conf"

## restaring nginx
    - name: "restart"
      service:
        name: nginx
        state: reloaded
```
