<img align="right" width="15%" src="docs/proxmox.svg" alt="Proxmox logo"/>

# Ansible Proxmox

Changes settings on the virtualization host <b>Proxmox</b>.

The following steps will be performed:
- Updates the login manager configuration to disable hardware buttons<br>*PowerKey, SuspendKey, HibernateKey, etc.*
- Remove Proxmox enterprise repository
- Add Proxmox no-subscription repository
- Enable the Web UI on port 443

For the last point *(Web UI on port 443)* the recommendation from the [official documentation](https://pve.proxmox.com/wiki/Web_Interface_Via_Nginx_Proxy) was used.

## Webserver

Proxmox provides access to the API and web interface via port `8006`. To offer access via the standard HTTPS port, NGINX is installed in the light version.

NGINX requires a valid certificate, this can be configured via the interface under [ACME](https://pve.proxmox.com/wiki/Certificate_Management). After the correct setup, Proxmox will manage the certificate and renew it automatically. NGINX will use this certificate and **automatically reload it after a renewal by Proxmox**. See the next step for technical details.

## How it works

Configure on the Proxmox an **ACME** first, so the certificate `/etc/pve/local/pveproxy-ssl.pem` is created.

- If the certificate is renewed by Proxmox, the web server is **automatically reloaded**. This is made possible with the systemd option [`ReloadPropagatedFrom`](https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html#PropagatesReloadTo=).

- If no ACME has been set up, the service is **ignored when booting**. This is controlled by the [`ConditionPathExists`](https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html#AssertArchitecture=) option. If the service has been ignored, it remains deactivated until Proxmox is restarted.<br>There is a check in the Ansible playbook if ACME has been set up, without a valid configuration the execution of the playbook will be **aborted at the beginning**.

- If an existing ACME configuration is deleted in the Proxmox interface, the old certificate files remain available. The NGINX web server remains active and will respond with an expired certificate.

These options are stored in the NGINX extended service file under `/etc/systemd/system/nginx.service.d/override.conf`:
```ini
# {{ ansible_managed }}

[Unit]
# The path /etc/pve/local is only available after this service.
Requires=pve-cluster.service
After=pve-cluster.service

# The web server requires an existing certificate. The service is only
# activated if an automatic certificate management environment (ACME)
# has been set up in Promxox.
ConditionPathExists=/etc/pve/local/pveproxy-ssl.pem
ConditionPathExists=/etc/pve/local/pveproxy-ssl.key

# When systemd reload the unit listed here, the action is
# propagated to this unit. This occurs when the certificate is updated.
ReloadPropagatedFrom=pveproxy.service
```

> You can edit this file directly for test purposes using the command `sudo systemctl edit nginx`.

## Versions

The following versions were tested:

✅ Proxmox VE 7.4-xx

> This project is intended for my home proxmox server and should not be used on production servers.

## Workspace

Open the workspace file `proxmox.code-workspace` to access the predefined build tasks with Visual Studio Code.

Predefined build tasks:
| Task     | Description                                |            Command |
| -------- | ------------------------------------------ | -----------------: |
| 🚀 Deploy | Run the main playbook with all tasks.      | `ansible-playbook` |
| 🧪 Check  | Check the code without making any changes. | `ansible-playbook` |