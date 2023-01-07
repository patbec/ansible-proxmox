<img align="right" width="15%" src="docs/proxmox.svg" alt="Proxmox logo"/>

# Ansible Proxmox

Changes settings on the virtualization host <b>Proxmox</b>.

The following steps will be performed:
- Update login manager configuration to turn off the screen without suspending
- Remove Proxmox enterprise repository
- Add Proxmox no-subscription repository
- Enable the Web UI on port 443

For the last point *(Web UI on port 443)* the recommendation from the [official documentation](https://pve.proxmox.com/wiki/Web_Interface_Via_Nginx_Proxy) was used.

## Workspace

Open the workspace file `ansible-proxmox.code-workspace` to access the predefined build tasks with Visual Studio Code.

## Security

Security-critical data such as passwords or keys are encrypted with Ansible Vault.

> If you read this and find something, I did something wrong and you can email me at [security@thinkbox.center](mailto:security@thinkbox.center).