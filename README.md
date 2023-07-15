<img align="right" width="15%" src="docs/proxmox.svg" alt="Proxmox logo"/>

# Ansible Proxmox

Changes settings on the virtualization host <b>Proxmox</b>.

The following steps will be performed:
- Updates the login manager configuration to disable hardware buttons<br>*PowerKey, SuspendKey, HibernateKey, etc.*
- Remove Proxmox enterprise repository
- Add Proxmox no-subscription repository
- Enable the Web UI on port 443

For the last point *(Web UI on port 443)* the recommendation from the [official documentation](https://pve.proxmox.com/wiki/Web_Interface_Via_Nginx_Proxy) was used.

## Preparation

Configure on the Proxmox an **ACME Challenge** first, so the certificate `/etc/pve/local/pveproxy-ssl.pem` is created. The playbook checks if this file exists, the web server will not start otherwise.

> This project is intended for my home proxmox server and should not be used on production servers.

## Versions

The following versions were tested:

✅ Proxmox VE 7.4-xx

## Workspace

Open the workspace file `proxmox.code-workspace` to access the predefined build tasks with Visual Studio Code.

Predefined build tasks:
| Task     | Description                                |            Command |
| -------- | ------------------------------------------ | -----------------: |
| 🚀 Deploy | Run the main playbook with all tasks.      | `ansible-playbook` |
| 🧪 Check  | Check the code without making any changes. | `ansible-playbook` |
