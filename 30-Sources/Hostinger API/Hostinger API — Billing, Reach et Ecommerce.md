---
tags: [hostinger, api, reference]
---

# Hostinger API — Billing, Reach et Ecommerce

Retour à l'index : [[Hostinger API — Index]] · 9 facturation + 52 Reach + 29 ecommerce.

## Billing (`/api/billing/v1`)

## Billing: Catalog

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/billing/v1/catalog` | Get catalog item list |

#### `GET` `/api/billing/v1/catalog`
**Get catalog item list**

Retrieve catalog items available for order. Prices in catalog items is displayed as cents (without floating point), e.g: float `17.99` is displayed as integer `1799`. Use this endpoint to view available services and pricing before placing orders.

- **Query** : `category`, `name`


## Billing: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/billing/v1/orders` | Create purchase order |

#### `POST` `/api/billing/v1/orders`
**Create purchase order**

Create a purchase order for any Hostinger product. This unified endpoint places an order for one or more catalog items and works across all Hostinger products, leveraging the existing billing infrastructure. Use the [catalog endpoint](#tag/billing-catalog) to look up the `item_id` values available for purchase. If no payment method is provided, your default payment method will be used automatically. If the response i […]

- **Corps** (requis) :
    - `payment_method_id` — integer — Payment method ID, default will be used if not provided
    - `items` — array<object> · **requis** — Catalog price items to purchase
    - `coupons` — array<?> — Discount coupon codes


## Billing: Payment methods

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/billing/v1/payment-methods` | Get payment method list |
| `POST` | `/api/billing/v1/payment-methods/{paymentMethodId}` | Set default payment method |
| `DELETE` | `/api/billing/v1/payment-methods/{paymentMethodId}` | Delete payment method |

#### `GET` `/api/billing/v1/payment-methods`
**Get payment method list**

Retrieve available payment methods that can be used for placing new orders. If you want to add new payment method, please use [hPanel](https://hpanel.hostinger.com/billing/payment-methods). Use this endpoint to view available payment options before creating orders.

- Aucun paramètre

#### `POST` `/api/billing/v1/payment-methods/{paymentMethodId}`
**Set default payment method**

Set the default payment method for your account. Use this endpoint to configure the primary payment method for future orders.

- **Chemin** : `paymentMethodId`

#### `DELETE` `/api/billing/v1/payment-methods/{paymentMethodId}`
**Delete payment method**

Delete a payment method from your account. Use this endpoint to remove unused payment methods from user accounts.

- **Chemin** : `paymentMethodId`


## Billing: Subscriptions

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/billing/v1/subscriptions` | Get subscription list |
| `DELETE` | `/api/billing/v1/subscriptions/{subscriptionId}/auto-renewal/disable` | Disable auto-renewal |
| `PATCH` | `/api/billing/v1/subscriptions/{subscriptionId}/auto-renewal/enable` | Enable auto-renewal |
| `POST` | `/api/billing/v1/subscriptions/{subscriptionId}/renew` | Renew subscription |

#### `GET` `/api/billing/v1/subscriptions`
**Get subscription list**

Retrieve a list of all subscriptions associated with your account. Use this endpoint to monitor active services and billing status.

- Aucun paramètre

#### `DELETE` `/api/billing/v1/subscriptions/{subscriptionId}/auto-renewal/disable`
**Disable auto-renewal**

Disable auto-renewal for a subscription. Use this endpoint when disable auto-renewal for a subscription.

- **Chemin** : `subscriptionId`

#### `PATCH` `/api/billing/v1/subscriptions/{subscriptionId}/auto-renewal/enable`
**Enable auto-renewal**

Enable auto-renewal for a subscription. Use this endpoint when enable auto-renewal for a subscription.

- **Chemin** : `subscriptionId`

#### `POST` `/api/billing/v1/subscriptions/{subscriptionId}/renew`
**Renew subscription**

Create a renewal order for an existing Hostinger subscription. This endpoint places a renewal order for a single subscription, leveraging the existing billing infrastructure. Use the [subscriptions endpoint](#tag/billing-subscriptions) to look up the `subscriptionId` values available for renewal. If no payment method is provided, your default payment method will be used automatically. If the response is `202 Accepted […]

- **Chemin** : `subscriptionId`
- **Corps** (optionnel) :
    - `payment_method_id` — integer — Payment method ID, default will be used if not provided
    - `coupons` — array<?> — Discount coupon codes


## Reach — marketing email (`/api/reach/v1`)

## Reach: Automations

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/automations` | List automations |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/automations/{automationUuid}` | Get automation details |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/automations/{automationUuid}/steps` | List automation steps |

#### `GET` `/api/reach/v1/profiles/{profileUuid}/automations`
**List automations**

Get a paginated list of the automations in a profile. Every automation comes with the counts of contacts that entered it, are moving through it, finished it or failed on the way. Those counts describe the contact journey and are not email engagement metrics - for opens, clicks and unsubscribes use the campaign statistics endpoint instead.

- **Chemin** : `profileUuid`
- **Query** : `status`, `sort_direction`, `page`, `per_page`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/automations/{automationUuid}`
**Get automation details**

Get a single automation with the counts of contacts that entered it, are moving through it, finished it or failed on the way. This describes the automation itself. To see the workflow it runs, use the steps endpoint.

- **Chemin** : `profileUuid`, `automationUuid`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/automations/{automationUuid}/steps`
**List automation steps**

Get the workflow of an automation as a flat list of steps. The steps form a tree rather than a straight line: follow `parent_uuid` to reconstruct the branches, and use `step_order` to order the steps that share a parent. An automation with no steps yet returns an empty list.

- **Chemin** : `profileUuid`, `automationUuid`


## Reach: Campaigns

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/campaigns` | List campaigns |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/campaigns` | Create a draft campaign |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/campaigns/{campaignUuid}` | Get campaign details |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/campaigns/{campaignUuid}/statistics` | Get campaign performance |

#### `GET` `/api/reach/v1/profiles/{profileUuid}/campaigns`
**List campaigns**

Get a paginated list of the campaigns in a profile. Each campaign carries its headline engagement rates. Filter by status to find drafts, scheduled, sending or sent campaigns, keeping in mind that a fully sent campaign has the status `publish`. By default only regular campaigns are returned - pass `type` to get the emails sent by automations or the double opt-in confirmations instead.

- **Chemin** : `profileUuid`
- **Query** : `status`, `type`, `sort_direction`, `page`, `per_page`

#### `POST` `/api/reach/v1/profiles/{profileUuid}/campaigns`
**Create a draft campaign**

Create a campaign in a profile. The campaign is created as a draft, so nothing is sent and no contact is touched. It has no audience yet either - targeting and scheduling are not part of this request, the draft is finished and sent from the Reach interface.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `sender_name` — string · **requis** — From name shown to the recipients.
    - `sender_email` — string · **requis** — From address of the campaign. Its domain has to be verified on the profile before the campaign can be sent.
    - `title` — string — Name the campaign is listed under. Not shown to the recipients.
    - `subject` — string — Subject line of the email.
    - `template_uuid` — string — Template to send, as returned by the template endpoints. Can be left out and attached later, but the campaign cannot be 
    - `metadata` — object — Extra campaign fields. Any key outside the listed ones is rejected.

#### `GET` `/api/reach/v1/profiles/{profileUuid}/campaigns/{campaignUuid}`
**Get campaign details**

Get a single campaign with its sender, subject, template reference, targeting and delivery progress. This describes how the campaign was set up and how far it has got. For opens, clicks and unsubscribes use the campaign statistics endpoint.

- **Chemin** : `profileUuid`, `campaignUuid`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/campaigns/{campaignUuid}/statistics`
**Get campaign performance**

Get the performance of a campaign: delivery, opens, clicks and unsubscribes, with the matching rates. Every count is unique contacts rather than raw events, so a contact who opens the same email five times is counted once.

- **Chemin** : `profileUuid`, `campaignUuid`


## Reach: Contact Fields

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields` | List contact fields |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields` | Create a contact field |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields/{fieldUuid}` | Delete a contact field |
| `PATCH` | `/api/reach/v1/profiles/{profileUuid}/contacts/fields/{fieldUuid}` | Update a contact field |

#### `GET` `/api/reach/v1/profiles/{profileUuid}/contacts/fields`
**List contact fields**

Get the custom contact fields defined in a profile. Custom fields let you store your own attributes on contacts. The returned uuids are what you pass to the contact update endpoint to set values, and choice fields also list the options available to pick from.

- **Chemin** : `profileUuid`

#### `POST` `/api/reach/v1/profiles/{profileUuid}/contacts/fields`
**Create a contact field**

Define a new custom contact field in a profile. The `slug` is derived from the label and, like the field type, cannot be changed later. Use the returned uuid to set values on contacts.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `type` — enum · **requis** — Immutable once the field exists
    - `label` — string · **requis**
    - `options` — array<string> — Required for single_choice and multi_choice, ignored for the scalar types. Labels must be unique regardless of casing.

#### `DELETE` `/api/reach/v1/profiles/{profileUuid}/contacts/fields/{fieldUuid}`
**Delete a contact field**

Delete a custom contact field. Every value contacts hold for the field is deleted with it, and for the choice types so are its options. The contacts themselves are not affected.

- **Chemin** : `profileUuid`, `fieldUuid`

#### `PATCH` `/api/reach/v1/profiles/{profileUuid}/contacts/fields/{fieldUuid}`
**Update a contact field**

Rename a custom contact field and, for the choice types, replace its option set. Options carrying a uuid are kept and relabelled, options without one are created, and any existing option left out of the list is deleted along with the values contacts hold for it. The field type and slug cannot be changed.

- **Chemin** : `profileUuid`, `fieldUuid`
- **Corps** (requis) :
    - `label` — string · **requis**
    - `options` — array<object> — Replaces the option set when provided. Entries carrying a uuid are kept and relabelled, entries without one are created,


## Reach: Contacts

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

#### `GET` `/api/reach/v1/contacts`
**List contacts**

Get a list of contacts, optionally filtered by group and subscription status. This endpoint returns a paginated list of contacts with their basic information. You can filter contacts by group UUID and subscription status. **Deprecated.** This endpoint cannot target a profile, so it always falls back to the client's default profile and cannot list contacts of any other profile. Use `GET /api/reach/v1/profiles/{profile […]

- **Query** : `group_uuid`, `subscription_status`, `page`

#### `POST` `/api/reach/v1/contacts`
**Create a new contact**

Create a new contact in the email marketing system. This endpoint allows you to create a new contact with basic information like name, email, and surname. If double opt-in is enabled, the contact will be created with a pending status and a confirmation email will be sent.

- **Corps** (requis) :
    - `email` — string · **requis**
    - `name` — string
    - `surname` — string
    - `phone` — string — Phone number in E.164 format (leading "+" then 7-15 digits)
    - `note` — string
    - `tag_uuids` — array<string> — Existing tags to attach to the created contact

#### `GET` `/api/reach/v1/contacts/groups`
**List contact groups**

Get a list of all contact groups. This endpoint returns a list of contact groups that can be used to organize contacts.

- Aucun paramètre

#### `DELETE` `/api/reach/v1/contacts/{uuid}`
**Delete a contact**

Delete a contact with the specified UUID. This endpoint permanently removes a contact from the email marketing system. **Deprecated.** This endpoint cannot target a profile, so it always falls back to the client's default profile and cannot delete contacts of any other profile. Use `DELETE /api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}` instead.

- **Chemin** : `uuid`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/contacts`
**List profile contacts**

Get a paginated list of contacts belonging to a profile. Contacts can be filtered by subscription status, by tag, and by an email search term. The `meta.total` field of the response is the number of contacts matching the filters, so calling this endpoint without filters gives the profile's total contact count.

- **Chemin** : `profileUuid`
- **Query** : `subscription_status`, `tag_uuid`, `search`, `page`, `per_page`

#### `POST` `/api/reach/v1/profiles/{profileUuid}/contacts`
**Create new contacts**

Create a new contact in the email marketing system. This endpoint allows you to create a new contact with basic information like name, email, and surname. If double opt-in is enabled, the contact will be created with a pending status and a confirmation email will be sent.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `email` — string · **requis**
    - `name` — string
    - `surname` — string
    - `phone` — string — Phone number in E.164 format (leading "+" then 7-15 digits)
    - `note` — string
    - `tag_uuids` — array<string> — Existing tags to attach to the created contact

#### `POST` `/api/reach/v1/profiles/{profileUuid}/contacts/bulk`
**Create contacts in bulk**

Create many contacts in a profile in a single call. The contacts are imported in the background, so a success response means the import was accepted rather than finished. Contacts whose email already exists in the profile are left as they are. If double opt-in is enabled, new contacts start off pending and are sent a confirmation email.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `contacts` — array<object> · **requis**
    - `tag_uuids` — array<string> — Existing tags to attach to every created contact
    - `note` — string — Note applied to every created contact

#### `GET` `/api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}`
**Get contact details**

Get the full details of a single contact. Alongside the contact's own attributes this returns the tags assigned to it and the values it holds for the profile's custom contact fields.

- **Chemin** : `profileUuid`, `contactUuid`

#### `DELETE` `/api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}`
**Delete a profile contact**

Permanently delete a contact from a profile. The contact is removed together with its custom field values and tag assignments.

- **Chemin** : `profileUuid`, `contactUuid`

#### `PATCH` `/api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}`
**Update a contact**

Update a contact's attributes and custom field values. Only the properties present in the request body are changed, so a partial body is enough to change a single attribute. Sending a property as `null` clears it. The response carries the contact's core attributes. Read back its tags, custom field values, source and note with `GET /api/reach/v1/profiles/{profileUuid}/contacts/{contactUuid}`.

- **Chemin** : `profileUuid`, `contactUuid`
- **Corps** (requis) :
    - `email` — string
    - `name` — string
    - `surname` — string
    - `phone` — string — Phone number in E.164 format (leading "+" then 7-15 digits)
    - `subscription_status` — enum
    - `note` — string
    - `fields` — array<object> — Set custom field values. Omit to leave untouched, send an empty array to clear them all.


## Reach: Forms

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/forms` | List forms |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/forms/{formUuid}` | Get form details |
| `DELETE` | `/api/reach/v1/profiles/{profileUuid}/forms/{formUuid}` | Delete form |

#### `GET` `/api/reach/v1/profiles/{profileUuid}/forms`
**List forms**

Get a paginated list of the signup forms in a profile. Each form carries a reference to the template that renders it. Get the form details for a directly usable template URL and for the tags the form puts on the contacts it captures.

- **Chemin** : `profileUuid`
- **Query** : `page`, `per_page`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/forms/{formUuid}`
**Get form details**

Get a single form with the URL of its hosted template and the tags it applies to the contacts it captures. There is no ready-made embed snippet in the response - either serve the template HTML yourself or build your own embed around the form uuid.

- **Chemin** : `profileUuid`, `formUuid`

#### `DELETE` `/api/reach/v1/profiles/{profileUuid}/forms/{formUuid}`
**Delete form**

Permanently delete a form together with its template. A form that has already captured submissions cannot be deleted, so that the contacts it collected are never silently discarded - pause the form instead to stop it collecting new ones. Views alone do not block deletion.

- **Chemin** : `profileUuid`, `formUuid`


## Reach: Profiles

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles` | List Profiles |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/domains` | Get connected sending domain |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/domains/dns-status` | Get profile domain DNS status |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/features` | List plan feature access |
| `GET` | `/api/reach/v1/profiles/{profileUuid}/limits` | Get remaining plan limits |

#### `GET` `/api/reach/v1/profiles`
**List Profiles**

This endpoint returns all profiles available to the client, including their basic information.

- Aucun paramètre

#### `GET` `/api/reach/v1/profiles/{profileUuid}/domains`
**Get connected sending domain**

Get the sending domain connected to the profile, its verification status and any suspended sender addresses. Campaigns only go out once a domain is connected and active, so this is the cheapest way to check that precondition before building one. A profile with no domain connected returns the same shape with every field set to `null`. For the individual MX, SPF, DKIM and DMARC records behind the status, use the DNS st […]

- **Chemin** : `profileUuid`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/domains/dns-status`
**Get profile domain DNS status**

Retrieve the DNS configuration status for a profile's domain. This endpoint reports the state of MX, SPF, DKIM and DMARC records, including the actual records found and the suggested records required for correct email delivery.

- **Chemin** : `profileUuid`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/features`
**List plan feature access**

List which plan features the profile can use. This is the feature lock matrix, not a usage quota. `available` means the feature can be used right now and `locked` means it is not part of the base plan, so an upgrade is needed. For remaining emails, recipients and AI credits use the limits endpoint instead. Worth checking before building something that cannot be activated afterwards, such as an automation on a plan wi […]

- **Chemin** : `profileUuid`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/limits`
**Get remaining plan limits**

Get how much of the plan is left for the current period. Two things to keep in mind before you build alerting on this. The period is a calendar month rather than a billing anniversary, so the counters reset on the 1st no matter when the subscription started. And usage is tracked per order, so every profile on the same order shares one pool and reports the same numbers here. Only the current period is available, past  […]

- **Chemin** : `profileUuid`


## Reach: Segments

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

#### `GET` `/api/reach/v1/profiles/{profileUuid}/segmentation/filters/attributes`
**List segment filter attributes**

List every attribute a segment condition can filter on, with the operators each attribute accepts, the value format they expect and, where the value is constrained, the allowed values. The list is profile specific: it includes the profile's custom contact fields, its tags and its 20 most recently published campaigns, so the valid attributes cannot be hardcoded. Read it before creating or updating a segment to discove […]

- **Chemin** : `profileUuid`

#### `POST` `/api/reach/v1/profiles/{profileUuid}/segmentation/filters/contacts`
**Preview contacts matching conditions**

Preview the contacts matching a set of conditions without saving a segment. The body is the same set of conditions accepted when creating or updating a segment, so this is how to check who a filter reaches, and how many, before persisting it. Nothing is stored and no contact is modified. Call the segment filter attributes endpoint first to discover the valid `attribute`, `operator` and `value` combinations.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `conditions` — array<object> · **requis** — Conditions a contact must satisfy to appear in the preview
    - `logic` — enum · **requis** — How to combine multiple conditions
    - `page` — integer — Page number
    - `per_page` — integer — Number of items per page
    - `search` — string — Narrow the preview to contacts whose email matches
    - `sort_by` — enum
    - `sort_direction` — enum

#### `GET` `/api/reach/v1/profiles/{profileUuid}/segmentation/segments`
**List profile segments**

Get a paginated list of the segments defined in a profile. Each entry carries the number of contacts currently matching it, which is recalculated on read rather than stored. Use `count_type` to count either every matching contact or only the subscribed ones.

- **Chemin** : `profileUuid`
- **Query** : `count_type`, `page`, `per_page`

#### `POST` `/api/reach/v1/profiles/{profileUuid}/segmentation/segments`
**Create a profile segment**

Create a segment in a profile. A segment is a saved set of conditions rather than a fixed list, so its membership changes as contacts change. Creating one does not modify any contact.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `name` — string · **requis**
    - `conditions` — array<object> · **requis** — Conditions a contact must satisfy to fall into the segment
    - `logic` — enum · **requis** — How to combine multiple conditions

#### `GET` `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}`
**Get profile segment details**

Get a single segment of a profile, including the conditions that define it. To retrieve the contacts currently matching those conditions, use the segment contacts endpoint instead.

- **Chemin** : `profileUuid`, `segmentUuid`

#### `PUT` `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}`
**Update a profile segment**

Rename a segment and/or replace the conditions that define it. `name` is always required. Omit `conditions` to rename without touching the conditions; supply them and they replace the existing set entirely rather than being merged into it. Contacts are never modified, but which of them match the segment can change immediately.

- **Chemin** : `profileUuid`, `segmentUuid`
- **Corps** (requis) :
    - `name` — string · **requis**
    - `conditions` — array<object> — Replaces the existing conditions entirely. Omit to keep the current ones.
    - `logic` — enum — How to combine multiple conditions. Required when conditions are given.

#### `DELETE` `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}`
**Delete a profile segment**

Delete a segment. Only the segment definition is removed. The contacts that matched it are left untouched.

- **Chemin** : `profileUuid`, `segmentUuid`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}/contacts`
**List profile segment contacts**

Retrieve contacts associated with a specific segment for a given profile. This endpoint allows you to fetch and filter contacts that belong to a particular segment, identified by its UUID, scoped to a specific profile.

- **Chemin** : `profileUuid`, `segmentUuid`
- **Query** : `page`, `per_page`

#### `GET` `/api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}/count`
**Count profile segment contacts**

Count the contacts currently matching a segment without listing them. Cheaper than paging through the segment contacts endpoint when only the size is needed.

- **Chemin** : `profileUuid`, `segmentUuid`

#### `GET` `/api/reach/v1/segmentation/segments`
**List segments**

Get a list of all contact segments. This endpoint returns a list of contact segments that can be used to organize contacts. **Deprecated.** This endpoint cannot target a profile, so it always falls back to the client's default profile and cannot list the segments of any other profile. Use `GET /api/reach/v1/profiles/{profileUuid}/segmentation/segments` instead.

- Aucun paramètre

#### `POST` `/api/reach/v1/segmentation/segments`
**Create a new contact segment**

Create a new contact segment. This endpoint allows creating a new contact segment that can be used to organize contacts. The segment can be configured with specific criteria like email, name, subscription status, etc. **Deprecated.** This endpoint cannot target a profile, so it always falls back to the client's default profile and cannot create segments in any other profile. Use `POST /api/reach/v1/profiles/{profileU […]

- **Corps** (requis) :
    - `name` — string · **requis**
    - `conditions` — array<object> · **requis**
    - `logic` — enum · **requis**

#### `GET` `/api/reach/v1/segmentation/segments/{segmentUuid}`
**Get segment details**

Get details of a specific segment. This endpoint retrieves information about a single segment identified by UUID. Segments are used to organize and group contacts based on specific criteria. **Deprecated.** This endpoint cannot target a profile, so it always falls back to the client's default profile and cannot read segments of any other profile. Use `GET /api/reach/v1/profiles/{profileUuid}/segmentation/segments/{se […]

- **Chemin** : `segmentUuid`

#### `GET` `/api/reach/v1/segmentation/segments/{segmentUuid}/contacts`
**List segment contacts**

Retrieve contacts associated with a specific segment. This endpoint allows you to fetch and filter contacts that belong to a particular segment, identified by its UUID. **Deprecated.** This endpoint cannot target a profile, so it always falls back to the client's default profile and cannot read segments of any other profile. Use `GET /api/reach/v1/profiles/{profileUuid}/segmentation/segments/{segmentUuid}/contacts` i […]

- **Chemin** : `segmentUuid`
- **Query** : `page`, `per_page`


## Reach: Tags

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

#### `GET` `/api/reach/v1/profiles/{profileUuid}/tags`
**List profile tags**

Get all tags defined in a profile. Tags are the way contacts are grouped in Reach, and can be used to filter the contact list or to build segments.

- **Chemin** : `profileUuid`

#### `POST` `/api/reach/v1/profiles/{profileUuid}/tags`
**Create or find tags**

Create tags in a profile. Names that already exist in the profile are not duplicated: the existing tag is returned instead, so the call is safe to repeat. Every tag in the request is returned, whether it was created now or already existed.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `names` — array<string> · **requis**

#### `DELETE` `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}`
**Delete a tag**

Delete a tag and remove it from every contact carrying it. The contacts themselves are not deleted. This is idempotent: deleting a tag that does not exist in the profile still succeeds.

- **Chemin** : `profileUuid`, `tagUuid`

#### `PATCH` `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}`
**Rename a tag**

Rename a tag. The contacts assigned to the tag are unaffected. Names are unique within a profile, so renaming a tag to a name that is already taken is rejected.

- **Chemin** : `profileUuid`, `tagUuid`
- **Corps** (requis) :
    - `value` — string · **requis** — New tag name

#### `POST` `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts`
**Assign contacts to a tag**

Assign a tag to many contacts at once. Pass `contact_uuids` to target specific contacts, or `all_contacts` to target every contact in the profile. The work is queued, so a success response means it was accepted rather than finished. Contacts that already carry the tag are left alone.

- **Chemin** : `profileUuid`, `tagUuid`
- **Corps** (requis) :
    - `contact_uuids` — array<string> — Contacts to apply the change to. Required unless all_contacts is true.
    - `all_contacts` — boolean — Apply to every contact in the profile

#### `DELETE` `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts`
**Remove contacts from a tag**

Remove a tag from many contacts at once. Pass `contact_uuids` to target specific contacts, or `all_contacts` to target every contact in the profile. The work is queued, so a success response means it was accepted rather than finished. The tag itself and the contacts are not deleted.

- **Chemin** : `profileUuid`, `tagUuid`
- **Corps** (requis) :
    - `contact_uuids` — array<string> — Contacts to apply the change to. Required unless all_contacts is true.
    - `all_contacts` — boolean — Apply to every contact in the profile

#### `POST` `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts/{contactUuid}`
**Assign a contact to a tag**

Assign a tag to a single contact. Unlike the bulk endpoint this is applied immediately rather than queued. Assigning a tag the contact already carries succeeds without duplicating it.

- **Chemin** : `profileUuid`, `tagUuid`, `contactUuid`

#### `DELETE` `/api/reach/v1/profiles/{profileUuid}/tags/{tagUuid}/contacts/{contactUuid}`
**Remove a contact from a tag**

Remove a tag from a single contact. Unlike the bulk endpoint this is applied immediately rather than queued. Neither the tag nor the contact is deleted.

- **Chemin** : `profileUuid`, `tagUuid`, `contactUuid`


## Reach: Templates

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/reach/v1/profiles/{profileUuid}/templates` | List email templates |
| `POST` | `/api/reach/v1/profiles/{profileUuid}/templates` | Create an email template |

#### `GET` `/api/reach/v1/profiles/{profileUuid}/templates`
**List email templates**

Get a list of the email templates in a profile, most recently updated first. Templates are the reusable email bodies a campaign is built from. The list is not paginated and only the metadata is returned - the template content itself is not exposed. Use the `uuid` of a template as the `template_uuid` when creating a campaign.

- **Chemin** : `profileUuid`

#### `POST` `/api/reach/v1/profiles/{profileUuid}/templates`
**Create an email template**

Create an email template in a profile. The template holds the HTML body a campaign reuses, so it can be created before any campaign exists. Only the template metadata comes back - keep the returned `uuid` to reference it as the `template_uuid` of a campaign.

- **Chemin** : `profileUuid`
- **Corps** (requis) :
    - `template_content` — string · **requis** — The email body as HTML. It is sanitised before it is stored, so the saved template can differ from what was sent - inlin
    - `title` — string — Name the template is listed under. Not shown to the recipients.


## Ecommerce (`/api/ecommerce/v1`)

## Ecommerce: Discounts

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/discounts` | List discounts |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/discounts` | Create a discount |

#### `GET` `/api/ecommerce/v1/stores/{store_id}/discounts`
**List discounts**

List a store's discounts. Filter by free text over code and name, or by disabled state. Amounts for fixed discounts are integers in the smallest currency unit; percentage discounts carry a whole-number value between 1 and 100.

- **Chemin** : `store_id`
- **Query** : `q`, `is_disabled`, `page`

#### `POST` `/api/ecommerce/v1/stores/{store_id}/discounts`
**Create a discount**

Create a discount for a store. Fixed discounts take an amount in the smallest currency unit (e.g. $10 is 1000); percentage discounts take a whole-number value between 1 and 100. Free-shipping discounts ignore value. Returns the created discount.

- **Chemin** : `store_id`
- **Corps** (requis) :
    - `code` — string · **requis** — The discount code customers enter at checkout.
    - `name` — string — A human-friendly discount name.
    - `type` — enum · **requis** — The discount type.
    - `value` — integer · **requis** — For percentage discounts a whole number 1-100; for fixed discounts an amount in the smallest currency unit (e.g. $10 is 
    - `allocation` — enum — Whether the discount applies to the cart total or to each eligible item.
    - `starts_at` — string — When the discount becomes active. A bare date (2026-11-27) anchors to time_zone. Defaults to now when omitted.
    - `ends_at` — string — When the discount expires. A bare date runs to the end of that day in time_zone. Never expires when omitted.
    - `usage_limit` — integer — Maximum number of times the discount can be redeemed.
    - `min_cart_value` — integer — Minimum cart value in the smallest currency unit required for the discount to apply.
    - `time_zone` — string — IANA time zone used to interpret starts_at and ends_at.


## Ecommerce: Miscellaneous

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/miscellaneous/custom-storefront-instructions` | Get custom storefront setup instructions |

#### `GET` `/api/ecommerce/v1/miscellaneous/custom-storefront-instructions`
**Get custom storefront setup instructions**

Retrieve step-by-step setup instructions, formatted as Markdown, for connecting a custom sales channel to your store and keeping your catalog, orders, shipping and payments in sync through the Ecommerce API.

- Aucun paramètre


## Ecommerce: Orders

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/orders` | List store orders |
| `GET` | `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}` | Retrieve an order |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}/cancel` | Cancel an order |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}/fulfill` | Fulfil an order |

#### `GET` `/api/ecommerce/v1/stores/{store_id}/orders`
**List store orders**

List a store's orders newest first as summaries. Filter by status, payment or fulfilment status, customer email, order number or a free-text query. Amounts are in the smallest currency unit. Retrieve a single order for its line items, addresses and fulfilments.

- **Chemin** : `store_id`
- **Query** : `status`, `payment_status`, `fulfillment_status`, `email`, `display_id`, `q`, `created_at_from`, `created_at_to`, `page`

#### `GET` `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}`
**Retrieve an order**

Retrieve one order in full: line items (each with the id the fulfil endpoint needs), addresses, the totals breakdown and fulfilments with tracking. Amounts are in the smallest currency unit.

- **Chemin** : `store_id`, `order_id`

#### `POST` `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}/cancel`
**Cancel an order**

Cancel the order and optionally email the customer. Returns the updated order summary.

- **Chemin** : `store_id`, `order_id`
- **Corps** (requis) :
    - `notify_customer` — boolean — Whether to email the customer about the cancellation. Defaults to true.

#### `POST` `/api/ecommerce/v1/stores/{store_id}/orders/{order_id}/fulfill`
**Fulfil an order**

Create a fulfilment for the order and attach tracking in one call. Omit items to fulfil every remaining unfulfilled item. Returns the updated order summary.

- **Chemin** : `store_id`, `order_id`
- **Corps** (requis) :
    - `items` — array<object> — Line items to fulfil. Omit to fulfil every remaining unfulfilled item.
    - `tracking_number` — string — Carrier tracking number for the shipment.
    - `tracking_url` — string — Public tracking URL for the shipment. Requires tracking_number.
    - `notify_customer` — boolean — Whether to email the customer about the fulfilment. Defaults to true.


## Ecommerce: Payments

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/ecommerce/v1/stores/{store_id}/payment-methods/manual` | Enable manual payment method |
| `GET` | `/api/ecommerce/v1/stores/{store_id}/payment-providers` | List store payment providers |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/payment-providers/{provider_id}/connect-link` | Create a payment provider connect link |

#### `POST` `/api/ecommerce/v1/stores/{store_id}/payment-methods/manual`
**Enable manual payment method**

Enable a manual payment method so the store can accept orders without an online payment provider.

- **Chemin** : `store_id`
- **Corps** (requis) :
    - `title` — string — Optional display name shown to customers at checkout.

#### `GET` `/api/ecommerce/v1/stores/{store_id}/payment-providers`
**List store payment providers**

List a store's payment providers, split into providers already connected to the store and gateways available to install. Never exposes gateway credentials, secrets, or configuration.

- **Chemin** : `store_id`
- **Query** : `include_currency_unsupported`

#### `POST` `/api/ecommerce/v1/stores/{store_id}/payment-providers/{provider_id}/connect-link`
**Create a payment provider connect link**

Create an onboarding link for connecting a payment gateway to the store. Returns the gateway onboarding URL for the merchant to open and a deep-link into the store admin.

- **Chemin** : `store_id`, `provider_id`


## Ecommerce: Product variants

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants` | List product variants |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants` | Create a product variant |
| `PATCH` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants/batch` | Update product variants in batch |
| `DELETE` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants/{variant_id}` | Delete a product variant |

#### `GET` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants`
**List product variants**

List a product's variants, ordered by rank, with their options, prices and inventory. Prices are integers in the smallest currency unit and live on variants.

- **Chemin** : `store_id`, `product_id`
- **Query** : `page`

#### `POST` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants`
**Create a product variant**

Add a variant to a product along one or more option dimensions (e.g. Size, Color). Options missing from the product are created automatically; provide a value for every option the product already has. Prices are integers in the smallest currency unit and default to the store currency. Returns the created variant.

- **Chemin** : `store_id`, `product_id`
- **Corps** (requis) :
    - `title` — string — The variant title. Defaults to the option values joined with ' / ' (e.g. 'Red / L').
    - `sku` — string — The variant SKU.
    - `options` — array<object> · **requis** — Option name/value pairs that distinguish this variant, e.g. [{name: Size, value: M}]. Options missing from the product a
    - `prices` — array<object> — Prices per currency. Amounts are integers in the smallest currency unit. A free item is amount: 0.
    - `inventory_quantity` — integer — Units in stock. Defaults to 0.
    - `manage_inventory` — boolean — Whether stock is tracked for this variant. Defaults to false.

#### `PATCH` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants/batch`
**Update product variants in batch**

Update up to 100 existing variants in place by id — title, inventory, stock tracking and prices. Variants omitted from the request are left untouched. Prices replace the variant's existing prices in full. Returns the updated variants.

- **Chemin** : `store_id`, `product_id`
- **Corps** (requis) :
    - `variants` — array<object> · **requis** — Variants to update in place by id, up to 100. Variants omitted from the list are left untouched.

#### `DELETE` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/variants/{variant_id}`
**Delete a product variant**

Delete a single variant from the product.

- **Chemin** : `store_id`, `product_id`, `variant_id`


## Ecommerce: Products

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/products` | List products |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/digital` | Create digital product |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/physical` | Create physical product |
| `DELETE` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}` | Delete a product |
| `PATCH` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}` | Update a product |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/images` | Upload and attach a product image |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/images/upload-url` | Create a product image upload URL |

#### `GET` `/api/ecommerce/v1/stores/{store_id}/products`
**List products**

List a store's products newest first as lean summaries (name, status, thumbnail, variant count and price range). Prices are integers in the smallest currency unit and live on variants. Filter by status, free text or a set of product ids. Use include=variants to embed each product's variants with prices and inventory, and include=media to embed its media.

- **Chemin** : `store_id`
- **Query** : `product_ids`, `status`, `q`, `include`, `page`

#### `POST` `/api/ecommerce/v1/stores/{store_id}/products/digital`
**Create digital product**

Create a published digital product with a single variant and an optional external download link.

- **Chemin** : `store_id`
- **Corps** (requis) :
    - `name` — string · **requis** — The product name.
    - `price` — integer · **requis** — Price in the smallest currency unit (e.g. cents). Must be positive.
    - `description` — string — The product description.
    - `currency` — string — ISO 4217 currency code. Defaults to the store's default currency when omitted.
    - `download_url` — string — Optional external download link delivered to the customer after purchase.

#### `POST` `/api/ecommerce/v1/stores/{store_id}/products/physical`
**Create physical product**

Create a published physical product with a single variant priced in the store currency.

- **Chemin** : `store_id`
- **Corps** (requis) :
    - `name` — string · **requis** — The product name.
    - `price` — integer · **requis** — Price in the smallest currency unit (e.g. cents). Must be positive.
    - `description` — string — The product description.
    - `currency` — string — ISO 4217 currency code. Defaults to the store's default currency when omitted.

#### `DELETE` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}`
**Delete a product**

Delete a product and its variants from the store. A subscription product with active subscribers is archived instead of deleted so its data stays available.

- **Chemin** : `store_id`, `product_id`

#### `PATCH` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}`
**Update a product**

Update a product's name, description or status. Set status to published to make it buyable, draft to hide it, or archived to retire it. Variants, prices and inventory are managed through the variant endpoints, not here. Returns the updated product summary.

- **Chemin** : `store_id`, `product_id`
- **Corps** (requis) :
    - `name` — string — The product name.
    - `description` — string — The product description.
    - `status` — enum — Set "published" to make the product buyable, "draft" to hide it, or "archived" to retire it.

#### `POST` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/images`
**Upload and attach a product image**

Fetch a raster image (JPEG, PNG, GIF or WebP, max 15MB) from a URL and attach it to a product in a single call. Image downloads require HTTPS on port 443 without embedded credentials. At most one redirect is allowed, and its destination must meet the same requirements. Private or reserved network destinations, unsupported URLs and longer redirect chains are rejected. The image is virus-scanned and validated by conten […]

- **Chemin** : `store_id`, `product_id`
- **Corps** (requis) :
    - `image_url` — string — Publicly reachable URL of a raster image (JPEG, PNG, GIF or WebP), maximum 15MB. Fetching the image requires HTTPS on po
    - `object_name` — string — Key returned by the upload-url endpoint. Provide this instead of image_url to attach an uploaded image.
    - `is_thumbnail` — boolean — When true, the image becomes the product's thumbnail (primary image). When omitted, it becomes the thumbnail only if the

#### `POST` `/api/ecommerce/v1/stores/{store_id}/products/{product_id}/images/upload-url`
**Create a product image upload URL**

Returns a signed URL to upload a product image to (multipart/form-data POST). Then call the attach-image endpoint with the returned object_name to scan and attach it to the product.

- **Chemin** : `store_id`, `product_id`


## Ecommerce: Sales channels

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores/{store_id}/sales-channels` | List sales channels |
| `POST` | `/api/ecommerce/v1/stores/{store_id}/sales-channels` | Create a sales channel |
| `PATCH` | `/api/ecommerce/v1/stores/{store_id}/sales-channels/{sales_channel_id}` | Update sales channel |

#### `GET` `/api/ecommerce/v1/stores/{store_id}/sales-channels`
**List sales channels**

List a store's active sales channels with their full metadata.

- **Chemin** : `store_id`

#### `POST` `/api/ecommerce/v1/stores/{store_id}/sales-channels`
**Create a sales channel**

Create a sales channel for a store. A "custom" channel is headless: build your own frontend and keep your catalog, orders, shipping and payments in sync through the Ecommerce API. A "quick-link" channel is a hosted one-page store whose handle is auto-generated.

- **Chemin** : `store_id`
- **Corps** (requis) :
    - `type` — enum · **requis** — Sales channel type. "custom" is a headless channel: it requires a name and takes an optional public url. "quick-link" is
    - `name` — string — Merchant-facing custom name. Required for custom channels; not supported for quick-link.
    - `url` — string — Optional public url for the channel. Custom channels only; not supported for quick-link.

#### `PATCH` `/api/ecommerce/v1/stores/{store_id}/sales-channels/{sales_channel_id}`
**Update sales channel**

Update a custom sales channel. The merchant-facing `name` and the public `url` (returned as the channel `domain`) can be changed. Pass `null` to clear a value.

- **Chemin** : `store_id`, `sales_channel_id`
- **Corps** (requis) :
    - `name` — string — Merchant-facing custom name shown in the sales channels list. Pass null to clear it.
    - `url` — string — Public address where the custom sales channel lives. Pass null to clear it.


## Ecommerce: Shipping

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/ecommerce/v1/stores/{store_id}/shipping` | Set store shipping |

#### `POST` `/api/ecommerce/v1/stores/{store_id}/shipping`
**Set store shipping**

Set the flat-rate shipping price for a store, creating the shipping zone if it does not exist yet.

- **Chemin** : `store_id`
- **Corps** (requis) :
    - `price` — integer · **requis** — Flat shipping rate in the smallest currency unit (e.g. cents). Use 0 for free shipping.


## Ecommerce: Stores

| Méthode | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ecommerce/v1/stores` | Get stores |
| `POST` | `/api/ecommerce/v1/stores` | Create store |
| `DELETE` | `/api/ecommerce/v1/stores/{store_id}` | Delete store |
| `GET` | `/api/ecommerce/v1/stores/{store_id}/metadata` | Get store metadata |

#### `GET` `/api/ecommerce/v1/stores`
**Get stores**

Retrieve the stores associated with your account.

- **Query** : `page`

#### `POST` `/api/ecommerce/v1/stores`
**Create store**

Create a new store for your account. A primary sales channel is created alongside the store.

- **Corps** (requis) :
    - `name` — string
    - `country_code` — string — ISO 3166-1 alpha-2 country code.
    - `company_email` — string
    - `company_name` — string
    - `language` — string — ISO 639-1 language code.
    - `sales_channel` — object

#### `DELETE` `/api/ecommerce/v1/stores/{store_id}`
**Delete store**

Soft-delete a store owned by your account. The underlying store data is preserved; only the store is marked as deleted.

- **Chemin** : `store_id`

#### `GET` `/api/ecommerce/v1/stores/{store_id}/metadata`
**Get store metadata**

Get a store's readiness metadata: whether payment methods and shipping are configured, plus its default currency. Useful to verify prerequisites before building a storefront.

- **Chemin** : `store_id`

