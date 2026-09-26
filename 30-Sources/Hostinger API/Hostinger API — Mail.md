---
tags: [hostinger, api, reference]
---

# Hostinger API — Mail

Retour à l'index : [[Hostinger API — Index]] · 38 endpoints sur `/api/mail/v1` (boîtes, alias, redirections, répondeurs, listes de diffusion, webhooks, journaux).

> [!tip] Ordre logique
> Commander une offre de mail (`/orders`) → créer les boîtes (`/mailboxes`) → puis les alias/redirections/répondeurs → brancher les webhooks → vérifier via `/logs`.

## Mail: API Tokens

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/mail/v1/api-tokens` | List API tokens |
| `DELETE` | `/api/mail/v1/api-tokens/{tokenId}` | Revoke API token |
| `POST` | `/api/mail/v1/orders/{orderId}/api-tokens` | Create API token |

#### `GET` `/api/mail/v1/api-tokens`
**List API tokens**

Retrieve a paginated list of [Hostinger Email API](https://api.mail.hostinger.com/) tokens across all your mail orders, optionally filtered by order. Plaintext tokens are never included; they are returned only when a token is created.

- **Query** : `order_id`, `page`, `per_page`

#### `DELETE` `/api/mail/v1/api-tokens/{tokenId}`
**Revoke API token**

Revoke an API token. The token immediately loses access to the [Hostinger Email API](https://api.mail.hostinger.com/). This action cannot be undone.

- **Chemin** : `tokenId`

#### `POST` `/api/mail/v1/orders/{orderId}/api-tokens`
**Create API token**

Create an API token for the given mail order. The token grants access to the [Hostinger Email API](https://api.mail.hostinger.com/), where you can provision and manage the mailboxes it is scoped to. The plaintext token is returned only in this response, never again. A maximum of 10 tokens can exist per order. Use `scope.has_all_mailboxes` to cover all current and future mailboxes, or list specific mailboxes in `scope […]

- **Chemin** : `orderId`
- **Corps** (requis) :
    - `name` — string · **requis** — Human-readable label for this token
    - `scope` — object · **requis** — Mailbox scope this token can access


## Mail: Aliases

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/aliases/{aliasId}` | Delete alias |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/aliases` | Create alias |
| `GET` | `/api/mail/v1/orders/{orderId}/aliases` | List aliases |

#### `DELETE` `/api/mail/v1/aliases/{aliasId}`
**Delete alias**

Delete an alias. Messages sent to the alias address are no longer delivered to the mailbox.

- **Chemin** : `aliasId`

#### `POST` `/api/mail/v1/mailboxes/{mailboxId}/aliases`
**Create alias**

Create an alias for the given mailbox. The alias address is formed from the given local part and the domain of the mailbox. Messages sent to the alias are delivered to the mailbox.

- **Chemin** : `mailboxId`
- **Corps** (requis) :
    - `local_part` — string · **requis** — Local part of the alias address (the part before the @). The domain is taken from the mailbox. Case-insensitive and stor

#### `GET` `/api/mail/v1/orders/{orderId}/aliases`
**List aliases**

Retrieve a paginated list of aliases across all mailboxes of a mail order.

- **Chemin** : `orderId`
- **Query** : `page`, `per_page`


## Mail: Autoreplies

| Méthode | Endpoint | Description |
|---|---|---|
| `PUT` | `/api/mail/v1/autoreplies/{autoreplyId}` | Update autoreply |
| `DELETE` | `/api/mail/v1/autoreplies/{autoreplyId}` | Delete autoreply |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/autoreplies` | Create autoreply |
| `GET` | `/api/mail/v1/orders/{orderId}/autoreplies` | List autoreplies |

#### `PUT` `/api/mail/v1/autoreplies/{autoreplyId}`
**Update autoreply**

Replace the autoreply with the given content and schedule. Omitted optional fields are cleared: omit `starts_at` to activate the autoreply immediately and omit `ends_at` to keep it active indefinitely.

- **Chemin** : `autoreplyId`
- **Corps** (requis) :
    - `subject` — string · **requis** — Subject of the automatic reply
    - `body` — string · **requis** — Body of the automatic reply
    - `display_name` — string — Sender display name used for the reply
    - `starts_at` — string — When the autoreply becomes active. Defaults to now.
    - `ends_at` — string — When the autoreply stops. Omit for an indefinite autoreply.

#### `DELETE` `/api/mail/v1/autoreplies/{autoreplyId}`
**Delete autoreply**

Delete the autoreply of a mailbox. The mailbox stops sending automatic replies immediately.

- **Chemin** : `autoreplyId`

#### `POST` `/api/mail/v1/mailboxes/{mailboxId}/autoreplies`
**Create autoreply**

Create an automatic reply for the given mailbox. A mailbox can have only one autoreply. Omit `starts_at` to activate the autoreply immediately and omit `ends_at` to keep it active indefinitely.

- **Chemin** : `mailboxId`
- **Corps** (requis) :
    - `subject` — string · **requis** — Subject of the automatic reply
    - `body` — string · **requis** — Body of the automatic reply
    - `display_name` — string — Sender display name used for the reply
    - `starts_at` — string — When the autoreply becomes active. Defaults to now.
    - `ends_at` — string — When the autoreply stops. Omit for an indefinite autoreply.

#### `GET` `/api/mail/v1/orders/{orderId}/autoreplies`
**List autoreplies**

Retrieve a paginated list of autoreplies across all mailboxes of a mail order.

- **Chemin** : `orderId`
- **Query** : `page`, `per_page`


## Mail: Catchalls

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/catchalls/{catchallId}` | Delete catch-all |
| `POST` | `/api/mail/v1/catchalls/{catchallId}/confirmation/resend` | Resend catch-all confirmation |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/catchalls` | Create catch-all |
| `GET` | `/api/mail/v1/orders/{orderId}/catchalls` | List catch-alls |

#### `DELETE` `/api/mail/v1/catchalls/{catchallId}`
**Delete catch-all**

Delete a catch-all. Messages sent to unknown addresses of the domain are no longer routed to the mailbox.

- **Chemin** : `catchallId`

#### `POST` `/api/mail/v1/catchalls/{catchallId}/confirmation/resend`
**Resend catch-all confirmation**

Resend the confirmation email to the mailbox address of an unconfirmed catch-all.

- **Chemin** : `catchallId`

#### `POST` `/api/mail/v1/mailboxes/{mailboxId}/catchalls`
**Create catch-all**

Create a catch-all that routes all messages sent to unknown addresses of the domain to the given mailbox. The mailbox address receives a confirmation email and the catch-all becomes active only after it is confirmed. A domain can have only one catch-all.

- **Chemin** : `mailboxId`

#### `GET` `/api/mail/v1/orders/{orderId}/catchalls`
**List catch-alls**

Retrieve a paginated list of catch-alls across all mailboxes of a mail order.

- **Chemin** : `orderId`
- **Query** : `page`, `per_page`


## Mail: Forwarders

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/forwarders/{forwarderId}` | Delete forwarder |
| `POST` | `/api/mail/v1/forwarders/{forwarderId}/confirmation/resend` | Resend forwarder confirmation |
| `PATCH` | `/api/mail/v1/forwarders/{forwarderId}/keep-copy` | Update forwarder keep-copy setting |
| `POST` | `/api/mail/v1/mailboxes/{mailboxId}/forwarders` | Create forwarder |
| `GET` | `/api/mail/v1/orders/{orderId}/forwarders` | List forwarders |

#### `DELETE` `/api/mail/v1/forwarders/{forwarderId}`
**Delete forwarder**

Delete a forwarder. The mailbox stops forwarding messages to the destination address immediately.

- **Chemin** : `forwarderId`

#### `POST` `/api/mail/v1/forwarders/{forwarderId}/confirmation/resend`
**Resend forwarder confirmation**

Resend the confirmation email to the destination address of an unconfirmed forwarder.

- **Chemin** : `forwarderId`

#### `PATCH` `/api/mail/v1/forwarders/{forwarderId}/keep-copy`
**Update forwarder keep-copy setting**

Enable or disable keeping a copy of forwarded messages in the mailbox.

- **Chemin** : `forwarderId`
- **Corps** (requis) :
    - `is_keep_copy_enabled` — boolean · **requis** — Whether to keep a copy of forwarded messages in the mailbox

#### `POST` `/api/mail/v1/mailboxes/{mailboxId}/forwarders`
**Create forwarder**

Create a forwarder from the given mailbox to the destination address. The destination receives a confirmation email and forwarding becomes active only after it is confirmed.

- **Chemin** : `mailboxId`
- **Corps** (requis) :
    - `destination` — string · **requis** — Email address the messages will be forwarded to
    - `is_keep_copy_enabled` — boolean — Whether to keep a copy of forwarded messages in the mailbox. Defaults to false.

#### `GET` `/api/mail/v1/orders/{orderId}/forwarders`
**List forwarders**

Retrieve a paginated list of forwarders across all mailboxes of a mail order.

- **Chemin** : `orderId`
- **Query** : `page`, `per_page`


## Mail: Logs

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/mail/v1/orders/{orderId}/logs/access` | List access logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/action` | List action logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/inbound` | List inbound logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/mailbox-actions` | List mailbox action logs |
| `GET` | `/api/mail/v1/orders/{orderId}/logs/outbound` | List outbound logs |

#### `GET` `/api/mail/v1/orders/{orderId}/logs/access`
**List access logs**

Retrieve paginated access logs for the domain attached to the given mail order. Supports filtering by account, date range, protocol, status, and deletion flag. Results are sorted by timestamp descending.

- **Chemin** : `orderId`
- **Query** : `account`, `date`, `from_date`, `to_date`, `status`, `protocol`, `has_deletions`, `page`, `per_page`

#### `GET` `/api/mail/v1/orders/{orderId}/logs/action`
**List action logs**

Retrieve paginated account action logs (administrative and user actions) for the given mail order. Supports filtering by account, date range, and status. Results are sorted by timestamp descending.

- **Chemin** : `orderId`
- **Query** : `account`, `date`, `from_date`, `to_date`, `status`, `page`, `per_page`

#### `GET` `/api/mail/v1/orders/{orderId}/logs/inbound`
**List inbound logs**

Retrieve paginated inbound (received mail) delivery logs for the domain attached to the given mail order. Supports filtering by account, date range, status, sender, and recipient. Results are sorted by timestamp descending.

- **Chemin** : `orderId`
- **Query** : `account`, `date`, `from_date`, `to_date`, `status`, `sender`, `recipient`, `page`, `per_page`

#### `GET` `/api/mail/v1/orders/{orderId}/logs/mailbox-actions`
**List mailbox action logs**

Retrieve paginated mailbox action logs (message and mailbox events) for a mailbox in the given mail order. The mailbox email must belong to the order's domain. Supports date range and event type filters. Results are sorted by timestamp descending.

- **Chemin** : `orderId`
- **Query** : `email`, `date`, `from_date`, `to_date`, `event`, `page`, `per_page`

#### `GET` `/api/mail/v1/orders/{orderId}/logs/outbound`
**List outbound logs**

Retrieve paginated outbound (sent mail) delivery logs for the domain attached to the given mail order. Supports filtering by account, date range, status, sender, and recipient. Results are sorted by timestamp descending.

- **Chemin** : `orderId`
- **Query** : `account`, `date`, `from_date`, `to_date`, `status`, `sender`, `recipient`, `page`, `per_page`


## Mail: Mailboxes

| Méthode | Endpoint | Description |
|---|---|---|
| `DELETE` | `/api/mail/v1/mailboxes/{mailboxId}` | Delete mailbox |
| `PATCH` | `/api/mail/v1/mailboxes/{mailboxId}/password` | Change mailbox password |
| `GET` | `/api/mail/v1/orders/{orderId}/mailboxes` | List mailboxes |
| `POST` | `/api/mail/v1/orders/{orderId}/mailboxes` | Create mailbox |

#### `DELETE` `/api/mail/v1/mailboxes/{mailboxId}`
**Delete mailbox**

Delete a mailbox. The mailbox is soft-deleted and stays restorable for a limited period before it is permanently removed.

- **Chemin** : `mailboxId`

#### `PATCH` `/api/mail/v1/mailboxes/{mailboxId}/password`
**Change mailbox password**

Change the password of a mailbox.

- **Chemin** : `mailboxId`
- **Corps** (requis) :
    - `password` — string · **requis** — New mailbox password. Minimum 8 characters with uppercase, lowercase, number and special character; must not be a common

#### `GET` `/api/mail/v1/orders/{orderId}/mailboxes`
**List mailboxes**

Retrieve a paginated list of mailboxes belonging to a mail order. Use this endpoint to monitor mailboxes of your mail service, including their status, enabled protocols, attached resource counts, and periodically synced usage numbers (usage may lag behind live values).

- **Chemin** : `orderId`
- **Query** : `search`, `sort`, `page`, `per_page`

#### `POST` `/api/mail/v1/orders/{orderId}/mailboxes`
**Create mailbox**

Create a mailbox under the given mail order. The full email address is composed from the given local part and the domain of the order.

- **Chemin** : `orderId`
- **Corps** (requis) :
    - `local_part` — string · **requis** — Local part of the mailbox address (the part before the @). The domain is taken from the order. Must start and end with a
    - `password` — string · **requis** — Mailbox password. Minimum 8 characters with uppercase, lowercase, number and special character.


## Mail: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/mail/v1/orders` | List orders |
| `GET` | `/api/mail/v1/orders/{orderId}/plan` | Get order plan |

#### `GET` `/api/mail/v1/orders`
**List orders**

Retrieve a paginated list of mail orders associated with your account. Use this endpoint to monitor your mail services, including their status, plan, attached domain, and expiration details.

- **Query** : `domain`, `status`, `is_trial`, `sort`, `page`, `per_page`

#### `GET` `/api/mail/v1/orders/{orderId}/plan`
**Get order plan**

Retrieve the plan the given mail order was purchased with, including domain-level and mailbox-level quotas, limits, and protocol availability.

- **Chemin** : `orderId`


## Mail: Webhooks

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

#### `POST` `/api/mail/v1/mailboxes/{mailboxId}/webhooks`
**Create webhook**

Create a webhook for the given mailbox. The generated secret is returned only in this response and is sent as a bearer token with every delivery.

- **Chemin** : `mailboxId`
- **Corps** (requis) :
    - `name` — string · **requis** — Human-readable name for this webhook
    - `description` — string — Optional description of the webhook's purpose
    - `events` — array<string> · **requis** — Events that trigger this webhook
    - `status` — enum — Initial status of the webhook
    - `url` — string · **requis** — Publicly reachable URL that receives the webhook POST requests

#### `GET` `/api/mail/v1/orders/{orderId}/webhooks`
**List webhooks**

Retrieve a paginated list of webhooks belonging to the given mail order. Supports filtering by mailbox and status. The webhook secret is never included; it is returned only when a webhook is created or its secret is regenerated.

- **Chemin** : `orderId`
- **Query** : `mailbox_id`, `status`, `page`, `per_page`

#### `GET` `/api/mail/v1/orders/{orderId}/webhooks/delivery-logs`
**List webhook delivery logs**

Retrieve a paginated list of webhook delivery logs for the given mail order, including delivery outcome, duration, and retry counts. Supports filtering by mailbox.

- **Chemin** : `orderId`
- **Query** : `mailbox_id`, `page`, `per_page`

#### `GET` `/api/mail/v1/webhooks/{webhookId}`
**Get webhook**

Retrieve the details of a single webhook. The webhook secret is never included; it is returned only when a webhook is created or its secret is regenerated.

- **Chemin** : `webhookId`

#### `DELETE` `/api/mail/v1/webhooks/{webhookId}`
**Delete webhook**

Permanently delete a webhook. This action cannot be undone. After deletion the URL no longer receives event notifications.

- **Chemin** : `webhookId`

#### `PATCH` `/api/mail/v1/webhooks/{webhookId}`
**Update webhook**

Partially update a webhook. Only the fields included in the request body are changed; omitted fields retain their current values. Pass `"description": null` to clear the description.

- **Chemin** : `webhookId`
- **Corps** (requis) :
    - `name` — string — New human-readable name for the webhook
    - `description` — string — New description, or null to clear it
    - `events` — array<string> — Replaces the full list of subscribed events
    - `status` — enum — New status for the webhook
    - `url` — string — New URL to deliver events to

#### `POST` `/api/mail/v1/webhooks/{webhookId}/regenerate-secret`
**Regenerate webhook secret**

Regenerate the secret of a webhook. The previous secret is immediately invalidated. The new secret is returned only in this response and is sent as a bearer token with every delivery.

- **Chemin** : `webhookId`

#### `POST` `/api/mail/v1/webhooks/{webhookId}/test`
**Test webhook**

Send a test delivery to the webhook URL and return the result. Test requests are rate limited upstream.

- **Chemin** : `webhookId`

