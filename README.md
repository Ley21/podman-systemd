# Podman systemd Role
## Description

This role creates systemd services for an pod, all required container and start and enable them.
Additonal this role will create backup/recovery scripts for all volumes.

## Requirments

- Ansible >2.8

Installed packages on target:
- podman

## Variables to configure on playbook

### Pod

Pod is the environment for all containers.

```yaml
pod: application_name # Name of application
ports:
  - 8080:80 # Exposed ports for container applaction
```

### Volumes

Volumes to use in different containers.

```yaml
volumes:
- name: apache
  containers: 
  - apache
  mount: "/var/www/html"
- ... # More volumes
```

### Containers

Containers to start in pod

```yaml
containers:
- name: apache
  image: image_name
  envs: # Optional
  - name: VARIABLE
    value: value_for
  args: "-v ..." # Additional args for podman container (see podman run docu)
  dependencies: # Optional
  - databes # Other container which is dependent to work
- ... # More containers
```

## Example Playbook

```yaml
---
- hosts: all
  become: true
  vars:
  # ... (see above)
  tasks:
  - name: Create podman for nextcloud    
    include_role:
      name: podman-systemd
```