# Ansible YAML Playbook Guide

This repository provides a detailed guide to writing and executing Ansible playbooks using YAML. It includes explanations, examples, and practical use cases for automating system administration tasks.

---

## Table of Contents
1. [Introduction](#introduction)
2. [Understanding YAML in Ansible](#understanding-yaml-in-ansible)
3. [Common Playbook Examples](#common-playbook-examples)
    - [File Management](#file-management)
    - [User and Group Management](#user-and-group-management)
    - [Package Installation](#package-installation)
    - [Firewall Configuration](#firewall-configuration)
4. [Using Variables](#using-variables)
5. [Tags and Conditional Execution](#tags-and-conditional-execution)
6. [Ansible Roles](#ansible-roles)
7. [Handlers in Playbooks](#handlers-in-playbooks)
8. [Best Practices](#best-practices)

---

## Introduction

Ansible simplifies IT automation by allowing administrators to manage configurations and deployments using human-readable YAML syntax. This guide is designed to make it easier for users to write, understand, and execute Ansible playbooks effectively.

---

## Understanding YAML in Ansible

### What is YAML?
- YAML stands for "Yet Another Markup Language."
- Ansible uses YAML for defining playbooks because it is simple, human-readable, and concise compared to JSON or XML.

### Basic YAML Syntax
- Start a YAML file with `---` and optionally end with `...`.
- Key-value pairs, lists, and dictionaries are used to structure the data.

Example:
```yaml
---
name: My Ansible Playbook
hosts: all
tasks:
  - name: Create a directory
    file:
      path: /tmp/mydir
      state: directory
...
```

## Common Playbook Examples

## File Management
### Create a file or directory:
```yaml
---
- hosts: demo
   tasks:
     - name: Create a directory
       file:
         path: /home/ansible/mydir
         state: directory
     - name: Create a file
       file:
         path: /home/ansible/myfile.txt
         state: touch
...
```
![Screenshot from 2025-01-27 23-50-52](https://github.com/user-attachments/assets/3e91fc1f-40d1-43d5-8756-c8526065a4bb)


### Set permissions, ownership, and group:
```yaml
---
- hosts: demo
  tasks:
    - name: Change permissions and ownership
      file:
        path: /home/ansible/mydir
        mode: '0755'
        owner: ansible
        group: ansible
...
```
![Screenshot from 2025-01-27 23-52-41](https://github.com/user-attachments/assets/7f8d2b65-2c37-4228-9151-8b11be419992)


### User and Group Management
Create a user and group:
```yaml
---
- hosts: demo
  tasks:
    - name: Create a group
      group:
        name: developers
    - name: Create a user
      user:
        name: john
        group: developers
        shell: /bin/bash
...
```
![Screenshot from 2025-01-27 23-57-54](https://github.com/user-attachments/assets/eecbedd3-f9da-4566-ae34-9f9e6e74eb63)


Change a user's primary group:
```yaml
---
- hosts: demo
  tasks:
    - name: Change primary group
      user:
        name: john
        group: admin
...
```

### Package Installation
Install multiple packages:
```yaml
---
- hosts: demo
  tasks:
    - name: Install packages
      yum:
        name:
          - httpd
          - wget
        state: present
...
```
![Screenshot from 2025-01-28 00-01-10](https://github.com/user-attachments/assets/66f949a3-a657-4679-97fe-bbcf4b00b6ea)

### Firewall Configuration
Open firewall ports and enable services:
```yaml
---
- hosts: demo
  tasks:
    - name: Add firewall rules
      firewalld:
        service: "{{ item }}"
        permanent: true
        state: enabled
      with_items:
        - http
        - https
    - name: Reload firewall
      service:
        name: firewalld
        state: restarted
...
```

## Using Variables
### Define and use variables in playbooks:
```yaml
---
- hosts: demo
  vars:
    pkg_name: httpd
    project_root: /var/www/html
  tasks:
    - name: Install a package
      yum:
        name: "{{ pkg_name }}"
        state: present
    - name: Create project directory
      file:
        path: "{{ project_root }}"
        state: directory

...
```

### Using external variable files:
```yaml
---
- hosts: demo
  vars_files:
    - vars.yml
  tasks:
    - name: Display variables
      debug:
        msg: "Welcome to {{ company }} located at {{ location }}."
...
```

## Tags and Conditional Execution
### Add tags to tasks for selective execution:
```yaml
---
- hosts: demo
  tasks:
    - name: Install HTTPD
      yum:
        name: httpd
        state: present
      tags: webserver
    - name: Start HTTPD service
      service:
        name: httpd
        state: started
      tags: webserver

...
```

### Run specific tasks:
```
ansible-playbook site.yml --tags webserver
```

## Ansible Roles
Organize playbooks into roles for modularity and reusability.
### Directory structure:
```yaml
roles/
  webserver/
    tasks/main.yml
    templates/
    handlers/main.yml
    vars/main.yml
    defaults/main.yml
```

## Handlers in Playbooks
Handlers are triggered only when a task changes its state.
```yaml
---
- hosts: demo
  tasks:
    - name: Install HTTPD
      yum:
        name: httpd
        state: present
      notify: Restart HTTPD

  handlers:
    - name: Restart HTTPD
      service:
        name: httpd
        state: restarted
...
```
## Best Practices
- Use variables and external files for dynamic playbooks.
- Group tasks logically and use roles for scalability.
- Use tags for better task management.

## Contributing
Contributions are welcome! Fork this repository and create a pull request with your updates.

