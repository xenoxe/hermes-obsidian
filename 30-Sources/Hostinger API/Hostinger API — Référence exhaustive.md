---
tags: [hostinger, api, reference]
---

# Hostinger API — Référence exhaustive

Retour à l'index : [[Hostinger API — Index]] · **392 endpoints**, spec v1.54.2

Carte complète, groupée par produit puis par section. Les notes détaillées par produit : [[Hostinger API — VPS]] · [[Hostinger API — Domaines et DNS]] · [[Hostinger API — Mail]] · [[Hostinger API — Hébergement web et WordPress]] · [[Hostinger API — Billing, Reach et Ecommerce]]

Légende : 🧪 endpoint expérimental · ⚠️ endpoint déprécié

## vps (64 endpoints)

### VPS: Actions

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/actions` | Get actions |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/actions/{actionId}` | Get action details |

### VPS: Backups

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/backups` | Get backups |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/backups/{backupId}/restore` | Restore backup |

### VPS: Data centers

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/data-centers` | Get data center list |

### VPS: Docker Manager

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker` | Get project list 🧪 |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker` | Create new project 🧪 |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}` | Get project contents 🧪 |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/containers` | Get project containers 🧪 |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/down` | Delete project 🧪 |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/logs` | Get project logs 🧪 |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/restart` | Restart project 🧪 |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/start` | Start project 🧪 |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/stop` | Stop project 🧪 |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/update` | Update project 🧪 |

### VPS: Firewall

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/firewall` | Get firewall list |
| `POST` | `/api/vps/v1/firewall` | Create new firewall |
| `GET` | `/api/vps/v1/firewall/{firewallId}` | Get firewall details |
| `DELETE` | `/api/vps/v1/firewall/{firewallId}` | Delete firewall |
| `POST` | `/api/vps/v1/firewall/{firewallId}/activate/{virtualMachineId}` | Activate firewall |
| `POST` | `/api/vps/v1/firewall/{firewallId}/deactivate/{virtualMachineId}` | Deactivate firewall |
| `PUT` | `/api/vps/v1/firewall/{firewallId}/rules` | Replace all firewall rules in group |
| `POST` | `/api/vps/v1/firewall/{firewallId}/rules` | Create firewall rule |
| `PUT` | `/api/vps/v1/firewall/{firewallId}/rules/{ruleId}` | Update firewall rule |
| `DELETE` | `/api/vps/v1/firewall/{firewallId}/rules/{ruleId}` | Delete firewall rule |
| `POST` | `/api/vps/v1/firewall/{firewallId}/sync` | Sync firewall to all assigned VMs |
| `POST` | `/api/vps/v1/firewall/{firewallId}/sync/{virtualMachineId}` | Sync firewall |

### VPS: Malware scanner

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx` | Get scan metrics |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx` | Install Monarx |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx` | Uninstall Monarx |

### VPS: OS Templates

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/templates` | Get templates |
| `GET` | `/api/vps/v1/templates/{templateId}` | Get template details |

### VPS: PTR records

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/ptr/{ipAddressId}` | Create PTR record |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/ptr/{ipAddressId}` | Delete PTR record |

### VPS: Post-install scripts

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/post-install-scripts` | Get post-install scripts |
| `POST` | `/api/vps/v1/post-install-scripts` | Create post-install script |
| `GET` | `/api/vps/v1/post-install-scripts/{postInstallScriptId}` | Get post-install script |
| `PUT` | `/api/vps/v1/post-install-scripts/{postInstallScriptId}` | Update post-install script |
| `DELETE` | `/api/vps/v1/post-install-scripts/{postInstallScriptId}` | Delete post-install script |

### VPS: Public Keys

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/public-keys` | Get public keys |
| `POST` | `/api/vps/v1/public-keys` | Create public key |
| `POST` | `/api/vps/v1/public-keys/attach/{virtualMachineId}` | Attach public key |
| `DELETE` | `/api/vps/v1/public-keys/{publicKeyId}` | Delete public key |

### VPS: Recovery

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/recovery` | Start recovery mode |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/recovery` | Stop recovery mode |

### VPS: Snapshots

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot` | Get snapshot |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot` | Create snapshot |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot` | Delete snapshot |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot/restore` | Restore snapshot |

### VPS: Virtual machine

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines` | Get virtual machines |
| `POST` | `/api/vps/v1/virtual-machines` | Purchase new virtual machine |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}` | Get virtual machine details |
| `PUT` | `/api/vps/v1/virtual-machines/{virtualMachineId}/hostname` | Set hostname |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/hostname` | Reset hostname |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/metrics` | Get metrics |
| `PUT` | `/api/vps/v1/virtual-machines/{virtualMachineId}/nameservers` | Set nameservers |
| `PUT` | `/api/vps/v1/virtual-machines/{virtualMachineId}/panel-password` | Set panel password |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/public-keys` | Get attached public keys |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/recreate` | Recreate virtual machine |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/restart` | Restart virtual machine |
| `PUT` | `/api/vps/v1/virtual-machines/{virtualMachineId}/root-password` | Set root password |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/setup` | Setup purchased virtual machine |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/start` | Start virtual machine |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/stop` | Stop virtual machine |

## hosting (105 endpoints)

### Hosting: Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/cache/clear` | Clear website cache |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/cache/toggle` | Toggle website cache |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/cacheless-mode/toggle` | Toggle cacheless mode |

### Hosting: Cron Jobs

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/cron-jobs` | List account cron jobs |
| `POST` | `/api/hosting/v1/accounts/{username}/cron-jobs` | Create account cron job |
| `DELETE` | `/api/hosting/v1/accounts/{username}/cron-jobs/{uid}` | Delete account cron job |
| `GET` | `/api/hosting/v1/accounts/{username}/cron-jobs/{uid}/output` | Get cron job output |

### Hosting: Databases

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/databases` | List account databases |
| `POST` | `/api/hosting/v1/accounts/{username}/databases` | Create account database |
| `GET` | `/api/hosting/v1/accounts/{username}/databases/remote-connections` | List database remote connections |
| `DELETE` | `/api/hosting/v1/accounts/{username}/databases/{name}` | Delete account database |
| `PATCH` | `/api/hosting/v1/accounts/{username}/databases/{name}/change-password` | Change database password |
| `GET` | `/api/hosting/v1/accounts/{username}/databases/{name}/phpmyadmin-link` | Get phpMyAdmin link |
| `POST` | `/api/hosting/v1/accounts/{username}/databases/{name}/remote-connections` | Create database remote connection |
| `DELETE` | `/api/hosting/v1/accounts/{username}/databases/{name}/remote-connections` | Delete database remote connection |
| `PATCH` | `/api/hosting/v1/accounts/{username}/databases/{name}/repair` | Repair database |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/databases/setup` | Setup website database |

### Hosting: Datacenters

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/datacenters` | List available datacenters |

### Hosting: Domains

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/parked-domains` | List website parked domains |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/parked-domains` | Create website parked domain |
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/parked-domains/{parkedDomain}` | Delete website parked domain |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/subdomains` | List website subdomains |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/subdomains` | Create website subdomain |
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/subdomains/{subdomain}` | Delete website subdomain |
| `POST` | `/api/hosting/v1/domains/free-subdomains` | Generate a free subdomain |
| `POST` | `/api/hosting/v1/domains/verify-ownership` | Verify domain ownership |

### Hosting: Files

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/domains/{domain}/files` | List website files and directories |
| `GET` | `/api/hosting/v1/accounts/{username}/domains/{domain}/files/content` | Get website file content |
| `POST` | `/api/hosting/v1/files/upload-urls` | Generate upload URL |

### Hosting: Git

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings` | Get Git auto-deployment settings |
| `PUT` | `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings` | Update Git auto-deployment settings |
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings` | Delete Git auto-deployment settings |
| `GET` | `/api/hosting/v1/git/installations` | List Git installations |
| `GET` | `/api/hosting/v1/git/installations/{uuid}/repositories` | List Git installation repositories |

### Hosting: NodeJS

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds` | List NodeJS builds |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds` | Start Node.js build |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings` | Get Node.js build settings |
| `PUT` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings` | Update Node.js build settings |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings/env` | List Node.js environment variables |
| `PUT` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings/env` | Replace Node.js environment variables |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings/from-archive` | Get Node.js build settings from archive |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/{uuid}` | Get Node.js build details |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/{uuid}/analysis` | Analyse failed Node.js build |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/{uuid}/logs` | Get NodeJS build logs |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/runtime-logs` | Get Node.js runtime logs |
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/runtime-logs` | Clear Node.js runtime logs |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/server/restart` | Restart Node.js application |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/vulnerabilities` | List Node.js vulnerabilities |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/vulnerabilities/patch` | Patch Node.js vulnerabilities |

### Hosting: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/orders` | List orders |

### Hosting: PHP

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/details` | Get PHP details |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/extensions` | Update PHP extensions |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/extensions/reset` | Reset PHP extensions |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/options` | Update PHP options |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/php-info` | Get PHP info |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/version` | Update PHP version |

### Hosting: Redirects

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects` | List website redirects |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects` | Create website redirect |
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects` | Delete website redirect |

### Hosting: SSL

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl` | Uninstall SSL |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/https-redirect/toggle` | Toggle HTTPS redirect |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/setup` | Install SSL |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/status` | Get SSL status |

### Hosting: Websites

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/deploy` | Deploy static site archive |
| `GET` | `/api/hosting/v1/websites` | List websites |
| `POST` | `/api/hosting/v1/websites` | Create website |
| `DELETE` | `/api/hosting/v1/websites/{domain}` | Delete website |

### WordPress: AI Tools

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/hostinger-plugins/ai-option/status` | Show AI option status |
| `PATCH` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/hostinger-plugins/ai-option/status` | Set AI option status |

### WordPress: Installations

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/wordpress/import` | Import WordPress website |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/installations` | Install WordPress |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/installations/check-is-valid` | Check if WordPress installations are valid |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/installations/detect` | Detect WordPress installations |
| `DELETE` | `/api/hosting/v1/accounts/{username}/wordpress/{software}` | Delete WordPress installation |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/jwt-token` | Get installation JWT token |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/update` | Update WordPress core |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/updates` | List available WordPress core updates |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/version` | Show WordPress core version |
| `GET` | `/api/hosting/v1/wordpress/installations` | List WordPress installations |

### WordPress: LiteSpeed Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/litespeed-cache/purge` | Purge LiteSpeed Cache |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/litespeed-cache/status` | Show LiteSpeed Cache status |

### WordPress: Login

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/login/links` | Create login links |

### WordPress: Maintenance

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/maintenance/status` | Show maintenance status |
| `PATCH` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/maintenance/toggle` | Toggle maintenance mode |

### WordPress: Object Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/memcached/status` | Show Memcached object cache status |
| `PATCH` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/memcached/toggle` | Toggle Memcached object cache |

### WordPress: Plugins

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/wordpress/plugins/deploy` | Deploy WordPress plugin |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins` | List installed WordPress plugins |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/activate` | Activate WordPress plugin |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/available` | List available WordPress plugins |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/deactivate` | Deactivate WordPress plugin |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/hostinger/update` | Update Hostinger WordPress plugin |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/install` | Install WordPress plugins |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/uninstall` | Uninstall WordPress plugins |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/update` | Update WordPress plugins |
| `GET` | `/api/hosting/v1/wordpress/plugins` | Search WordPress plugins |
| `GET` | `/api/hosting/v1/wordpress/plugins/is-woocommerce-installed` | Check if WooCommerce is installed |
| `GET` | `/api/hosting/v1/wordpress/plugins/suggested` | List suggested WordPress plugins |

### WordPress: Themes

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/wordpress/themes/deploy` | Deploy WordPress theme |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes` | List installed WordPress themes |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/activate` | Activate WordPress theme |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/install` | Install WordPress theme |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/uninstall` | Uninstall WordPress themes |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/update` | Update WordPress themes |
| `GET` | `/api/hosting/v1/wordpress/themes` | List WordPress themes |

## agency-hosting (40 endpoints)

### Agency Hosting: Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/cache` | Clear website cache |

### Agency Hosting: Cron Jobs

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs` | List website cron jobs |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs` | Create website cron job |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs/{uuid}` | Delete website cron job |

### Agency Hosting: Databases

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/databases` | List website databases |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/databases` | Create website database |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}` | Delete website database |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}/users` | Create website database user |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}/users/{database_user_name}` | Delete website database user |

### Agency Hosting: Datacenters

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/datacenters` | List available datacenters |

### Agency Hosting: Domains

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/domains` | List domains |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/domains` | Link domain to website |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}` | Unlink domain from website |
| `PUT` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{from_domain}` | Change website domain |

### Agency Hosting: Files

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/files/import-archive` | Import website from archive |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/files/upload-urls` | Generate upload URL |

### Agency Hosting: Metrics

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/disk-usage-metrics` | List Agency Plan order disk usage metrics |
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/resource-usage-metrics` | List order resource usage metrics |

### Agency Hosting: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders` | List orders |

### Agency Hosting: PHP

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/websites/php-settings/versions` | List available PHP versions for an order |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/extensions` | List PHP extensions for a website |
| `PUT` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/extensions` | Replace website PHP extensions |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/options` | List PHP options for a website |
| `PUT` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/options` | Replace website PHP options |
| `PATCH` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/version` | Update website PHP version |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/versions` | List available PHP versions for a website |

### Agency Hosting: SSL

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl` | Uninstall website SSL |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/reinstall` | Reinstall website SSL |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/setup` | Install website SSL |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/status` | Get website SSL status |

### Agency Hosting: Website Setups

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/agency-hosting/v1/orders/{order_id}/websites/setups` | Create a new website |
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/websites/setups/{setup_uuid}` | Get website setup status |

### Agency Hosting: Websites

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites` | List Agency Plan websites |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}` | Get website details |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}` | Delete website |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/build-assets` | Build website NodeJS assets |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/processes` | List website processes |

### Agency Hosting: WordPress

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings` | Get WordPress settings |
| `PATCH` | `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings/version` | Change WordPress version |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings/versions` | List available WordPress versions |

## domains (40 endpoints)

### Domains: Availability

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/domains/v1/availability` | Check domain availability |
| `POST` | `/api/domains/v1/availability/alternatives-from-description` | Suggest domain names from a description |
| `POST` | `/api/domains/v1/availability/alternatives-from-domain` | Suggest domain names from a domain |

### Domains: Forwarding

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/domains/v1/forwarding` | Create domain forwarding |
| `GET` | `/api/domains/v1/forwarding/{domain}` | Get domain forwarding |
| `PUT` | `/api/domains/v1/forwarding/{domain}` | Update domain forwarding |
| `DELETE` | `/api/domains/v1/forwarding/{domain}` | Delete domain forwarding |

### Domains: Move

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/domains/v1/move/incoming` | Get incoming domain move list |
| `GET` | `/api/domains/v1/move/incoming/{domain}` | Get incoming domain move |
| `PUT` | `/api/domains/v1/move/incoming/{domain}` | Accept incoming domain move |
| `DELETE` | `/api/domains/v1/move/incoming/{domain}` | Reject incoming domain move |
| `GET` | `/api/domains/v1/move/outgoing` | Get outgoing domain move list |
| `GET` | `/api/domains/v1/move/outgoing/{domain}` | Get outgoing domain move |
| `POST` | `/api/domains/v1/move/outgoing/{domain}` | Start outgoing domain move |
| `DELETE` | `/api/domains/v1/move/outgoing/{domain}` | Cancel outgoing domain move |

### Domains: Portfolio

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/domains/v1/portfolio` | Get domain list |
| `POST` | `/api/domains/v1/portfolio` | Purchase new domain |
| `POST` | `/api/domains/v1/portfolio/claim` | Claim free domain |
| `GET` | `/api/domains/v1/portfolio/{domain}` | Get domain details |
| `GET` | `/api/domains/v1/portfolio/{domain}/auth-code` | Get domain authorization code |
| `PUT` | `/api/domains/v1/portfolio/{domain}/domain-lock` | Enable domain lock |
| `DELETE` | `/api/domains/v1/portfolio/{domain}/domain-lock` | Disable domain lock |
| `PUT` | `/api/domains/v1/portfolio/{domain}/nameservers` | Update domain nameservers |
| `PUT` | `/api/domains/v1/portfolio/{domain}/privacy-protection` | Enable privacy protection |
| `DELETE` | `/api/domains/v1/portfolio/{domain}/privacy-protection` | Disable privacy protection |
| `GET` | `/api/domains/v1/portfolio/{domain}/renewal` | Get domain renewal information |
| `POST` | `/api/domains/v1/portfolio/{domain}/setup` | Complete domain setup |

### Domains: Transfer

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/domains/v1/transfers` | Get transfer list |
| `POST` | `/api/domains/v1/transfers/claim` | Claim free domain transfer |
| `GET` | `/api/domains/v1/transfers/{domain}` | Get transfer |

### Domains: WHOIS

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/domains/v1/irtp/{domain}` | Get pending IRTP verification |
| `DELETE` | `/api/domains/v1/irtp/{domain}` | Cancel pending IRTP verification |
| `GET` | `/api/domains/v1/whois` | Get WHOIS profile list |
| `POST` | `/api/domains/v1/whois` | Create WHOIS profile |
| `PUT` | `/api/domains/v1/whois/change` | Change WHOIS profile for domain |
| `PUT` | `/api/domains/v1/whois/default/{whoisId}` | Set WHOIS profile as default |
| `DELETE` | `/api/domains/v1/whois/default/{whoisId}` | Unset default WHOIS profile |
| `GET` | `/api/domains/v1/whois/{whoisId}` | Get WHOIS profile |
| `DELETE` | `/api/domains/v1/whois/{whoisId}` | Delete WHOIS profile |
| `GET` | `/api/domains/v1/whois/{whoisId}/usage` | Get WHOIS profile usage |

## dns (8 endpoints)

### DNS: Snapshot

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/dns/v1/snapshots/{domain}` | Get DNS snapshot list |
| `GET` | `/api/dns/v1/snapshots/{domain}/{snapshotId}` | Get DNS snapshot |
| `POST` | `/api/dns/v1/snapshots/{domain}/{snapshotId}/restore` | Restore DNS snapshot |

### DNS: Zone

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/dns/v1/zones/{domain}` | Get DNS records |
| `PUT` | `/api/dns/v1/zones/{domain}` | Update DNS records |
| `DELETE` | `/api/dns/v1/zones/{domain}` | Delete DNS records |
| `POST` | `/api/dns/v1/zones/{domain}/reset` | Reset DNS records |
| `POST` | `/api/dns/v1/zones/{domain}/validate` | Validate DNS records |

## mail (38 endpoints)

### Mail: API Tokens

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/mail/v1/api-tokens` | List API tokens |
| `DELETE` | `/api/mail/v1/api-tokens/{tokenId}` | Revoke API token |
| `POST` | `/api/mail/v1/orders/{orderId}/api-tokens` | Create API token |

### Mail: Aliases

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/aliases/{aliasId}` | Delete alias |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/aliases` | Create alias |
| `GET` | `/api/mail/v1/orders/{orderId}/aliases` | List aliases |

### Mail: Autoreplies

| Méthode | Endpoint | Description |
|---|---|---|
| `PUT` | `/api/mail/v1/autoreplies/{autoreplyId}` | Update autoreply |
| `DELETE` | `/api/mail/v1/autoreplies/{autoreplyId}` | Delete autoreply |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/autoreplies` | Create autoreply |
| `GET` | `/api/mail/v1/orders/{orderId}/autoreplies` | List autoreplies |

### Mail: Catchalls

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/catchalls/{catchallId}` | Delete catch-all |
| `POST` | `/api/mail/v1/catchalls/{catchallId}/confirmation/resend` | Resend catch-all confirmation |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/catchalls` | Create catch-all |
| `GET` | `/api/mail/v1/orders/{orderId}/catchalls` | List catch-alls |

### Mail: Forwarders

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/forwarders/{forwarderId}` | Delete forwarder |
| `POST` | `/api/mail/v1/forwarders/{forwarderId}/confirmation/resend` | Resend forwarder confirmation |
| `PATCH` | `/api/mail/v1/forwarders/{forwarderId}/keep-copy` | Update forwarder keep-copy setting |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/forwarders` | Create forwarder |
| `GET` | `/api/mail/v1/orders/{orderId}/forwarders` | List forwarders |

### Mail: Logs

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/mail/v1/orders/{orderId}/logs/access` | List access logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/action` | List action logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/inbound` | List inbound logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/mailbox-actions` | List mailbox action logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/outbound` | List outbound logs |

### Mail: Mailboxes

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/mailboxes/{mailboxId}` | Delete mailbox |
| `PATCH` | `/api/mail/v1/mailboxes/{mailboxId}/password` | Change mailbox password |
| `GET` | `/api/mail/v1/orders/{orderId}/mailboxes` | List mailboxes |
| `POST` | `/api/mail/v1/orders/{orderId}/mailboxes` | Create mailbox |

### Mail: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/mail/v1/orders` | List orders |
| `GET` | `/api/mail/v1/orders/{orderId}/plan` | Get order plan |

### Mail: Webhooks

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/webhooks` | Create webhook |
| `GET` | `/api/mail/v1/orders/{orderId}/webhooks` | List webhooks |
| `GET` | `/api/mail/v1/orders/{orderId}/webhooks/delivery-logs` | List webhook delivery logs |
| `GET` | `/api/mail/v1/webhooks/{webhookId}` | Get webhook |
| `DELETE` | `/api/mail/v1/webhooks/{webhookId}` | Delete webhook |
| `PATCH` | `/api/mail/v1/webhooks/{webhookId}` | Update webhook |
| `POST` | `/api/mail/v1/webhooks/{webhookId}/regenerate-secret` | Regenerate webhook secret |
| `POST` | `/api/mail/v1/webhooks/{webhookId}/test` | Test webhook |

## billing (9 endpoints)

### Billing: Catalog

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/billing/v1/catalog` | Get catalog item list |

### Billing: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/billing/v1/orders` | Create purchase order |

### Billing: Payment methods

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/billing/v1/payment-methods` | Get payment method list |
| `POST` | `/api/billing/v1/payment-methods/{paymentMethodId}` | Set default payment method |
| `DELETE` | `/api/billing/v1/payment-methods/{paymentMethodId}` | Delete payment method |

### Billing: Subscriptions

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/billing/v1/subscriptions` | Get subscription list |
| `DELETE` | `/api/billing/v1/subscriptions/{subscriptionId}/auto-renewal/disable` | Disable auto-renewal |
| `PATCH` | `/api/billing/v1/subscriptions/{subscriptionId}/auto-renewal/enable` | Enable auto-renewal |
| `POST` | `/api/billing/v1/subscriptions/{subscriptionId}/renew` | Renew subscription |

## reach (52 endpoints)

### Reach: Automations

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/automations` | List automations |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/automations/{automationUuid}` | Get automation details |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/automations/{automationUuid}/steps` | List automation steps |

### Reach: Campaigns

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/campaigns` | List campaigns |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/campaigns` | Create a draft campaign |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/campaigns/{campaignUuid}` | Get campaign details |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/campaigns/{campaignUuid}/statistics` | Get campaign performance |

### Reach: Contact Fields

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields` | List contact fields |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields` | Create a contact field |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields/{fieldUuid}` | Delete a contact field |
| `PATCH` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields/{fieldUuid}` | Update a contact field |

### Reach: Contacts

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/contacts` | List contacts |
| `POST` | `/api/reach/v1/contacts` | Create a new contact |
| `GET` | `/api/reach/v1/contacts/groups` | List contact groups |
| `DELETE` | `/api/reach/v1/contacts/{uuid}` | Delete a contact |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/contacts` | List profile contacts |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/contacts` | Create new contacts |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/contacts/bulk` | Create contacts in bulk |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}` | Get contact details |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}` | Delete a profile contact |
| `PATCH` | `/api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}` | Update a contact |

### Reach: Forms

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/forms` | List forms |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/forms/{formUuid}` | Get form details |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/forms/{formUuid}` | Delete form |

### Reach: Profiles

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles` | List Profiles |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/domains` | Get connected sending domain |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/domains/dns-status` | Get profile domain DNS status |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/features` | List plan feature access |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/limits` | Get remaining plan limits |

### Reach: Segments

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/segmentation/filters/attributes` | List segment filter attributes |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/segmentation/filters/contacts` | Preview contacts matching conditions |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/segmentation/segments` | List profile segments |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/segmentation/segments` | Create a profile segment |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}` | Get profile segment details |
| `PUT` | `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}` | Update a profile segment |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}` | Delete a profile segment |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}/contacts` | List profile segment contacts |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}/count` | Count profile segment contacts |
| `GET` | `/api/reach/v1/segmentation/segments` | List segments |
| `POST` | `/api/reach/v1/segmentation/segments` | Create a new contact segment |
| `GET` | `/api/reach/v1/segmentation/segments/{segmentUuid}` | Get segment details |
| `GET` | `/api/reach/v1/segmentation/segments/{segmentUuid}/contacts` | List segment contacts |

### Reach: Tags

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/tags` | List profile tags |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/tags` | Create or find tags |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}` | Delete a tag |
| `PATCH` | `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}` | Rename a tag |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts` | Assign contacts to a tag |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts` | Remove contacts from a tag |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts/{contactUuid}` | Assign a contact to a tag |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts/{contactUuid}` | Remove a contact from a tag |

### Reach: Templates

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/templates` | List email templates |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/templates` | Create an email template |

## ecommerce (29 endpoints)

### Ecommerce: Discounts

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/discounts` | List discounts |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/discounts` | Create a discount |

### Ecommerce: Miscellaneous

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/miscellaneous/custom-storefront-instructions` | Get custom storefront setup instructions |

### Ecommerce: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/orders` | List store orders |
| `GET` | `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}` | Retrieve an order |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}/cancel` | Cancel an order |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}/fulfill` | Fulfil an order |

### Ecommerce: Payments

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/ecommerce/v1/stores/{store_id}/payment-methods/manual` | Enable manual payment method |
| `GET` | `/api/ecommerce/v1/stores/{store_id}/payment-providers` | List store payment providers |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/payment-providers/{provider_id}/connect-link` | Create a payment provider connect link |

### Ecommerce: Product variants

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants` | List product variants |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants` | Create a product variant |
| `PATCH` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants/batch` | Update product variants in batch |
| `DELETE` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants/{variant_id}` | Delete a product variant |

### Ecommerce: Products

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/products` | List products |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/digital` | Create digital product |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/physical` | Create physical product |
| `DELETE` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}` | Delete a product |
| `PATCH` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}` | Update a product |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/images` | Upload and attach a product image |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/images/upload-url` | Create a product image upload URL |

### Ecommerce: Sales channels

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/sales-channels` | List sales channels |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/sales-channels` | Create a sales channel |
| `PATCH` | `/api/ecommerce/v1/stores/{store_id}/sales-channels/{sales_channel_id}` | Update sales channel |

### Ecommerce: Shipping

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/ecommerce/v1/stores/{store_id}/shipping` | Set store shipping |

### Ecommerce: Stores

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores` | Get stores |
| `POST` | `/api/ecommerce/v1/stores` | Create store |
| `DELETE` | `/api/ecommerce/v1/stores/{store_id}` | Delete store |
| `GET` | `/api/ecommerce/v1/stores/{store_id}/metadata` | Get store metadata |

## horizons (6 endpoints)

### Horizons: Websites

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/horizons/v1/websites` | Get website list |
| `POST` | `/api/horizons/v1/websites` | Create website |
| `GET` | `/api/horizons/v1/websites/{websiteId}` | Get website |
| `POST` | `/api/horizons/v1/websites/{websiteId}/clone` | Clone website |
| `POST` | `/api/horizons/v1/websites/{websiteId}/messages` | Edit website |
| `POST` | `/api/horizons/v1/websites/{websiteId}/publish` | Publish website |

## autre (1 endpoints)

### Domain Access Verifier: Verifications

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/v2/direct/verifications/active` | Get domain verifications |