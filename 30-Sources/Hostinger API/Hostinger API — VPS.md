---
tags: [hostinger, api, reference]
---

# Hostinger API — VPS

Retour à l'index : [[Hostinger API — Index]] · 64 endpoints sur `/api/vps/v1`, 13 sections.

## Règles de ce compte

> [!warning] Périmètre de déploiement
> - Cible autorisée **uniquement** : `srv1398132.hstgr.cloud` — VM id **1398132**, IP **148.230.114.215** (Ubuntu 24.04 + Docker + Traefik, 2 vCPU / 8 Go / 100 Go).
> - `srv788682.hstgr.cloud` (VM id **788682**, IP 147.93.53.234) est **hors périmètre, définitivement** : aucune écriture, même sur demande explicite.
> - Avant toute action d'écriture : re-`GET` `/api/vps/v1/virtual-machines` et vérifier l'id/hostname — ne jamais se fier à un id de mémoire.

- **HTTPS via Traefik obligatoire**, jamais de port hôte publié.
- **Préférer l'image officielle** de l'app au catalogue avant d'écrire un compose maison. (Le catalogue one-click d'hPanel n'est **pas** exposé par l'API : `Catalog` = offres/pricing de facturation, pas des installations d'apps.)
- Les actions d'écriture renvoient une **action asynchrone** (`{"state": "sent"}`) : poller ensuite `/containers` ou `/actions`.

## Docker Manager — la recette qui fonctionne

```bash
VM=1398132
# 1. Lister les projets existants (lecture)
curl -s "https://developers.hostinger.com/api/vps/v1/virtual-machines/$VM/docker" \
  -H "Authorization: Bearer $HOSTINGER_API_KEY" -H "Content-Type: application/json"

# 2. Déployer (le corps peut être du YAML, une URL de compose, ou un dépôt GitHub)
#    POST remplace un projet de même nom — c'est plus sûr que /update (qui ne prend pas de corps)
curl -s -X POST "https://developers.hostinger.com/api/vps/v1/virtual-machines/$VM/docker" \
  -H "Authorization: Bearer $HOSTINGER_API_KEY" -H "Content-Type: application/json" \
  -d '{"project_name": "mon-app", "content": "<contenu docker-compose.yaml>"}'

# 3. Poller l'état réel du conteneur, puis les logs
curl -s ".../docker/mon-app/containers" -H "Authorization: Bearer $HOSTINGER_API_KEY"
curl -s ".../docker/mon-app/logs"       -H "Authorization: Bearer $HOSTINGER_API_KEY"
```

### Labels Traefik à mettre sur le service

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.mon-app.entrypoints=websecure"
  - "traefik.http.routers.mon-app.tls.certresolver=letsencrypt"
  - "traefik.http.routers.mon-app.rule=Host(`mon-app.srv1398132.hstgr.cloud`)"
  - "traefik.http.services.mon-app.loadbalancer.server.port=3000"
```

> [!tip] Deux pièges Traefik v3 qui coûtent des heures
> 1. **Domaine littéral sans backticks** → `Host(...) is not defined`, route ignorée, 404 + certificat `TRAEFIK DEFAULT CERT`. Un domaine écrit en dur **doit** être entre backticks.
> 2. **BasicAuth dans un compose** : chaque `$` du hash bcrypt doit être doublé (`$$2y$$12$$…`), sinon le hash est mutilé et Traefik renvoie un 401 permanent, sans rien dire dans ses logs.

### Vérification après déploiement

```bash
curl -k -o /dev/null -w '%{http_code}\n' https://mon-app.srv1398132.hstgr.cloud/   # attendu : 200
curl -o /dev/null -w '%{http_code}\n' http://mon-app.srv1398132.hstgr.cloud/      # attendu : 301
# et le certificat doit être Let's Encrypt, pas « TRAEFIK DEFAULT CERT »
```

### Pièges de dépannage

- Conteneur en boucle (`state=restarting`, aucun port) = échec au démarrage → **lire `/logs`**, ne pas deviner.
- Toujours `restart: unless-stopped` + un volume nommé pour la persistance.
- Les erreurs de parsing de règle Traefik apparaissent dans les logs du projet `traefik` lui-même.

## Endpoints (`/api/vps/v1`)

## VPS: Actions

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/actions` | Get actions |
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/actions/{actionId}` | Get action details |

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/actions`
**Get actions**

Retrieve actions performed on a specified virtual machine. Actions are operations or events that have been executed on the virtual machine, such as starting, stopping, or modifying the machine. This endpoint allows you to view the history of these actions, providing details about each action, such as the action name, timestamp, and status. Use this endpoint to view VPS operation history and troubleshoot issues.

- **Chemin** : `virtualMachineId`
- **Query** : `page`

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/actions/{actionId}`
**Get action details**

Retrieve detailed information about a specific action performed on a specified virtual machine. Use this endpoint to monitor specific VPS operation status and details.

- **Chemin** : `virtualMachineId`, `actionId`


## VPS: Backups

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/backups` | Get backups |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/backups/{backupId}/restore` | Restore backup |

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/backups`
**Get backups**

Retrieve backups for a specified virtual machine. Use this endpoint to view available backup points for VPS data recovery.

- **Chemin** : `virtualMachineId`
- **Query** : `page`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/backups/{backupId}/restore`
**Restore backup**

Restore a backup for a specified virtual machine. The system will then initiate the restore process, which may take some time depending on the size of the backup. **All data on the virtual machine will be overwritten with the data from the backup.** Use this endpoint to recover VPS data from backup points.

- **Chemin** : `virtualMachineId`, `backupId`


## VPS: Data centers

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/data-centers` | Get data center list |

#### `GET` `/api/vps/v1/data-centers`
**Get data center list**

Retrieve all available data centers. Use this endpoint to view location options before deploying VPS instances.

- Aucun paramètre


## VPS: Docker Manager

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

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker`
**Get project list** · *expérimental*

Retrieves a list of all Docker Compose projects currently deployed on the virtual machine. This endpoint returns basic information about each project including name, status, file path and list of containers with details about their names, image, status, health and ports. Container stats are omitted in this endpoint. If you need to get detailed information about container with stats included, use the `Get project cont […]

- **Chemin** : `virtualMachineId`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker`
**Create new project** · *expérimental*

Deploy new project from docker-compose.yaml contents or download contents from URL. URL can be Github repository url in format https://github.com/[user]/[repo] and it will be automatically resolved to docker-compose.yaml file in master branch. Any other URL provided must return docker-compose.yaml file contents. If project with the same name already exists, existing project will be replaced.

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `project_name` — string · **requis** — Docker Compose project name using alphanumeric characters, dashes, and underscores only
    - `content` — string · **requis** — URL pointing to docker-compose.yaml file, Github repository or raw YAML content of the compose file
    - `environment` — string — Project environment variables

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}`
**Get project contents** · *expérimental*

Retrieves the complete project information including the docker-compose.yml file contents, project metadata, and current deployment status. This endpoint provides the full configuration and state details of a specific Docker Compose project. Use this to inspect project settings, review the compose file, or check the overall project health.

- **Chemin** : `virtualMachineId`, `projectName`

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/containers`
**Get project containers** · *expérimental*

Retrieves a list of all containers belonging to a specific Docker Compose project on the virtual machine. This endpoint returns detailed information about each container including their current status, port mappings, and runtime configuration. Use this to monitor the health and state of all services within your Docker Compose project.

- **Chemin** : `virtualMachineId`, `projectName`

#### `DELETE` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/down`
**Delete project** · *expérimental*

Completely removes a Docker Compose project from the virtual machine, stopping all containers and cleaning up associated resources including networks, volumes, and images. This operation is irreversible and will delete all project data. Use this when you want to permanently remove a project and free up system resources.

- **Chemin** : `virtualMachineId`, `projectName`

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/logs`
**Get project logs** · *expérimental*

Retrieves aggregated log entries from all services within a Docker Compose project. This endpoint returns recent log output from each container, organized by service name with timestamps. The response contains the last 300 log entries across all services. Use this for debugging, monitoring application behavior, and troubleshooting issues across your entire project stack.

- **Chemin** : `virtualMachineId`, `projectName`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/restart`
**Restart project** · *expérimental*

Restarts all services in a Docker Compose project by stopping and starting containers in the correct dependency order. This operation preserves data volumes and network configurations while refreshing the running containers. Use this to apply configuration changes or recover from service failures.

- **Chemin** : `virtualMachineId`, `projectName`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/start`
**Start project** · *expérimental*

Starts all services in a Docker Compose project that are currently stopped. This operation brings up containers in the correct dependency order as defined in the compose file. Use this to resume a project that was previously stopped or to start services after a system reboot.

- **Chemin** : `virtualMachineId`, `projectName`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/stop`
**Stop project** · *expérimental*

Stops all running services in a Docker Compose project while preserving container configurations and data volumes. This operation gracefully shuts down containers in reverse dependency order. Use this to temporarily halt a project without removing data or configurations.

- **Chemin** : `virtualMachineId`, `projectName`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/docker/{projectName}/update`
**Update project** · *expérimental*

Updates a Docker Compose project by pulling the latest image versions and recreating containers with new configurations. This operation preserves data volumes while applying changes from the compose file. Use this to deploy application updates, apply configuration changes, or refresh container images to their latest versions.

- **Chemin** : `virtualMachineId`, `projectName`


## VPS: Firewall

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

#### `GET` `/api/vps/v1/firewall`
**Get firewall list**

Retrieve all available firewalls. Use this endpoint to view existing firewall configurations.

- **Query** : `page`

#### `POST` `/api/vps/v1/firewall`
**Create new firewall**

Create a new firewall. Use this endpoint to set up new firewall configurations for VPS security.

- **Corps** (requis) :
    - `name` — string · **requis**

#### `GET` `/api/vps/v1/firewall/{firewallId}`
**Get firewall details**

Retrieve firewall by its ID and rules associated with it. Use this endpoint to view specific firewall configuration and rules.

- **Chemin** : `firewallId`

#### `DELETE` `/api/vps/v1/firewall/{firewallId}`
**Delete firewall**

Delete a specified firewall. Any virtual machine that has this firewall activated will automatically have it deactivated. Use this endpoint to remove unused firewall configurations.

- **Chemin** : `firewallId`

#### `POST` `/api/vps/v1/firewall/{firewallId}/activate/{virtualMachineId}`
**Activate firewall**

Activate a firewall for a specified virtual machine. Only one firewall can be active for a virtual machine at a time. Use this endpoint to apply firewall rules to VPS instances.

- **Chemin** : `firewallId`, `virtualMachineId`

#### `POST` `/api/vps/v1/firewall/{firewallId}/deactivate/{virtualMachineId}`
**Deactivate firewall**

Deactivate a firewall for a specified virtual machine. Use this endpoint to remove firewall protection from VPS instances.

- **Chemin** : `firewallId`, `virtualMachineId`

#### `PUT` `/api/vps/v1/firewall/{firewallId}/rules`
**Replace all firewall rules in group**

Replaces all firewall rules within a specified firewall group with the provided set of rules in a single atomic operation, instead of creating or deleting rules one by one. Any virtual machine using this firewall group will need to be synchronized after replacing rules; pass the "sync" parameter to trigger synchronization immediately.

- **Chemin** : `firewallId`
- **Corps** (requis) :
    - `rules` — array<object> · **requis** — The complete set of firewall rules that atomically replaces all existing rules in the group
    - `sync` — boolean — Synchronize the firewall group to all its virtual machines after replacing the rules

#### `POST` `/api/vps/v1/firewall/{firewallId}/rules`
**Create firewall rule**

Create new firewall rule for a specified firewall. By default, the firewall drops all incoming traffic, which means you must add accept rules for all ports you want to use. Any virtual machine that has this firewall activated will lose sync with the firewall and will have to be synced again manually. Use this endpoint to add new security rules to firewalls.

- **Chemin** : `firewallId`
- **Corps** (requis) :
    - `protocol` — enum · **requis**
    - `port` — string · **requis** — Port or port range, ex: 1024:2048
    - `source` — enum · **requis**
    - `source_detail` — string · **requis** — IP range, CIDR, single IP or `any`

#### `PUT` `/api/vps/v1/firewall/{firewallId}/rules/{ruleId}`
**Update firewall rule**

Update a specific firewall rule from a specified firewall. Any virtual machine that has this firewall activated will lose sync with the firewall and will have to be synced again manually. Use this endpoint to modify existing firewall rules.

- **Chemin** : `firewallId`, `ruleId`
- **Corps** (requis) :
    - `protocol` — enum · **requis**
    - `port` — string · **requis** — Port or port range, ex: 1024:2048
    - `source` — enum · **requis**
    - `source_detail` — string · **requis** — IP range, CIDR, single IP or `any`

#### `DELETE` `/api/vps/v1/firewall/{firewallId}/rules/{ruleId}`
**Delete firewall rule**

Delete a specific firewall rule from a specified firewall. Any virtual machine that has this firewall activated will lose sync with the firewall and will have to be synced again manually. Use this endpoint to remove specific firewall rules.

- **Chemin** : `firewallId`, `ruleId`

#### `POST` `/api/vps/v1/firewall/{firewallId}/sync`
**Sync firewall to all assigned VMs**

Sync a firewall's rules to every virtual machine it's assigned to. Firewall can lose sync with a virtual machine if the firewall has new rules added, removed or updated. Use this endpoint to apply updated firewall rules to all VPS instances assigned to the firewall.

- **Chemin** : `firewallId`

#### `POST` `/api/vps/v1/firewall/{firewallId}/sync/{virtualMachineId}`
**Sync firewall**

Deprecated: use `POST /api/vps/v1/firewall/{firewallId}/sync` instead, which syncs the firewall to all virtual machines assigned to it. Sync a firewall for a specified virtual machine. Firewall can lose sync with virtual machine if the firewall has new rules added, removed or updated. Use this endpoint to apply updated firewall rules to VPS instances.

- **Chemin** : `firewallId`, `virtualMachineId`


## VPS: Malware scanner

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx` | Get scan metrics |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx` | Install Monarx |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx` | Uninstall Monarx |

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx`
**Get scan metrics**

Retrieve scan metrics for the [Monarx](https://www.monarx.com/) malware scanner installed on a specified virtual machine. The scan metrics provide detailed information about malware scans performed by Monarx, including number of scans, detected threats, and other relevant statistics. This information is useful for monitoring security status of the virtual machine and assessing effectiveness of the malware scanner. Us […]

- **Chemin** : `virtualMachineId`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx`
**Install Monarx**

Install the Monarx malware scanner on a specified virtual machine. [Monarx](https://www.monarx.com/) is a security tool designed to detect and prevent malware infections on virtual machines. By installing Monarx, users can enhance the security of their virtual machines, ensuring that they are protected against malicious software. Use this endpoint to enable malware protection on VPS instances.

- **Chemin** : `virtualMachineId`

#### `DELETE` `/api/vps/v1/virtual-machines/{virtualMachineId}/monarx`
**Uninstall Monarx**

Uninstall the Monarx malware scanner on a specified virtual machine. If Monarx is not installed, the request will still be processed without any effect. Use this endpoint to remove malware scanner from VPS instances.

- **Chemin** : `virtualMachineId`


## VPS: OS Templates

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/templates` | Get templates |
| `GET` | `/api/vps/v1/templates/{templateId}` | Get template details |

#### `GET` `/api/vps/v1/templates`
**Get templates**

Retrieve available OS templates for virtual machines. Use this endpoint to view operating system options before creating or recreating VPS instances.

- Aucun paramètre

#### `GET` `/api/vps/v1/templates/{templateId}`
**Get template details**

Retrieve detailed information about a specific OS template for virtual machines. Use this endpoint to view specific template specifications before deployment.

- **Chemin** : `templateId`


## VPS: PTR records

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/ptr/{ipAddressId}` | Create PTR record |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/ptr/{ipAddressId}` | Delete PTR record |

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/ptr/{ipAddressId}`
**Create PTR record**

Create or update a PTR (Pointer) record for a specified virtual machine. Use this endpoint to configure reverse DNS lookup for VPS IP addresses.

- **Chemin** : `virtualMachineId`, `ipAddressId`
- **Corps** (requis) :
    - `domain` — string · **requis** — Pointer record domain

#### `DELETE` `/api/vps/v1/virtual-machines/{virtualMachineId}/ptr/{ipAddressId}`
**Delete PTR record**

Delete a PTR (Pointer) record for a specified virtual machine. Once deleted, reverse DNS lookups to the virtual machine's IP address will no longer return the previously configured hostname. Use this endpoint to remove reverse DNS configuration from VPS instances.

- **Chemin** : `virtualMachineId`, `ipAddressId`


## VPS: Post-install scripts

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/post-install-scripts` | Get post-install scripts |
| `POST` | `/api/vps/v1/post-install-scripts` | Create post-install script |
| `GET` | `/api/vps/v1/post-install-scripts/{postInstallScriptId}` | Get post-install script |
| `PUT` | `/api/vps/v1/post-install-scripts/{postInstallScriptId}` | Update post-install script |
| `DELETE` | `/api/vps/v1/post-install-scripts/{postInstallScriptId}` | Delete post-install script |

#### `GET` `/api/vps/v1/post-install-scripts`
**Get post-install scripts**

Retrieve post-install scripts associated with your account. Use this endpoint to view available automation scripts for VPS deployment.

- **Query** : `page`

#### `POST` `/api/vps/v1/post-install-scripts`
**Create post-install script**

Add a new post-install script to your account, which can then be used after virtual machine installation. The script contents will be saved to the file `/post_install` with executable attribute set and will be executed once virtual machine is installed. The output of the script will be redirected to `/post_install.log`. Maximum script size is 48KB. Use this endpoint to create automation scripts for VPS setup tasks.

- **Corps** (requis) :
    - `name` — string · **requis** — Name of the script
    - `content` — string · **requis** — Content of the script

#### `GET` `/api/vps/v1/post-install-scripts/{postInstallScriptId}`
**Get post-install script**

Retrieve post-install script by its ID. Use this endpoint to view specific automation script details.

- **Chemin** : `postInstallScriptId`

#### `PUT` `/api/vps/v1/post-install-scripts/{postInstallScriptId}`
**Update post-install script**

Update a specific post-install script. Use this endpoint to modify existing automation scripts.

- **Chemin** : `postInstallScriptId`
- **Corps** (requis) :
    - `name` — string · **requis** — Name of the script
    - `content` — string · **requis** — Content of the script

#### `DELETE` `/api/vps/v1/post-install-scripts/{postInstallScriptId}`
**Delete post-install script**

Delete a post-install script from your account. Use this endpoint to remove unused automation scripts.

- **Chemin** : `postInstallScriptId`


## VPS: Public Keys

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/public-keys` | Get public keys |
| `POST` | `/api/vps/v1/public-keys` | Create public key |
| `POST` | `/api/vps/v1/public-keys/attach/{virtualMachineId}` | Attach public key |
| `DELETE` | `/api/vps/v1/public-keys/{publicKeyId}` | Delete public key |

#### `GET` `/api/vps/v1/public-keys`
**Get public keys**

Retrieve public keys associated with your account. Use this endpoint to view available SSH keys for VPS authentication.

- **Query** : `page`

#### `POST` `/api/vps/v1/public-keys`
**Create public key**

Add a new public key to your account. Use this endpoint to register SSH keys for VPS authentication.

- **Corps** (requis) :
    - `name` — string · **requis**
    - `key` — string · **requis**

#### `POST` `/api/vps/v1/public-keys/attach/{virtualMachineId}`
**Attach public key**

Attach existing public keys from your account to a specified virtual machine. Multiple keys can be attached to a single virtual machine. Use this endpoint to enable SSH key authentication for VPS instances.

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `ids` — array<integer> · **requis** — Public Key IDs to attach

#### `DELETE` `/api/vps/v1/public-keys/{publicKeyId}`
**Delete public key**

Delete a public key from your account. **Deleting public key from account does not remove it from virtual machine** Use this endpoint to remove unused SSH keys from account.

- **Chemin** : `publicKeyId`


## VPS: Recovery

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/recovery` | Start recovery mode |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/recovery` | Stop recovery mode |

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/recovery`
**Start recovery mode**

Initiate recovery mode for a specified virtual machine. Recovery mode is a special state that allows users to perform system rescue operations, such as repairing file systems, recovering data, or troubleshooting issues that prevent the virtual machine from booting normally. Virtual machine will boot recovery disk image and original disk image will be mounted in `/mnt` directory. Use this endpoint to enable system res […]

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `root_password` — string · **requis** — Temporary root password for recovery mode

#### `DELETE` `/api/vps/v1/virtual-machines/{virtualMachineId}/recovery`
**Stop recovery mode**

Stop recovery mode for a specified virtual machine. If virtual machine is not in recovery mode, this operation will fail. Use this endpoint to exit system rescue mode and return VPS to normal operation.

- **Chemin** : `virtualMachineId`


## VPS: Snapshots

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot` | Get snapshot |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot` | Create snapshot |
| `DELETE` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot` | Delete snapshot |
| `POST` | `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot/restore` | Restore snapshot |

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot`
**Get snapshot**

Retrieve snapshot for a specified virtual machine. Use this endpoint to view current VPS snapshot information.

- **Chemin** : `virtualMachineId`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot`
**Create snapshot**

Create a snapshot of a specified virtual machine. A snapshot captures the state and data of the virtual machine at a specific point in time, allowing users to restore the virtual machine to that state if needed. This operation is useful for backup purposes, system recovery, and testing changes without affecting the current state of the virtual machine. **Creating new snapshot will overwrite the existing snapshot!** U […]

- **Chemin** : `virtualMachineId`

#### `DELETE` `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot`
**Delete snapshot**

Delete a snapshot of a specified virtual machine. Use this endpoint to remove VPS snapshots.

- **Chemin** : `virtualMachineId`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/snapshot/restore`
**Restore snapshot**

Restore a specified virtual machine to a previous state using a snapshot. Restoring from a snapshot allows users to revert the virtual machine to that state, which is useful for system recovery, undoing changes, or testing. Use this endpoint to revert VPS instances to previous saved states.

- **Chemin** : `virtualMachineId`


## VPS: Virtual machine

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

#### `GET` `/api/vps/v1/virtual-machines`
**Get virtual machines**

Retrieve all available virtual machines. Use this endpoint to view available VPS instances.

- Aucun paramètre

#### `POST` `/api/vps/v1/virtual-machines`
**Purchase new virtual machine**

Purchase and setup a new virtual machine. If virtual machine setup fails for any reason, login to [hPanel](https://hpanel.hostinger.com/) and complete the setup manually. If no payment method is provided, your default payment method will be used automatically. If the response is `202 Accepted`, the payment is still being processed and the virtual machine was not set up. Login to [hPanel](https://hpanel.hostinger.com/ […]

- **Corps** (requis) :
    - `item_id` — string · **requis** — Catalog price item ID
    - `payment_method_id` — integer — Payment method ID, default will be used if not provided
    - `setup` — object · **requis**
    - `coupons` — array<?> — Discount coupon codes

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}`
**Get virtual machine details**

Retrieve detailed information about a specified virtual machine. Use this endpoint to view comprehensive VPS configuration and status.

- **Chemin** : `virtualMachineId`

#### `PUT` `/api/vps/v1/virtual-machines/{virtualMachineId}/hostname`
**Set hostname**

Set hostname for a specified virtual machine. Changing hostname does not update PTR record automatically. If you want your virtual machine to be reachable by a hostname, you need to point your domain A/AAAA records to virtual machine IP as well. Use this endpoint to configure custom hostnames for VPS instances.

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `hostname` — string · **requis**

#### `DELETE` `/api/vps/v1/virtual-machines/{virtualMachineId}/hostname`
**Reset hostname**

Reset hostname and PTR record of a specified virtual machine to default value. Use this endpoint to restore default hostname configuration for VPS instances.

- **Chemin** : `virtualMachineId`

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/metrics`
**Get metrics**

Retrieve historical metrics for a specified virtual machine. It includes the following metrics: - CPU usage - Memory usage - Disk usage - Network usage - Uptime Use this endpoint to monitor VPS performance and resource utilization over time.

- **Chemin** : `virtualMachineId`
- **Query** : `date_from`, `date_to`

#### `PUT` `/api/vps/v1/virtual-machines/{virtualMachineId}/nameservers`
**Set nameservers**

Set nameservers for a specified virtual machine. Be aware, that improper nameserver configuration can lead to the virtual machine being unable to resolve domain names. Use this endpoint to configure custom DNS resolvers for VPS instances.

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `ns1` — string · **requis**
    - `ns2` — string
    - `ns3` — string

#### `PUT` `/api/vps/v1/virtual-machines/{virtualMachineId}/panel-password`
**Set panel password**

Set panel password for a specified virtual machine. If virtual machine does not use panel OS, the request will still be processed without any effect. Requirements for password are same as in the [recreate virtual machine endpoint](/#tag/vps-virtual-machine/POST/api/vps/v1/virtual-machines/{virtualMachineId}/recreate). Use this endpoint to configure control panel access credentials for VPS instances.

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `password` — string · **requis** — Panel password for the virtual machine

#### `GET` `/api/vps/v1/virtual-machines/{virtualMachineId}/public-keys`
**Get attached public keys**

Retrieve public keys attached to a specified virtual machine. Use this endpoint to view SSH keys configured for specific VPS instances.

- **Chemin** : `virtualMachineId`
- **Query** : `page`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/recreate`
**Recreate virtual machine**

Recreate a virtual machine from scratch. The recreation process involves reinstalling the operating system and resetting the virtual machine to its initial state. Snapshots, if there are any, will be deleted. ## Password Requirements Password will be checked against leaked password databases. Requirements for the password are: - At least 12 characters long - At least one uppercase letter - At least one lowercase lett […]

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `template_id` — integer · **requis** — Template ID
    - `password` — string — Root password for the virtual machine. If not provided, random password will be generated. Password will not be shown in
    - `panel_password` — string — Panel password for the panel-based OS template. If not provided, random password will be generated. If OS does not suppo
    - `post_install_script_id` — integer — Post-install script to execute after virtual machine was recreated

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/restart`
**Restart virtual machine**

Restart a specified virtual machine by fully stopping and starting it. If the virtual machine was stopped, it will be started. Use this endpoint to reboot VPS instances.

- **Chemin** : `virtualMachineId`

#### `PUT` `/api/vps/v1/virtual-machines/{virtualMachineId}/root-password`
**Set root password**

Set root password for a specified virtual machine. Requirements for password are same as in the [recreate virtual machine endpoint](/#tag/vps-virtual-machine/POST/api/vps/v1/virtual-machines/{virtualMachineId}/recreate). Use this endpoint to update administrator credentials for VPS instances.

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `password` — string · **requis** — Root password for the virtual machine

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/setup`
**Setup purchased virtual machine**

Setup newly purchased virtual machine with `initial` state. Use this endpoint to configure and initialize purchased VPS instances.

- **Chemin** : `virtualMachineId`
- **Corps** (requis) :
    - `template_id` — integer · **requis** — Template ID
    - `data_center_id` — integer · **requis** — Data center ID
    - `post_install_script_id` — integer — Post-install script ID
    - `password` — string — Password for the virtual machine. If not provided, random password will be generated. Password will not be shown in the 
    - `hostname` — string — Override default hostname of the virtual machine
    - `install_monarx` — boolean — Install Monarx malware scanner (if supported)
    - `enable_backups` — boolean — Enable weekly backup schedule
    - `ns1` — string — Name server 1
    - `ns2` — string — Name server 2
    - `public_key` — object — Use SSH key

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/start`
**Start virtual machine**

Start a specified virtual machine. If the virtual machine is already running, the request will still be processed without any effect. Use this endpoint to power on stopped VPS instances.

- **Chemin** : `virtualMachineId`

#### `POST` `/api/vps/v1/virtual-machines/{virtualMachineId}/stop`
**Stop virtual machine**

Stop a specified virtual machine. If the virtual machine is already stopped, the request will still be processed without any effect. This is a compute-only power state change and does not affect billing. To stop future charges, disable auto-renewal on the owning subscription. Use this endpoint to power off running VPS instances.

- **Chemin** : `virtualMachineId`

