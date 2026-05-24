# Miscellaneous Ansible Playbooks

Collection of useful Ansible playbooks for system administration, configuration management, and automation tasks.

## <¯ Overview

This repository contains a variety of Ansible playbooks and Jinja2 templates for common DevOps and system administration tasks. Each playbook is designed to be modular, reusable, and follows Ansible best practices.

## =Á Contents

### Playbooks

#### **errror_handling.yml**
Demonstrates error handling and block rescue patterns in Ansible:
- Downloads transaction lists from remote sources
- Shows proper error handling with block/rescue constructs
- Includes file manipulation and string replacement tasks

#### **deploy_sudo_template.yml**
Automates sudoers configuration management:
- Deploys sudo configuration templates
- Uses Jinja2 templating for dynamic sudoers rules
- Manages sudo access for different user groups

#### **mutiple_hosts_play_example.yml**
Example playbook showing multi-host management:
- Demonstrates running tasks across multiple host groups
- Shows host pattern matching and delegation
- Includes inter-playbook variable usage

#### **nfs.yml & nfs_final.yml**
Network File System setup and configuration:
- **nfs.yml**: Initial NFS server setup
- **nfs_final.yml**: Complete NFS configuration with security hardening
- Includes export configuration and client setup

#### **website_install.yml**
Automated web server deployment:
- Installs and configures web servers (Apache/Nginx)
- Sets up virtual hosts and SSL certificates
- Includes basic web application deployment steps

### Jinja2 Templates

#### **etc.hosts.j2**
Template for managing `/etc/hosts` file:
- Dynamically generates host entries based on inventory
- Includes localhost and Ansible hostname variables
- Useful for multi-server environments

#### **exports.j2**
NFS export configuration template:
- Defines NFS share paths and permissions
- Includes IP-based access control
- Supports read-write (rw) mount options

#### **hardened.j2**
Security hardening template for sudoers:
- Implements role-based access control
- Defines sysops group permissions
- Includes host-based sudo restrictions

## =€ Usage

### Prerequisites
```bash
# Install Ansible
pip install ansible

# Or use package manager
sudo apt-get install ansible  # Ubuntu/Debian
sudo yum install ansible      # CentOS/RHEL
```

### Running Playbooks

#### Basic execution:
```bash
ansible-playbook -i inventory errror_handling.yml
```

#### With custom inventory:
```bash
ansible-playbook -i /path/to/inventory nfs.yml
```

#### With extra variables:
```bash
ansible-playbook website_install.yml -e "domain=example.com ssl_enabled=true"
```

#### With specific hosts:
```bash
ansible-playbook -i inventory deploy_sudo_template.yml --limit webservers
```

### Inventory Configuration

Example `inventory` file:
```ini
[web]
web1.example.com
web2.example.com

[nfs]
nfs-server.example.com

[all:vars]
ansible_user=ubuntu
ansible_private_key_file=~/.ssh/id_rsa
```

## =' Configuration

### Required Variables

Most playbooks require these common variables:

```yaml
# NFS Configuration
nfs_ip: "192.168.1.100"
nfs_hostname: "nfs-server"
share_path: "/data/share"

# Web Server
domain: "example.com"
document_root: "/var/www/html"

# Sudo Configuration
sysops_group: "sysops"
sudo_rules_path: "/etc/sudoers.d/"
```

### Template Variables

Jinja2 templates use Ansible facts:
- `{{ ansible_hostname }}` - System hostname
- `{{ ansible_default_ipv4.address }}` - Primary IP address
- `{{ groups['web'] }}` - Host groups from inventory
- `{{ groups['web']|join(', ') }}` - Comma-separated host lists

## =Ë Playbook Features

### Error Handling
- **Block/Rescue**: Graceful error handling and recovery
- **Retry Logic**: Automatic retries for transient failures
- **Rollback**: Automatic rollback on failure

### Security
- **Sudo Management**: Controlled privilege escalation
- **Hardening Templates**: Security-focused configurations
- **Access Control**: Role-based permissions

### Modularity
- **Reusable Tasks**: Common task patterns
- **Template Support**: Dynamic configuration generation
- **Host Grouping**: Organized target management

## =à Advanced Usage

### Custom Variables
```bash
ansible-playbook nfs.yml \
  -e "nfs_ip=10.0.0.1" \
  -e "share_path=/mnt/data" \
  -e "client_network=10.0.0.0/24"
```

### Dry Run
```bash
ansible-playbook website_install.yml --check
```

### Verbose Output
```bash
ansible-playbook errror_handling.yml -v
ansible-playbook nfs.yml -vvv  # Very verbose
```

### Tags
```bash
ansible-playbook deploy_sudo_template.yml --tags "configuration"
ansible-playbook website_install.yml --skip-tags "ssl"
```

## =Ú Examples

### Deploy NFS Server
```bash
ansible-playbook -i inventory nfs_final.yml \
  -e "nfs_ip=192.168.1.100" \
  -e "share_path=/data/nfs"
```

### Configure Sudo Access
```bash
ansible-playbook -i inventory deploy_sudo_template.yml \
  -e "sysops_group=devops" \
  -e "host_alias=webservers"
```

### Setup Web Server
```bash
ansible-playbook -i inventory website_install.yml \
  -e "domain=myapp.com" \
  -e "ssl_enabled=true"
```

## > Contributing

Contributions welcome! Please:
1. Follow Ansible best practices
2. Include error handling
3. Add documentation for variables
4. Test playbooks before submitting

## =Ý Notes

- **Error Handling**: The repository intentionally includes `errror_handling.yml` (with typo) as an example
- **Template Security**: Review hardening templates before production use
- **Backup**: Always backup configuration files before deployment
- **Testing**: Use `--check` mode for dry-run testing

## = Related Repositories

- **ansible**: System administration playbooks
- **percona_client_install_ansible**: Database-focused automation

---

**Miscellaneous Ansible Playbooks** - Streamlining system administration with reusable, well-documented automation.