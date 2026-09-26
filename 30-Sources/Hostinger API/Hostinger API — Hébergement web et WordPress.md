---
tags: [hostinger, api, reference]
---

# Hostinger API — Hébergement web et WordPress

Retour à l'index : [[Hostinger API — Index]] · 105 endpoints hébergement mutualisé + 40 Agency + 6 Horizons.

Trois niveaux à ne pas confondre :

| Produit | Préfixe | À quoi ça correspond |
|---|---|---|
| Hosting | `/api/hosting/v1` | Sites sur une offre d'hébergement classique (un compte, plusieurs sites) |
| Agency Hosting | `/api/agency-hosting/v1` | Offres agence — un « plan » contenant plusieurs sites, gérés au niveau du site |
| Horizons | `/api/horizons/v1` | Générateur de sites IA |

## Hosting (`/api/hosting/v1`)

## Hosting: Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/cache/clear` | Clear website cache |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/cache/toggle` | Toggle website cache |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/cacheless-mode/toggle` | Toggle cacheless mode |

#### `DELETE` `/api/hosting/v1/accounts/{username}/websites/{domain}/cache/clear`
**Clear website cache**

Permanently clears all server-side cache for the website at once. Use it when content was updated and needs to be visible immediately, or after making major changes. Also purges the Hostinger CDN cache when CDN is enabled on the website. For a WordPress installation living in a subdirectory, pass the directory query parameter to clear its cache.

- **Chemin** : `username`, `domain`
- **Query** : `directory`

#### `PATCH` `/api/hosting/v1/accounts/{username}/websites/{domain}/cache/toggle`
**Toggle website cache**

Turns server-side caching for the website on or off, based on the enabled flag. Enable it for faster page loads, reduced server load, and improved user experience; recommended for production websites. Disabling may impact performance; to temporarily bypass caching while developing or debugging, prefer toggling cacheless mode instead. Does nothing if caching is already in the requested state.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `enabled` — boolean · **requis** — Turn server-side caching on (true) or off (false) for the website.

#### `PATCH` `/api/hosting/v1/accounts/{username}/websites/{domain}/cacheless-mode/toggle`
**Toggle cacheless mode**

Turns development (cacheless) mode on or off, based on the enabled flag. When enabled, nothing is cached, effectively turning off all caching for the website; use it while actively developing, testing changes, debugging issues, or when real-time updates must be visible. Disable it after finishing development work to restore the performance benefits of caching.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `enabled` — boolean · **requis** — Turn development (cacheless) mode on (true) or off (false) for the website.


## Hosting: Cron Jobs

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/cron-jobs` | List account cron jobs |
| `POST` | `/api/hosting/v1/accounts/{username}/cron-jobs` | Create account cron job |
| `DELETE` | `/api/hosting/v1/accounts/{username}/cron-jobs/{uid}` | Delete account cron job |
| `GET` | `/api/hosting/v1/accounts/{username}/cron-jobs/{uid}/output` | Get cron job output |

#### `GET` `/api/hosting/v1/accounts/{username}/cron-jobs`
**List account cron jobs**

Returns the list of cron jobs configured for the specified account, including their schedule and command.

- **Chemin** : `username`

#### `POST` `/api/hosting/v1/accounts/{username}/cron-jobs`
**Create account cron job**

Creates a cron job for the specified account from a schedule expression and a command. Returns the created cron job, including its uid, which is required to delete the cron job or fetch its output.

- **Chemin** : `username`
- **Corps** (requis) :
    - `time` — string · **requis** — Cron schedule expression (for example "0 2 * * *" runs daily at 02:00).
    - `command` — string · **requis** — Command to execute on the schedule.

#### `DELETE` `/api/hosting/v1/accounts/{username}/cron-jobs/{uid}`
**Delete account cron job**

Permanently deletes the cron job identified by its uid. The uid is returned by the list cron jobs endpoint.

- **Chemin** : `username`, `uid`

#### `GET` `/api/hosting/v1/accounts/{username}/cron-jobs/{uid}/output`
**Get cron job output**

Returns the output captured from the last execution of the cron job identified by its uid. The uid is returned by the list cron jobs endpoint.

- **Chemin** : `username`, `uid`


## Hosting: Databases

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

#### `GET` `/api/hosting/v1/accounts/{username}/databases`
**List account databases**

Returns a paginated list of databases for the specified account. Use the domain and is_assigned filters to find databases assigned to a specific domain.

- **Chemin** : `username`
- **Query** : `page`, `per_page`, `domain`, `is_assigned`, `search`

#### `POST` `/api/hosting/v1/accounts/{username}/databases`
**Create account database**

Creates a database with a database user and password for the specified account. The database name and user are automatically prefixed with the account username when needed.

- **Chemin** : `username`
- **Corps** (requis) :
    - `name` — string · **requis** — Database name. If the account username prefix is omitted, it is added automatically.
    - `user` — string · **requis** — Database user. If the account username prefix is omitted, it is added automatically.
    - `password` — string · **requis** — Database user password.
    - `website_domain` — string · **requis** — Website domain assigned to the database.

#### `GET` `/api/hosting/v1/accounts/{username}/databases/remote-connections`
**List database remote connections**

Returns the remote-access rules for the specified account: the remote hosts (IPv4/IPv6 addresses, or "%" for any host) allowed to connect to the account databases. Use the domain filter to only return rules for databases assigned to a specific domain.

- **Chemin** : `username`
- **Query** : `domain`

#### `DELETE` `/api/hosting/v1/accounts/{username}/databases/{name}`
**Delete account database**

Permanently deletes a database and its remote connections. The database name must be the full name returned by the list databases endpoint.

- **Chemin** : `username`, `name`

#### `PATCH` `/api/hosting/v1/accounts/{username}/databases/{name}/change-password`
**Change database password**

Changes the password for the specified database user. The database name must be the full name returned by the list databases endpoint. The password must also be updated in any website configuration that uses this database.

- **Chemin** : `username`, `name`
- **Corps** (requis) :
    - `password` — string · **requis** — New database user password.

#### `GET` `/api/hosting/v1/accounts/{username}/databases/{name}/phpmyadmin-link`
**Get phpMyAdmin link**

Returns a direct sign-on link to phpMyAdmin for the specified database. Use this when a visual database interface is needed for SQL queries, imports, exports, or table management. The database name must be the full name returned by the list databases endpoint.

- **Chemin** : `username`, `name`

#### `POST` `/api/hosting/v1/accounts/{username}/databases/{name}/remote-connections`
**Create database remote connection**

Allows a remote host to connect to the specified database. Provide an IPv4/IPv6 address, or "%" to allow any host. The database name must be the full name returned by the list databases endpoint.

- **Chemin** : `username`, `name`
- **Corps** (requis) :
    - `ip` — string · **requis** — Remote host to allow: an IPv4/IPv6 address, or "%" for any host.

#### `DELETE` `/api/hosting/v1/accounts/{username}/databases/{name}/remote-connections`
**Delete database remote connection**

Permanently removes a remote-access rule, revoking the given host's remote access to the database. Identify the rule with the required ip query parameter (the IPv4/IPv6 address, or "%", exactly as returned by the list remote connections endpoint). The database name must be the full name returned by the list databases endpoint.

- **Chemin** : `username`, `name`
- **Query** : `ip`

#### `PATCH` `/api/hosting/v1/accounts/{username}/databases/{name}/repair`
**Repair database**

Repairs corrupted database tables asynchronously. Use when database errors, crashes, or corruption are reported. The database name must be the full name returned by the list databases endpoint.

- **Chemin** : `username`, `name`

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/databases/setup`
**Setup website database**

Creates a new MySQL database for the website and writes its connection details into the website's environment variables, then restarts the application. The platform generates the password (and the database name and user, unless supplied). The password is never returned; the application reads it from the environment. Written variables: `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD` and `DATABASE_URL` (`mysq […]

- **Chemin** : `username`, `domain`
- **Corps** (optionnel) :
    - `name` — string — Optional database name. Generated when omitted. Letters, digits and underscores; must not start with an underscore. Up t
    - `user` — string — Optional database user. Generated when omitted. Letters, digits and underscores; must not start with an underscore. Up t


## Hosting: Datacenters

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/datacenters` | List available datacenters |

#### `GET` `/api/hosting/v1/datacenters`
**List available datacenters**

Retrieve a list of datacenters available for setting up hosting plans based on available datacenter capacity and hosting plan of your order. The first item in the list is the best match for your specific order requirements.

- **Query** : `order_id`


## Hosting: Domains

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

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/parked-domains`
**List website parked domains**

Retrieve all parked or alias domains created under the selected website. Use this endpoint to inspect parked domain configuration for a specific website, including the parent domain and root directory assigned to each parked domain.

- **Chemin** : `username`, `domain`

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/parked-domains`
**Create website parked domain**

Create a parked or alias domain for the selected website. Provide a domain name or IP address to park on the website so it serves the same content as the parent domain.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `parked_domain` — string · **requis** — Domain name or IP address to park on the selected website

#### `DELETE` `/api/hosting/v1/accounts/{username}/websites/{domain}/parked-domains/{parkedDomain}`
**Delete website parked domain**

Delete an existing parked or alias domain from the selected website. Use this endpoint to remove parked domains that are no longer needed.

- **Chemin** : `username`, `domain`, `parkedDomain`

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/subdomains`
**List website subdomains**

Retrieve all subdomains created under the selected website. Use this endpoint to inspect subdomain configuration for a specific website, including the parent domain and root directory assigned to each subdomain.

- **Chemin** : `username`, `domain`

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/subdomains`
**Create website subdomain**

Create a new subdomain for the selected website. Provide a subdomain prefix and, optionally, a custom directory or the website public directory to use as the subdomain root.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `subdomain` — string · **requis** — Subdomain prefix to create under the selected website
    - `directory` — string — Directory name for the subdomain relative to the website root
    - `is_using_public_directory` — boolean — Use the website public directory as the subdomain root directory

#### `DELETE` `/api/hosting/v1/accounts/{username}/websites/{domain}/subdomains/{subdomain}`
**Delete website subdomain**

Delete an existing subdomain from the selected website. Use this endpoint to remove subdomains that are no longer needed.

- **Chemin** : `username`, `domain`, `subdomain`

#### `POST` `/api/hosting/v1/domains/free-subdomains`
**Generate a free subdomain**

Generate a unique free subdomain that can be used for hosting services without purchasing custom domains. Free subdomains allow you to start using hosting services immediately and you can always connect a custom domain to your site later.

- Aucun paramètre

#### `POST` `/api/hosting/v1/domains/verify-ownership`
**Verify domain ownership**

Verify ownership of a single domain and return the verification status. Use this endpoint to check if a domain is accessible for you before using it for new websites. If the domain is accessible, the response will have `is_accessible: true`. If not, add the given TXT record to your domain's DNS records and try verifying again. Keep in mind that it may take up to 10 minutes for new TXT DNS records to propagate. Skip t […]

- **Corps** (requis) :
    - `domain` — string · **requis** — Domain to verify ownership for


## Hosting: Files

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/domains/{domain}/files` | List website files and directories |
| `GET` | `/api/hosting/v1/accounts/{username}/domains/{domain}/files/content` | Get website file content |
| `POST` | `/api/hosting/v1/files/upload-urls` | Generate upload URL |

#### `GET` `/api/hosting/v1/accounts/{username}/domains/{domain}/files`
**List website files and directories**

List files and directories under a website's document root. Use `directory` to browse a subdirectory relative to the document root. Symlinked entries are listed but never traversed into or resolved.

- **Chemin** : `username`, `domain`
- **Query** : `directory`, `max_depth`, `max_items`, `offset`, `file_types`

#### `GET` `/api/hosting/v1/accounts/{username}/domains/{domain}/files/content`
**Get website file content**

Get a single file's content, relative to a website's document root. Read-only; refuses symlinks, oversized files, non-text file types, and files identified as containing secrets (e.g. credential files) — none of these are returned by this endpoint.

- **Chemin** : `username`, `domain`
- **Query** : `path`, `from_line`, `max_lines`

#### `POST` `/api/hosting/v1/files/upload-urls`
**Generate upload URL**

Generate a file browser upload URL with authentication credentials for uploading files directly to a website's file storage. Returns `url`, `auth_key` and `rest_auth_key`. Use these to upload a file to the website's `public_html` directory via the TUS resumable upload protocol (TUS 1.0.0). Send `X-Auth: {auth_key}` and `X-Auth-Rest: {rest_auth_key}` headers on every request below. 1. Create the upload: `POST` to `{ur […]

- **Corps** (requis) :
    - `username` — string · **requis** — Account username
    - `domain` — string · **requis** — Website domain


## Hosting: Git

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings` | Get Git auto-deployment settings |
| `PUT` | `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings` | Update Git auto-deployment settings |
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings` | Delete Git auto-deployment settings |
| `GET` | `/api/hosting/v1/git/installations` | List Git installations |
| `GET` | `/api/hosting/v1/git/installations/{uuid}/repositories` | List Git installation repositories |

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings`
**Get Git auto-deployment settings**

Returns the Git auto-deployment settings of the website: which repository and branch deploy into which directory, and whether pushes trigger a deployment. `is_enabled` false keeps the repository link but ignores pushes. When the website has no auto-deployment configured every field is null. Save settings with `Update Git auto-deployment settings`.

- **Chemin** : `username`, `domain`

#### `PUT` `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings`
**Update Git auto-deployment settings**

Creates or replaces the Git auto-deployment settings of the website: repository, branch, the directory under the document root to deploy into, and `is_enabled`. Send the full set; `is_enabled` defaults to true and `directory` to the document root. `installation_uuid` must be an installation from `List Git installations` that belongs to the same customer as the website. For PHP and static websites, saving with `is_ena […]

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `installation_uuid` — string · **requis** — Active Git installation from `List Git installations`
    - `owner` — string · **requis** — Repository owner login, as returned by `List Git installation repositories`. GitLab group paths use slashes.
    - `repository` — string · **requis** — Repository name without the .git suffix
    - `branch` — string · **requis** — Branch to deploy
    - `directory` — string — Subdirectory under the website document root to deploy into. Empty, null or omitted means the document root.
    - `is_enabled` — boolean — Whether pushes to the branch deploy automatically

#### `DELETE` `/api/hosting/v1/accounts/{username}/websites/{domain}/git/auto-deployments/settings`
**Delete Git auto-deployment settings**

Removes the Git auto-deployment settings of the website. Files already deployed stay on the website; pushes stop deploying until settings are saved again. Succeeds also when nothing is configured.

- **Chemin** : `username`, `domain`

#### `GET` `/api/hosting/v1/git/installations`
**List Git installations**

Lists the Git provider accounts the customer has connected. Only installations with status `active` are returned unless the `status` filter says otherwise. An empty list means the customer has no active installation. Check `status=suspended` and `status=pending` as well. If there is none at all, a Git provider (GitHub or GitLab) has to be connected once in hPanel (Websites, Manage, Advanced, Git; or Add Website, Node […]

- **Query** : `provider`, `status`

#### `GET` `/api/hosting/v1/git/installations/{uuid}/repositories`
**List Git installation repositories**

Lists the repositories the Git installation can access, read live from the provider. Works for github and gitlab installations. Use an active installation: a suspended or pending one is still queried and the call fails with whatever the provider answers. The list is cut at the first 500 repositories in the order the provider returns them; when the account has more, name the repository directly instead of searching th […]

- **Chemin** : `uuid`


## Hosting: NodeJS

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

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds`
**List NodeJS builds**

Retrieve a paginated list of Node.js build processes for a specific website. Each build represents a single run of the Node.js build pipeline. Use the `states` query parameter to filter results by build state (pending, running, completed, failed). Use the `uuid` from a build to poll its output via the `Get Node.js Build Logs` endpoint.

- **Chemin** : `username`, `domain`
- **Query** : `page`, `per_page`, `states`

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds`
**Start Node.js build**

Start a Node.js build process using files already present on the website's file storage. WARNING: on success this overwrites the website's existing contents and cannot be undone — verify this is intended before calling this endpoint. With `source_type` `archive`, `source_options.archive_path` must point to an existing archive file on the server (relative to the website document root). Use the `Generate Upload URL` en […]

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `node_version` — enum · **requis** — Node.js version
    - `app_type` — enum · **requis** — Node.js application type
    - `root_directory` — string · **requis** — Application root directory (where package.json is located) relative to public_html
    - `output_directory` — string · **requis** — Build output directory relative to the root directory
    - `build_script` — string · **requis** — Build script that will be ran to build the application
    - `entry_file` — string — The main entry point file for the application
    - `package_manager` — enum — Package manager
    - `source_type` — enum · **requis** — Where the files come from: `archive` (an uploaded archive on the website) or `git` (a branch of a repository reachable t
    - `source_options` — object · **requis** — Source-specific options. For `archive` send `archive_path`. For `git` send `owner`, `repository`, `branch` and `installa

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings`
**Get Node.js build settings**

Returns the build settings stored for the website: framework (`app_type`), Node.js version, root and output directory, build script, entry file and package manager. Stored settings drive Git auto-deployment builds. A build started through the API uses the values sent in that request and saves them here only when no settings exist yet. Returns 404 until the first build or the first settings update stores them. Use thi […]

- **Chemin** : `username`, `domain`

#### `PUT` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings`
**Update Node.js build settings**

Replaces the build settings stored for the website. Send the full set: `node_version` is required and every nullable field you omit is stored as null. Creates the settings when none exist yet. This does not start a build. Stored settings drive Git auto-deployment builds; a build started through the API uses the values sent in that request, so to rebuild with corrected settings call `Start Node.js build` with the same […]

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `node_version` — enum · **requis** — Node.js major version
    - `app_type` — enum — Node.js application framework. Set it explicitly when auto-detection picked the wrong one.
    - `root_directory` — string — Application root directory (where package.json is located) relative to public_html. Omit it, or send ".", for public_htm
    - `output_directory` — string — Build output directory relative to the root directory
    - `build_script` — string — The package.json script that builds the application
    - `entry_file` — string — The main entry point file for the application (required for express, fastify, nest, nuxt and hono app types)
    - `package_manager` — enum — Package manager used to install dependencies

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings/env`
**List Node.js environment variables**

Lists the Node.js environment variables currently set for the website. Values are always masked as `********` and cannot be read back through this API. Use this endpoint to see which keys are configured or to verify a change, not to read values. To change variables, use the `Replace Node.js environment variables` endpoint. It replaces the whole set, so never copy the masked values from this response into that request […]

- **Chemin** : `username`, `domain`

#### `PUT` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings/env`
**Replace Node.js environment variables**

Replaces the website's Node.js environment variables with the ones provided. This is a full replace: any variable not in the request is deleted, and sending an empty `env_vars` array deletes every variable. Saving writes the values and restarts the running Node.js process. A restart is enough for apps that read environment variables at process start, such as Express or NestJS. It is not enough for frameworks that bak […]

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `env_vars` — array<object> · **requis** — Environment variables to set. This is the full desired set: any variable not in this list is deleted, and an empty array

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/settings/from-archive`
**Get Node.js build settings from archive**

Auto-detect Node.js build settings from a package.json inside an archive already on the server. Use this before calling `Start Node.js Build` to preview what settings will be used, or to let the user review and override values (framework, node version, root directory, output directory, build script) before committing to a build. The archive must already be present on the website's file storage. Use the `Generate Uplo […]

- **Chemin** : `username`, `domain`
- **Query** : `archive_path`

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/{uuid}`
**Get Node.js build details**

Returns one build by UUID: its state (`pending`, `running`, `completed`, `failed`), the options it ran with and timestamps. Poll this while a build is pending or running. When it is failed, read `Get NodeJS build logs` and `Analyse failed Node.js build` for the cause. Returns 404 when the UUID does not belong to a build of this website.

- **Chemin** : `username`, `domain`, `uuid`

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/{uuid}/analysis`
**Analyse failed Node.js build**

Returns an AI analysis of why a build failed and how to fix it, based on the build logs, the project file list and package.json. Only builds in the `failed` state can be analysed; any other state returns 422. When no analysis could be produced both `analysis` and `solution` are null, in which case read `Get NodeJS build logs` instead. Each call runs the analysis again, so call it once per failed build and keep the re […]

- **Chemin** : `username`, `domain`, `uuid`

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/builds/{uuid}/logs`
**Get NodeJS build logs**

Retrieve logs from a specific Node.js build process. To stream live output while a build is running, poll this endpoint repeatedly while the build state is `running`, passing the previously returned `lines` count as `from_line` to fetch only new output since the last call. Log content may contain ANSI escape sequences (color codes).

- **Chemin** : `username`, `domain`, `uuid`
- **Query** : `from_line`

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/runtime-logs`
**Get Node.js runtime logs**

Returns the Node.js application's runtime console log entries, oldest first, each with timestamp, level and message. On the first call send `period` (`1h`, `1d`, `1w` or `1m`) and optionally `levels` and `limit` (1-5000, default 1000); when more entries match than `limit`, the newest are kept. To poll for new entries send `total_lines + 1` from the previous response as `from_line` and omit `period`; `period` and `fro […]

- **Chemin** : `username`, `domain`
- **Query** : `period`, `from_line`, `limit`, `levels`

#### `DELETE` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/runtime-logs`
**Clear Node.js runtime logs**

Empties the Node.js application's runtime log file. This cannot be undone, so confirm with the user before calling it. Returns success even when no log file exists yet. Use it before reproducing a problem so the next `Get Node.js runtime logs` call returns only fresh entries; start that call with `period` again instead of reusing a `from_line` from before the clear.

- **Chemin** : `username`, `domain`

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/server/restart`
**Restart Node.js application**

Restarts the Node.js server process for the website. Does not rebuild or redeploy the application. Use it to apply environment or configuration changes, or to recover a hung application. Only applicable to server-side applications (Express, Next.js, NestJS, etc.). Static front-end apps (React, Vue, Vite) have no persistent server process, so restarting them has no effect. Returns success even when the website has no  […]

- **Chemin** : `username`, `domain`

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/vulnerabilities`
**List Node.js vulnerabilities**

Lists known npm package vulnerabilities detected on a Node.js website, enriched with advisory metadata (severity, CVSS score, CVE, advisory URL). Results are sorted from the most severe to the least severe, then by publish date (newest first). Use the `severities` query parameter to filter. Vulnerabilities with `is_patchable` set to `true` can be auto-fixed via the `Patch Node.js Vulnerabilities` endpoint, which open […]

- **Chemin** : `username`, `domain`
- **Query** : `severities`

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/nodejs/vulnerabilities/patch`
**Patch Node.js vulnerabilities**

Patches the selected Node.js vulnerabilities by updating the affected package versions in `package.json` and opening a GitHub pull request in the connected repository. The customer reviews and merges the pull request; merging triggers the automatic deployment. Auto-fix is only available for websites deployed from a connected GitHub repository. Websites deployed from an archive have no auto-fix path and return a 404.  […]

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `vulnerability_ids` — array<string> · **requis** — List of vulnerability IDs to patch, as returned by the list vulnerabilities endpoint.


## Hosting: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/orders` | List orders |

#### `GET` `/api/hosting/v1/orders`
**List orders**

Retrieve a paginated list of orders accessible to the authenticated client. This endpoint returns orders of your hosting accounts as well as orders of other client hosting accounts that have shared access with you. Use the available query parameters to filter results by order statuses or specific order IDs for more targeted results.

- **Query** : `page`, `per_page`, `statuses`, `order_ids`


## Hosting: PHP

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/details` | Get PHP details |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/extensions` | Update PHP extensions |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/extensions/reset` | Reset PHP extensions |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/options` | Update PHP options |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/php-info` | Get PHP info |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/php/version` | Update PHP version |

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/php/details`
**Get PHP details**

Returns the full PHP configuration for the website: current version, available versions (supported and unsupported), enabled/disabled extensions, options with their current value, default, type and the plan limit (`max`), and conflicting extension groups. Use it to check the current PHP setup before updating the version, extensions or options.

- **Chemin** : `username`, `domain`

#### `PATCH` `/api/hosting/v1/accounts/{username}/websites/{domain}/php/extensions`
**Update PHP extensions**

Enables or disables PHP extensions (modules) for the website. Use the Get PHP details endpoint to check the current extension states before changing them.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `enable` — array<string> — PHP extensions to enable.
    - `disable` — array<string> — PHP extensions to disable.

#### `PATCH` `/api/hosting/v1/accounts/{username}/websites/{domain}/php/extensions/reset`
**Reset PHP extensions**

Resets all PHP extensions of the website to their default state. Use it to recover from extension conflicts or restore the original configuration.

- **Chemin** : `username`, `domain`

#### `PATCH` `/api/hosting/v1/accounts/{username}/websites/{domain}/php/options`
**Update PHP options**

Updates PHP options for the website (e.g. `memory_limit`, `max_execution_time`, `upload_max_filesize`). Only provide the options you want to change, inside the `options` object. Values above the account plan limit are silently capped to that limit, so the request can succeed with a smaller applied value. Call the Get PHP details endpoint afterwards to read the applied value.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `options` — object · **requis** — Map of PHP options to update, keyed by option name. Only include options you want to change.

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/php/php-info`
**Get PHP info**

Returns the full phpinfo page (HTML) for the website. Use it to debug PHP issues or inspect the complete PHP environment of the website.

- **Chemin** : `username`, `domain`

#### `PATCH` `/api/hosting/v1/accounts/{username}/websites/{domain}/php/version`
**Update PHP version**

Changes the PHP version of the website. Use the Get PHP details endpoint to see the versions available for the website.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `version` — string · **requis** — PHP version to switch the website to.


## Hosting: Redirects

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects` | List website redirects |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects` | Create website redirect |
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects` | Delete website redirect |

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects`
**List website redirects**

Returns a paginated list of redirects configured for the selected website.

- **Chemin** : `username`, `domain`
- **Query** : `page`, `per_page`

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects`
**Create website redirect**

Creates a redirect from a URL on the selected website to another URL or IP address.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `from` — string · **requis** — Source URL on the selected website
    - `to` — string · **requis** — Destination URL or IP address

#### `DELETE` `/api/hosting/v1/accounts/{username}/websites/{domain}/redirects`
**Delete website redirect**

Permanently deletes the redirect identified by its source URL. Pass the `from` value exactly as returned by the list redirects endpoint.

- **Chemin** : `username`, `domain`
- **Query** : `from`


## Hosting: SSL

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl` | Uninstall SSL |
| `PATCH` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/https-redirect/toggle` | Toggle HTTPS redirect |
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/setup` | Install SSL |
| `GET` | `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/status` | Get SSL status |

#### `DELETE` `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl`
**Uninstall SSL**

Removes the SSL certificate assigned to the website, turns the HTTPS redirect off and cancels a pending installation retry. The website serves plain HTTP until a new installation completes. `Get SSL status` reports `not_installed` as soon as the call returns; the call also succeeds when no certificate is assigned, so repeating it is safe. Returns 422 for free subdomains (their certificate is managed by the platform)  […]

- **Chemin** : `username`, `domain`

#### `PATCH` `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/https-redirect/toggle`
**Toggle HTTPS redirect**

Turns the HTTP to HTTPS redirect of the website on or off, based on `is_enabled`. Does nothing when the redirect is already in the requested state. Turning it on requires an installed certificate (`status` `active` or `expired` on `Get SSL status`) and returns 422 when there is none; turning it off is always accepted.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `is_enabled` — boolean · **requis** — Turn the HTTP to HTTPS redirect on (true) or off (false) for the website.

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/setup`
**Install SSL**

Requests a lifetime SSL certificate for the website. The installation runs in the background; `Get SSL status` reports `active` or `failed` when it ends. An `active` lifetime certificate does not block the request: a new installation is requested, which is how a certificate is reinstalled. Returns 422 for free subdomains (their certificate is managed by the platform), while an installation is `installing` or `waiting […]

- **Chemin** : `username`, `domain`

#### `GET` `/api/hosting/v1/accounts/{username}/websites/{domain}/ssl/status`
**Get SSL status**

Returns the SSL state of the website: the certificate `status` and `provider`, whether the certificate is a lifetime one managed by the platform, whether HTTP requests are redirected to HTTPS, when the certificate stops being valid and the last installation error. `installing` and `waiting_for_retry` mean an installation is in progress. `failed` means the last installation gave up, or the website was not updated for  […]

- **Chemin** : `username`, `domain`


## Hosting: Websites

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/deploy` | Deploy static site archive |
| `GET` | `/api/hosting/v1/websites` | List websites |
| `POST` | `/api/hosting/v1/websites` | Create website |
| `DELETE` | `/api/hosting/v1/websites/{domain}` | Delete website |

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/deploy`
**Deploy static site archive**

Deploy a static application from an archive file. WARNING: this overwrites the website's existing contents and cannot be undone — verify this is intended before calling this endpoint. This endpoint allows you to deploy a static application from an archive file that has been uploaded to the website's directory. This only works for static sites (pre-built HTML/CSS/JS with no build step). For Node.js applications, use ` […]

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `archive_path` — string · **requis** — Relative path to the archive file from website root directory

#### `GET` `/api/hosting/v1/websites`
**List websites**

Retrieve a paginated list of websites (CloudLinux, Builder, and Horizons) accessible to the authenticated client. This endpoint returns websites from your hosting accounts as well as websites from other client hosting accounts that have shared access with you. Each website includes a `website_type` field describing the type of website detected on the underlying platform (`wordpress`, `builder`, `horizons`, `nodejs`,  […]

- **Query** : `page`, `per_page`, `username`, `order_id`, `is_enabled`, `domain`, `website_types`

#### `POST` `/api/hosting/v1/websites`
**Create website**

Create a new website for the authenticated client. You must choose which hosting order to create this website on. Pass that order as `order_id` together with the domain name. List orders to see available IDs; the website is provisioned on that order's hosting plan. The datacenter_code parameter is required when creating the first website on a new hosting plan - this will set up and configure new hosting account in th […]

- **Corps** (requis) :
    - `domain` — string · **requis** — Domain name for the website. Cannot start with "www."
    - `order_id` — integer · **requis** — Hosting order ID to create this website on. Choose the order whose hosting plan should host the new website. List orders
    - `datacenter_code` — string — Datacenter code. This parameter is required when creating the first website on a new hosting plan.

#### `DELETE` `/api/hosting/v1/websites/{domain}`
**Delete website**

This endpoint permanently removes a website and all of its data. This action cannot be undone. Before calling it, make sure the user understands the consequences and explicitly confirms that they want to proceed. All website files, databases and related configuration will be removed. The hosting plan itself is kept, so a new website can be created on it afterwards. Supported websites: main and addon domain websites o […]

- **Chemin** : `domain`


## WordPress: AI Tools

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/hostinger-plugins/ai-option/status` | Show AI option status |
| `PATCH` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/hostinger-plugins/ai-option/status` | Set AI option status |

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/hostinger-plugins/ai-option/status`
**Show AI option status**

Show the current AI option status for the Hostinger Tools plugin on the specified WordPress installation. Filter by `option` to return a single option, or omit it to return all options. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`
- **Query** : `option`

#### `PATCH` `/api/hosting/v1/accounts/{username}/wordpress/{software}/hostinger-plugins/ai-option/status`
**Set AI option status**

Enable or disable an AI option for the Hostinger Tools plugin on the specified WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `option` — enum · **requis** — AI option name
    - `enable` — boolean · **requis** — Enable (true) or disable (false) the AI option.


## WordPress: Installations

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

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/wordpress/import`
**Import WordPress website**

Import WordPress website to the specified domain. WARNING: this overwrites the website's existing contents and cannot be undone — verify this is intended before calling this endpoint. This endpoint allows you to import a WordPress website from archive and database files that have been uploaded to the website's directory.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `archive_path` — string · **requis** — Path to the WordPress archive file (relative to website root)
    - `sql_path` — string · **requis** — Path to the database SQL file (relative to website root)

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/installations`
**Install WordPress**

Install WordPress on an existing website. The website must already exist before calling this endpoint. To create a new website first, use POST /api/hosting/v1/websites and poll GET /api/hosting/v1/websites until it appears. Call GET /api/hosting/v1/wordpress/installations filtered by username and domain before proceeding to check whether WordPress is already installed on the target domain/path. If WordPress already e […]

- **Chemin** : `username`
- **Corps** (requis) :
    - `domain` — string · **requis** — Domain of the existing website where WordPress will be installed
    - `site_title` — string · **requis** — Title of the WordPress site
    - `language` — string — WordPress locale. Defaults to en_US when omitted.
    - `directory` — string — Relative directory to install WordPress into. Defaults to the website root when omitted.
    - `overwrite` — boolean — When false (default), does not replace an existing installation. If WordPress is already installed on the domain/path, t
    - `auto_updates` — enum — WordPress core auto-update policy
    - `version` — string — WordPress core version to install. If omitted, the latest core version compatible with the account vhost PHP version is 
    - `credentials` — object · **requis** — WordPress admin credentials
    - `database` — object — Optional. If the named database already exists, it will be used for this WordPress install. Otherwise a new database is 

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/installations/check-is-valid`
**Check if WordPress installations are valid**

Check whether one or more WordPress installations are valid and working correctly. Detects broken installations caused by missing files, broken plugins, themes and similar issues. Provide the WordPress installation (software) identifiers in the body. They can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`
- **Corps** (requis) :
    - `software_ids` — array<string> · **requis** — WordPress installation (software) identifiers to validate.
    - `force` — boolean — Force fresh validation without cache. Preferable for troubleshooting purposes.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/installations/detect`
**Detect WordPress installations**

Trigger a background scan to detect WordPress installations for the account. This operation is asynchronous: a successful response only means the scan has been queued. Poll GET /api/hosting/v1/wordpress/installations to fetch the detected installations once the scan completes.

- **Chemin** : `username`

#### `DELETE` `/api/hosting/v1/accounts/{username}/wordpress/{software}`
**Delete WordPress installation**

Delete the specified WordPress installation, with optional file and database removal. This removes all associated components including plugins, themes, staging websites and any other related data. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `delete_files` — boolean — Delete installation files from disk.
    - `delete_database` — boolean — Delete the installation database.

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/jwt-token`
**Get installation JWT token**

Return a JWT token used to authenticate requests against the specified WordPress installation, including its MCP (Model Context Protocol) endpoint. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/update`
**Update WordPress core**

Update the WordPress core for the specified installation (minor update or a specific version). Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the update job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `minor` — boolean — Update the minor version only.
    - `version` — string — Update to a specific WordPress core version.

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/updates`
**List available WordPress core updates**

List available WordPress core updates for the specified installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/version`
**Show WordPress core version**

Show the WordPress core version for the specified installation, along with known vulnerabilities affecting it. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `GET` `/api/hosting/v1/wordpress/installations`
**List WordPress installations**

List WordPress installations accessible to the authenticated client. Use this endpoint to discover existing WordPress installations and to poll for installation status after calling the install endpoint. When a newly requested installation appears in this list, WordPress is ready. Filter by username and domain to narrow results to a specific website. Each installation includes a `valid` flag and, when invalid, a `val […]

- **Query** : `username`, `domain`, `ownership`


## WordPress: LiteSpeed Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/litespeed-cache/purge` | Purge LiteSpeed Cache |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/litespeed-cache/status` | Show LiteSpeed Cache status |

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/litespeed-cache/purge`
**Purge LiteSpeed Cache**

Purge the LiteSpeed Cache for the specified WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/litespeed-cache/status`
**Show LiteSpeed Cache status**

Show the LiteSpeed Cache status for the specified WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`


## WordPress: Login

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/login/links` | Create login links |

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/login/links`
**Create login links**

Create temporary auto-login links for the specified WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`


## WordPress: Maintenance

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/maintenance/status` | Show maintenance status |
| `PATCH` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/maintenance/toggle` | Toggle maintenance mode |

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/maintenance/status`
**Show maintenance status**

Show the maintenance mode status for the specified WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `PATCH` `/api/hosting/v1/accounts/{username}/wordpress/{software}/maintenance/toggle`
**Toggle maintenance mode**

Enable or disable maintenance mode for the specified WordPress installation, based on the `enabled` flag. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `enabled` — boolean · **requis** — Enable (true) or disable (false) maintenance mode for the WordPress installation.


## WordPress: Object Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/memcached/status` | Show Memcached object cache status |
| `PATCH` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/memcached/toggle` | Toggle Memcached object cache |

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/memcached/status`
**Show Memcached object cache status**

Show the Memcached object cache status for the specified WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `PATCH` `/api/hosting/v1/accounts/{username}/wordpress/{software}/memcached/toggle`
**Toggle Memcached object cache**

Activate or deactivate the Memcached object cache for the specified WordPress installation, based on the `enabled` flag. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `enabled` — boolean · **requis** — Activate (true) or deactivate (false) the Memcached object cache for the WordPress installation.


## WordPress: Plugins

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

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/wordpress/plugins/deploy`
**Deploy WordPress plugin**

Deploy a WordPress plugin from an already uploaded directory. This endpoint allows you to deploy a WordPress plugin that has been uploaded to the website's directory. The plugin will be activated and made available in the WordPress admin panel.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `slug` — string · **requis** — Slug of the plugin
    - `plugin_path` — string · **requis** — Relative path to the plugin directory from wp-content/plugins

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins`
**List installed WordPress plugins**

List plugins installed on a WordPress installation, including their status, available updates and known vulnerabilities. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`
- **Query** : `category`

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/activate`
**Activate WordPress plugin**

Activate an installed plugin on a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the activation job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `plugin` — string · **requis** — Slug of the installed plugin to activate.

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/available`
**List available WordPress plugins**

List plugins recommended for installation on a WordPress installation that are not yet installed. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/deactivate`
**Deactivate WordPress plugin**

Deactivate an installed plugin on a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the deactivation job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `plugin` — string · **requis** — Slug of the installed plugin to deactivate.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/hostinger/update`
**Update Hostinger WordPress plugin**

Update a Hostinger plugin to its latest version on a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the update job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `slug` — enum · **requis** — Slug of the Hostinger plugin to update to its latest version.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/install`
**Install WordPress plugins**

Install one or more plugins on an existing WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). Use GET /api/hosting/v1/wordpress/plugins to discover the plugin slugs available for installation. This operation is asynchronous: a successful response only means the install job has been queued,  […]

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `plugins` — array<string> · **requis** — Plugin slugs to install. Use GET /api/hosting/v1/wordpress/plugins to discover available slugs.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/uninstall`
**Uninstall WordPress plugins**

Uninstall one or more plugins from a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the uninstall job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `plugins` — array<string> · **requis** — Slugs of the installed plugins to uninstall.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/update`
**Update WordPress plugins**

Update one or more installed plugins to their latest version on a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the update job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `plugins` — array<string> · **requis** — Slugs of the installed plugins to update to their latest version.

#### `GET` `/api/hosting/v1/wordpress/plugins`
**Search WordPress plugins**

Search the WordPress.org plugin directory for plugins available to install. Use the returned `slug` values with POST /api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/install.

- **Query** : `search`

#### `GET` `/api/hosting/v1/wordpress/plugins/is-woocommerce-installed`
**Check if WooCommerce is installed**

Check whether WooCommerce is installed on any WordPress installation of a domain. Optionally filter by domain to scope the check.

- **Query** : `domain`

#### `GET` `/api/hosting/v1/wordpress/plugins/suggested`
**List suggested WordPress plugins**

List curated plugin suggestions grouped by website type. Use the returned `slug` values with POST /api/hosting/v1/accounts/{username}/wordpress/{software}/plugins/install.

- **Query** : `order_id`


## WordPress: Themes

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/hosting/v1/accounts/{username}/websites/{domain}/wordpress/themes/deploy` | Deploy WordPress theme |
| `GET` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes` | List installed WordPress themes |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/activate` | Activate WordPress theme |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/install` | Install WordPress theme |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/uninstall` | Uninstall WordPress themes |
| `POST` | `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/update` | Update WordPress themes |
| `GET` | `/api/hosting/v1/wordpress/themes` | List WordPress themes |

#### `POST` `/api/hosting/v1/accounts/{username}/websites/{domain}/wordpress/themes/deploy`
**Deploy WordPress theme**

Deploy a WordPress theme from an already uploaded directory. This endpoint allows you to deploy a WordPress theme that has been uploaded to the website's directory. The theme can be optionally activated after deployment.

- **Chemin** : `username`, `domain`
- **Corps** (requis) :
    - `slug` — string · **requis** — Slug of the theme
    - `theme_path` — string · **requis** — Relative path to the theme directory from wp-content/themes
    - `is_activated` — boolean — Whether to activate the theme after deployment

#### `GET` `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes`
**List installed WordPress themes**

List themes installed on a WordPress installation, including their status, available updates and known vulnerabilities. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field).

- **Chemin** : `username`, `software`

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/activate`
**Activate WordPress theme**

Activate an installed theme on a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the activation job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `theme` — string · **requis** — Slug of the installed theme to activate.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/install`
**Install WordPress theme**

Install a theme on an existing WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). When the theme is one of the Hostinger themes (hostinger-blog, hostinger-affiliate-theme, hostinger-ai-theme), the optional `palette`, `layout`, and `font` fields are forwarded to the custom installer (default […]

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `theme` — string · **requis** — Slug of the theme to install. Hostinger theme slugs (hostinger-blog, hostinger-affiliate-theme, hostinger-ai-theme) trig
    - `palette` — string — Palette identifier. Only applied when the theme is a Hostinger theme; the default is used when omitted.
    - `layout` — string — Layout identifier. Only applied when the theme is a Hostinger theme; the default is used when omitted.
    - `font` — enum — Font identifier. Only applied when the theme is a Hostinger theme; the default is used when omitted.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/uninstall`
**Uninstall WordPress themes**

Uninstall one or more themes from a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the uninstall job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `themes` — array<string> · **requis** — Slugs of the installed themes to uninstall.

#### `POST` `/api/hosting/v1/accounts/{username}/wordpress/{software}/themes/update`
**Update WordPress themes**

Update one or more installed themes to their latest version on a WordPress installation. Provide the WordPress installation (software) identifier in the path. It can be obtained from GET /api/hosting/v1/wordpress/installations (the `id` field). This operation is asynchronous: a successful response only means the update job has been queued.

- **Chemin** : `username`, `software`
- **Corps** (requis) :
    - `themes` — array<string> · **requis** — Slugs of the installed themes to update to their latest version.

#### `GET` `/api/hosting/v1/wordpress/themes`
**List WordPress themes**

List WordPress themes available to install. Use the returned `slug` values with POST /api/hosting/v1/accounts/{username}/wordpress/{software}/themes/install.

- **Query** : `order_id`, `search`


## Agency Hosting (`/api/agency-hosting/v1`)

## Agency Hosting: Cache

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/cache` | Clear website cache |

#### `DELETE` `/api/agency-hosting/v1/websites/{website_uid}/cache`
**Clear website cache**

Clears cache for all domains associated with an Agency Plan website, including its preview domain. This operation clears all cache types for the website.

- **Chemin** : `website_uid`


## Agency Hosting: Cron Jobs

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs` | List website cron jobs |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs` | Create website cron job |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs/{uuid}` | Delete website cron job |

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs`
**List website cron jobs**

Returns a paginated list of cron jobs configured for an Agency Plan website. Each entry includes the schedule expression and the command executed on that schedule.

- **Chemin** : `website_uid`
- **Query** : `page`, `per_page`

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs`
**Create website cron job**

Creates a cron job for an Agency Plan website from a schedule expression and a command. Returns the created cron job, including its uuid, which is required to delete the cron job.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `time` — string · **requis** — Cron schedule expression (standard 5-field crontab syntax).
    - `command` — string · **requis** — Command to run on the schedule. Must not contain pipe (|) or redirection (<, >) characters.

#### `DELETE` `/api/agency-hosting/v1/websites/{website_uid}/cron-jobs/{uuid}`
**Delete website cron job**

Permanently deletes the cron job identified by its uuid from an Agency Plan website. The operation is idempotent: deleting a cron job that does not exist succeeds without error.

- **Chemin** : `website_uid`, `uuid`


## Agency Hosting: Databases

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/databases` | List website databases |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/databases` | Create website database |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}` | Delete website database |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}/users` | Create website database user |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}/users/{database_user_name}` | Delete website database user |

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/databases`
**List website databases**

Returns a paginated list of MySQL databases created for an Agency Plan website. Each entry includes the database's non-system users.

- **Chemin** : `website_uid`
- **Query** : `page`, `per_page`

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/databases`
**Create website database**

Creates a MySQL database with a dedicated user for an Agency Plan website. The database name, username, and password must all be provided by the caller.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `database_name` — string · **requis** — Database name to create (alphanumeric characters).
    - `database_user` — string · **requis** — Database username to create alongside the database (alphanumeric characters).
    - `password` — string · **requis** — Password for the database user (requires mixed case, letters, and numbers).

#### `DELETE` `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}`
**Delete website database**

Permanently deletes a MySQL database and all its data from an Agency Plan website, including its users. The operation is idempotent: deleting a database that does not exist succeeds without error.

- **Chemin** : `website_uid`, `database_name`

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}/users`
**Create website database user**

Creates a user for an existing database on an Agency Plan website. Each database supports a single non-system user; creating a user for a database that already has one fails.

- **Chemin** : `website_uid`, `database_name`
- **Corps** (requis) :
    - `database_user` — string · **requis** — Database username to create (alphanumeric and underscores).
    - `password` — string · **requis** — Password for the database user (requires mixed case, letters, and numbers).
    - `host` — string — Host the user connects from (IPv4, IPv6, % wildcard, or localhost). Defaults to localhost.

#### `DELETE` `/api/agency-hosting/v1/websites/{website_uid}/databases/{database_name}/users/{database_user_name}`
**Delete website database user**

Permanently deletes a database user from an Agency Plan website database, revoking all access it had. The operation is idempotent: deleting a user that does not exist succeeds without error.

- **Chemin** : `website_uid`, `database_name`, `database_user_name`


## Agency Hosting: Datacenters

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/datacenters` | List available datacenters |

#### `GET` `/api/agency-hosting/v1/orders/{order_id}/datacenters`
**List available datacenters**

Lists the datacenters available for provisioning a new website on the given Agency Plan hosting order. Each datacenter includes a `pinger_url` you can ping from the client to measure round-trip latency; comparing the results across datacenters lets you pick the nearest one (lowest ping) before choosing its `code` as the `datacenter_code` when creating a website setup.

- **Chemin** : `order_id`


## Agency Hosting: Domains

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/domains` | List domains |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/domains` | Link domain to website |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}` | Unlink domain from website |
| `PUT` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{from_domain}` | Change website domain |

#### `GET` `/api/agency-hosting/v1/domains`
**List domains**

Returns a paginated list of domains associated with Agency Plan websites accessible to the authenticated client. Use the website_uuids filter to narrow results to specific websites.

- **Query** : `page`, `per_page`, `website_uuids`

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/domains`
**Link domain to website**

Links a domain to the specified Agency Plan website so it can serve traffic for that domain.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `domain` — string · **requis** — Fully qualified domain name to link to the website

#### `DELETE` `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}`
**Unlink domain from website**

Unlinks a domain from the specified Agency Plan website. The website stops serving traffic on this domain immediately. Website files and database are preserved, and any other linked domains remain accessible. If this is the only domain on the website, unlinking leaves the website without an accessible domain.

- **Chemin** : `website_uid`, `domain`

#### `PUT` `/api/agency-hosting/v1/websites/{website_uid}/domains/{from_domain}`
**Change website domain**

Changes the primary domain for an Agency Plan website. Provide the current domain in the path and the new domain in the request body. Set domain to null to revert to the temporary domain.

- **Chemin** : `website_uid`, `from_domain`
- **Corps** (requis) :
    - `domain` — string · **requis** — New domain to assign to the website. Set to null to revert to the temporary domain.


## Agency Hosting: Files

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/files/import-archive` | Import website from archive |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/files/upload-urls` | Generate upload URL |

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/files/import-archive`
**Import website from archive**

Imports an Agency Plan website from an already-uploaded archive. Upload the archive to the website's root directory via file browser first, then provide its filename in this request. Website contents are overwritten by the archive contents. Supported archive types: .zip, .tar, .tar.gz, .tgz.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `archive_name` — string · **requis** — Archive filename (e.g., archive.zip). The file must already be uploaded to the website's .h5g/ directory.

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/files/upload-urls`
**Generate upload URL**

Generate a file browser upload URL with authentication credentials for uploading files to an Agency Plan website's file storage. Returns `url`, `auth_key` and `rest_auth_key`. Use these to upload a file to the website's file storage via the TUS resumable upload protocol (TUS 1.0.0). Send `X-Auth: {auth_key}` and `X-Auth-Rest: {rest_auth_key}` headers on every request below. 1. Create the upload: `POST` to `{url}/{rel […]

- **Chemin** : `website_uid`


## Agency Hosting: Metrics

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/disk-usage-metrics` | List Agency Plan order disk usage metrics |
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/resource-usage-metrics` | List order resource usage metrics |

#### `GET` `/api/agency-hosting/v1/orders/{order_id}/disk-usage-metrics`
**List Agency Plan order disk usage metrics**

Returns aggregated disk and inode usage for the Agency Plan order over the selected time frame, plus the plan quotas. Figures cover the whole order account. Values may be up to one hour stale. CPU, memory, and process usage are on the resource-usage-metrics endpoint.

- **Chemin** : `order_id`
- **Query** : `time_frame_days`

#### `GET` `/api/agency-hosting/v1/orders/{order_id}/resource-usage-metrics`
**List order resource usage metrics**

Returns aggregated CPU, memory, and process usage for the Agency Plan order over the selected time frame, plus the plan quotas and a per-website breakdown. Each website is identified by uid. Suspended and deleted websites are excluded from both the order totals and the per-website breakdown. Values may be up to one hour stale. Disk and inode usage are on the disk-usage-metrics endpoint.

- **Chemin** : `order_id`
- **Query** : `time_frame_hours`


## Agency Hosting: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders` | List orders |

#### `GET` `/api/agency-hosting/v1/orders`
**List orders**

Returns a paginated list of Agency Plan orders accessible to the authenticated client.

- **Query** : `page`, `per_page`


## Agency Hosting: PHP

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/websites/php-settings/versions` | List available PHP versions for an order |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/extensions` | List PHP extensions for a website |
| `PUT` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/extensions` | Replace website PHP extensions |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/options` | List PHP options for a website |
| `PUT` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/options` | Replace website PHP options |
| `PATCH` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/version` | Update website PHP version |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/php-settings/versions` | List available PHP versions for a website |

#### `GET` `/api/agency-hosting/v1/orders/{order_id}/websites/php-settings/versions`
**List available PHP versions for an order**

Lists the PHP versions available to websites created under an Agency Plan order, determined by the server the order is hosted on. Use this before creating a website; for a website that already exists, call the website-scoped versions endpoint instead.

- **Chemin** : `order_id`

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/php-settings/extensions`
**List PHP extensions for a website**

Lists every PHP extension available to an Agency Plan website and whether it is currently enabled.

- **Chemin** : `website_uid`

#### `PUT` `/api/agency-hosting/v1/websites/{website_uid}/php-settings/extensions`
**Replace website PHP extensions**

Replaces the set of PHP extensions enabled on an Agency Plan website with the ones provided. Any toggleable extension not in the request is disabled, so call the extensions endpoint first and send the full desired set. Extensions compiled into PHP, reported with the "built-in" state, are always active and are unaffected.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `extensions` — array<string> · **requis** — Extension names, exactly as returned by the extensions endpoint.

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/php-settings/options`
**List PHP options for a website**

Lists the php.ini directives that can be configured for an Agency Plan website, each with its default, the value currently in effect, and the values it accepts.

- **Chemin** : `website_uid`

#### `PUT` `/api/agency-hosting/v1/websites/{website_uid}/php-settings/options`
**Replace website PHP options**

Replaces the custom php.ini values on an Agency Plan website with the ones provided. Any option not in the request is reset to its default, so call the options endpoint first and send the full desired set. Sending an empty array resets every option to its default.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `options` — array<object> · **requis** — Option names and values. Each name must be one of the options returned by the options endpoint, and each value must sati

#### `PATCH` `/api/agency-hosting/v1/websites/{website_uid}/php-settings/version`
**Update website PHP version**

Switches an Agency Plan website to a different PHP version. Call the available versions endpoint first to see which versions can be selected. The website restarts on the new version, so requests served during the switch may fail and code that is incompatible with the target version will break.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `version` — string · **requis** — PHP version to switch the website to, as major.minor. Must be one of the versions returned by the available versions end

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/php-settings/versions`
**List available PHP versions for a website**

Lists the PHP versions an Agency Plan website can be switched to. The version the website is currently running is returned as settings.php.version by the website details endpoint.

- **Chemin** : `website_uid`


## Agency Hosting: SSL

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl` | Uninstall website SSL |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/reinstall` | Reinstall website SSL |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/setup` | Install website SSL |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/status` | Get website SSL status |

#### `DELETE` `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl`
**Uninstall website SSL**

Removes the platform-issued Let's Encrypt certificate of the domain: the certificate is revoked and deleted before the response, so the domain is no longer served with a platform certificate until a new setup completes. Also succeeds when the domain has no platform certificate to remove. Uploaded (custom) certificates are not affected. Returns 422 when a certificate process is recorded for the domain (a failed setup  […]

- **Chemin** : `website_uid`, `domain`

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/reinstall`
**Reinstall website SSL**

Replaces the Let's Encrypt certificate of the domain: the current platform certificate, when one is recorded, is revoked and removed, then a new setup starts in the background. Returns at once; `Get website SSL status` reports `installing` while it runs, then `active` or `failed`. Returns 422 for free subdomains, when a certificate process is recorded for the domain (a failed setup counts until it is cleaned up), or  […]

- **Chemin** : `website_uid`, `domain`

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/setup`
**Install website SSL**

Starts a Let's Encrypt certificate setup for the domain and returns at once; the setup runs in the background. `Get website SSL status` reports `installing` while it runs, then `active` or `failed`; the `ssl_setup` entry of `List website processes` shows the same progress. Returns 422 when the domain already has a platform certificate that is not expired, when a certificate process is recorded for the domain (a faile […]

- **Chemin** : `website_uid`, `domain`

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/domains/{domain}/ssl/status`
**Get website SSL status**

Returns the SSL state of one domain of an Agency Plan website: the certificate `status`, whether the certificate was uploaded by the customer, and when it stops being valid. `installing` means a certificate setup is running or retrying; the `ssl_setup` entry of `List website processes` shows the same progress. `active` means a valid certificate is in place: uploaded by the customer, issued by the platform, or a lifet […]

- **Chemin** : `website_uid`, `domain`


## Agency Hosting: Website Setups

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/agency-hosting/v1/orders/{order_id}/websites/setups` | Create a new website |
| `GET` | `/api/agency-hosting/v1/orders/{order_id}/websites/setups/{setup_uuid}` | Get website setup status |

#### `POST` `/api/agency-hosting/v1/orders/{order_id}/websites/setups`
**Create a new website**

Provisions a new website on one of your Agency Plan hosting orders. Choose the datacenter, stack (`flavor`), and PHP version for the site. Optionally attach your own `domain` — omit it, set it to `null`, or leave it unavailable and a free `*.hostingersite.com` subdomain is generated instead — and/or install WordPress by supplying the `wordpress` details (admin account, site title, and language). Common setups: - **Pl […]

- **Chemin** : `order_id`
- **Corps** (requis) :
    - `datacenter_code` — string · **requis** — Datacenter code where the website should be provisioned. Available codes depend on live capacity and are not a fixed set
    - `flavor` — string · **requis** — Setup flavor: a specific WordPress version in the format `wp-<major>.<minor>` or `wp-<major>.<minor>.<patch>` (e.g. `wp-
    - `settings` — object · **requis** — Website settings
    - `domain` — string — Primary domain to attach to the website. Omit or set to null to get a free auto-generated *.hostingersite.com subdomain 
    - `type` — enum — Website type
    - `wordpress` — object — WordPress installation options

#### `GET` `/api/agency-hosting/v1/orders/{order_id}/websites/setups/{setup_uuid}`
**Get website setup status**

Returns the current status of an Agency Plan website setup started via the setups endpoint. Poll this endpoint using the `setup_uuid` returned from the provisioning request until `status` becomes `completed`, at which point `website_uid` identifies the new website.

- **Chemin** : `order_id`, `setup_uuid`


## Agency Hosting: Websites

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites` | List Agency Plan websites |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}` | Get website details |
| `DELETE` | `/api/agency-hosting/v1/websites/{website_uid}` | Delete website |
| `POST` | `/api/agency-hosting/v1/websites/{website_uid}/build-assets` | Build website NodeJS assets |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/processes` | List website processes |

#### `GET` `/api/agency-hosting/v1/websites`
**List Agency Plan websites**

Retrieve a paginated list of Agency Plan websites (H5G, Builder, and Horizons) accessible to the authenticated client. This endpoint returns websites from your hosting accounts as well as websites from other client hosting accounts that have shared access with you. The response shape differs per platform — see the `platform` field on each item. Use `website_types` to list only websites of a given detected type, e.g.  […]

- **Query** : `page`, `per_page`, `order_ids`, `states`, `website_types`, `domain`

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}`
**Get website details**

Retrieves detailed information about a specific Agency Plan website, including configuration, status, metadata, hosting plan details, and resource quotas.

- **Chemin** : `website_uid`

#### `DELETE` `/api/agency-hosting/v1/websites/{website_uid}`
**Delete website**

Permanently deletes an Agency Plan website. Deletion is processed asynchronously: the website is immediately transitioned to a deleting state and the underlying server resources are removed in the background.

- **Chemin** : `website_uid`

#### `POST` `/api/agency-hosting/v1/websites/{website_uid}/build-assets`
**Build website NodeJS assets**

Builds and deploys a Node.js application for an Agency Plan website from an already-uploaded archive. Upload the archive to file browser first, then provide its relative path from document root in this request. Website contents are overwritten by the build result, which is deployed to public_html.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `archive_path` — string · **requis** — Directory, relative to the website document root, where the uploaded site archive currently lives. Most commonly this is

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/processes`
**List website processes**

Lists active and recently completed asynchronous processes for an Agency Plan website. Each process has a unique ID (for tracking), a type, and a status (running, completed, failed). Poll this endpoint after initiating async operations (SSL setup, backups, cloning) to track progress.

- **Chemin** : `website_uid`


## Agency Hosting: WordPress

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings` | Get WordPress settings |
| `PATCH` | `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings/version` | Change WordPress version |
| `GET` | `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings/versions` | List available WordPress versions |

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings`
**Get WordPress settings**

Returns the current WordPress settings for an Agency Plan website: installed core version, LiteSpeed Cache plugin status, object cache status, and maintenance mode status.

- **Chemin** : `website_uid`

#### `PATCH` `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings/version`
**Change WordPress version**

Changes the installed WordPress core version on an Agency Plan website to one of the versions available for installation.

- **Chemin** : `website_uid`
- **Corps** (requis) :
    - `version` — string · **requis** — Target WordPress core version to install. Must be one of the available versions.

#### `GET` `/api/agency-hosting/v1/websites/{website_uid}/wordpress/settings/versions`
**List available WordPress versions**

Lists the WordPress core versions available for installation on an Agency Plan website.

- **Chemin** : `website_uid`


## Horizons (`/api/horizons/v1`)

## Horizons: Websites

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/horizons/v1/websites` | Get website list |
| `POST` | `/api/horizons/v1/websites` | Create website |
| `GET` | `/api/horizons/v1/websites/{websiteId}` | Get website |
| `POST` | `/api/horizons/v1/websites/{websiteId}/clone` | Clone website |
| `POST` | `/api/horizons/v1/websites/{websiteId}/messages` | Edit website |
| `POST` | `/api/horizons/v1/websites/{websiteId}/publish` | Publish website |

#### `GET` `/api/horizons/v1/websites`
**Get website list**

List the Hostinger Horizons websites the user owns.\n Use this tool when the user asks which websites they have, or when you need a website ID before editing, publishing or cloning a website.\n Each website is returned with its ID, status, domain and the URL to open it in Hostinger Horizons interface.\n The complete list of websites is returned in a single response - it is not paginated.

- Aucun paramètre

#### `POST` `/api/horizons/v1/websites`
**Create website**

Create new Hostinger Horizons website from the given message.\n Use this tool when user asks you to create a website, landing page, blog or any other type of application.\n This tool initiates the website creation process and returns a website URL and ID. The generation happens asynchronously.\n After invoking this tool, your chat reply must be EXACTLY 1 sentence summarizing that Hostinger Horizons is now creating th […]

- **Corps** (requis) :
    - `message` — array<object> · **requis**

#### `GET` `/api/horizons/v1/websites/{websiteId}`
**Get website**

Get the link for the user to open their website in Hostinger Horizons interface.\n Use this tool when the user wants the link to an existing website, or when you need its website URL before or after editing it.\n Websites can be edited with the `Edit website` tool, or by the user in Hostinger Horizons interface in the provided website URL.

- **Chemin** : `websiteId`

#### `POST` `/api/horizons/v1/websites/{websiteId}/clone`
**Clone website**

Clone a Hostinger Horizons website into a new website.\n Use this tool when the user wants a copy of an existing website, for example to try out changes without touching the original.\n This tool returns the ID and URL of the newly created copy. The original website is left untouched.\n To edit the copy, use the `Edit website` tool with the returned website ID, or the user can open the provided website URL in Hosting […]

- **Chemin** : `websiteId`

#### `POST` `/api/horizons/v1/websites/{websiteId}/messages`
**Edit website**

Edit an existing Hostinger Horizons website with a follow-up message.\n Use this tool when the user wants to change, extend or fix a website that already exists.\n This tool queues the requested changes and returns the website URL and ID. The changes are applied asynchronously.\n After invoking this tool, your chat reply must be EXACTLY 1 sentence summarizing that Hostinger Horizons is now applying the requested chan […]

- **Chemin** : `websiteId`
- **Corps** (requis) :
    - `message` — array<object> · **requis**

#### `POST` `/api/horizons/v1/websites/{websiteId}/publish`
**Publish website**

Publish a Hostinger Horizons website so its latest changes go live.\n Use this tool when the user asks to publish, deploy or make their website live.\n This tool starts the publish process and returns the URL the website will be live on. Publishing happens asynchronously and takes a few minutes.\n After invoking this tool, your chat reply must be EXACTLY 1 sentence summarizing that the website is being published and  […]

- **Chemin** : `websiteId`

