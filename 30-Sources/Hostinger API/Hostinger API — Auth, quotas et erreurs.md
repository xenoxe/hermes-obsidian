---
tags: [hostinger, api, reference]
---

# Hostinger API — Auth, quotas et erreurs

Retour à l'index : [[Hostinger API — Index]]

## Clé API

1. Générer un jeton dans **hPanel → API** (<https://hpanel.hostinger.com/profile/api>)
2. Le jeton hérite des permissions de l'utilisateur qui l'a créé ; il peut avoir une date d'expiration.
3. Envoyer le jeton en en-tête `Authorization` sur **chaque** requête.

```bash
# La variable est lue depuis .env — jamais écrite en clair dans une note
curl https://developers.hostinger.com/api/vps/v1/virtual-machines \
  -H "Authorization: Bearer $HOSTINGER_API_KEY" \
  -H "Content-Type: application/json"
```

> [!danger] Règle absolue
> Aucune clé, aucun jeton et aucun `.env` ne doit être écrit dans ce vault (donc jamais poussé sur GitHub), même dans un dépôt privé, même pour « tester ». Le vault a un garde-fou automatique qui bloque les commits et les push contenant un secret.

## Forme des requêtes

- **Base URL** : `https://developers.hostinger.com`
- Chaque chemin est préfixé par `/api/`, le produit et la version : `/api/vps/v1/virtual-machines`
- Les versions sont **par endpoint** : les chemins `v1` continuent de fonctionner quand d'autres apparaissent
- `Content-Type: application/json` obligatoire ; les réponses sont toujours du JSON

### Trois emplacements de paramètres

| Type | Exemple | Notes |
|---|---|---|
| Chemin | `/api/vps/v1/virtual-machines/{virtualMachineId}` | Toujours requis |
| Query | `?page=2&per_page=50` | Optionnels (filtres, recherche, pagination) |
| Corps | objet JSON unique pour `POST`/`PUT`/`PATCH` | Structure documentée par endpoint |

### Réponse asynchrone

Un **`202 Accepted`** signifie que la requête est acceptée mais se termine en arrière-plan (typiquement les achats, pendant le paiement). **Poller la ressource** plutôt que de renvoyer la requête.

## Limite de débit (rate limit)

- **90 requêtes par minute** (certains comptes ont une limite personnalisée)
- Les requêtes authentifiées sont comptées **par utilisateur** : le même jeton utilisé depuis plusieurs machines partage le même budget
- Les requêtes non authentifiées sont comptées **par adresse IP**
- Certains endpoints (vérification de disponibilité de domaine) ont un compteur séparé
- Dépassement → **`429 Too Many Requests`** ; les réponses authentifiées portent les en-têtes `X-RateLimit-Limit`, `X-RateLimit-Remaining` (et l'horodatage de réinitialisation)

## Pagination

Les endpoints de liste qui peuvent renvoyer de grandes collections sont paginés (**43 endpoints** acceptent `page`). Ceux qui renvoient une collection bornée (datacenters, templates OS, cron jobs d'un site) renvoient tout d'un coup.

| Paramètre | Type | Défaut | Remarque |
|---|---|---|---|
| `page` | entier | `1` | Page à renvoyer |
| `per_page` | entier | `25` | Éléments par page, **maximum 100** |

La réponse contient un objet `meta` (total d'éléments, pagination courante) pour savoir quand s'arrêter.

## Walking every page

Keep requesting until you've collected `total` items, or until a page comes back empty.

```bash
page=1
while :; do
  body=$(curl -s "https://developers.hostinger.com/api/hosting/v1/websites?page=$page&per_page=100" \
    -H "Authorization: Bearer $HOSTINGER_API_TOKEN")

  count=$(echo "$body" | jq '.data | length')
  [ "$count" -eq 0 ] && break

  echo "$body" | jq -r '.data[].domain'
  page=$((page + 1))
done
```

In Python, the same loop against the [SDK](/api-reference/sdks.md):

```python
page = 1
while True:
    response = websites.list_websites_v1(page=page)
    if not response.data:
        break
    for website in response.data:
        print(website.domain)
    page += 1
```

> Each page is a separate request and counts against your [rate limit](/api-reference/overview.md#rate-limits). Use `per_page=100` when you're paging through a large collection — it's the difference between 2 requests and 8 for the same 200 items.

## Erreurs

Toute réponse d'erreur est du JSON, quel que soit le code. Forme générale :

```json
{
  "message": "Unauthenticated.",
  "correlation_id": "26a91bd9-f8c8-4a83-9df9-83e23d696fe3"
}
```

- `message` — description lisible, destinée au développeur qui lit les logs (pas à afficher à un utilisateur final)
- `correlation_id` — identifiant unique de la requête, **à citer au support** ; renvoyé aussi dans l'en-tête `x-correlation-id`

### Erreurs de validation (422)

Un `422 Unprocessable Content` = requête bien formée mais valeurs invalides. La réponse ajoute un objet `errors` indexé par nom de champ, chaque valeur étant la liste des problèmes :

```json
{
  "message": "The name field is required. (and 1 more error)",
  "errors": {
    "name": ["The name field is required."],
    "port": ["The port must be a number."]
  },
  "correlation_id": "26a91bd9-f8c8-4a83-9df9-83e23d696fe3"
}
```

`message` résume le premier problème ; lire `errors` pour attribuer les échecs aux bons champs.

## Status codes

| Code                        | Meaning                                                                             | What to do                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| --------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `200 OK`                    | The request succeeded.                                                              | —                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `201 Created`               | A resource was created.                                                             | Read the new resource from the response body.                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `202 Accepted`              | A payment is being processed and the order will complete asynchronously.            | Poll the relevant resource until it appears. Returned by the purchase endpoints for [billing orders](/api-reference/endpoints/billing/orders/create-purchase-order.md), [subscription renewals](/api-reference/endpoints/billing/subscriptions/renew-subscription.md), [domain registration](/api-reference/endpoints/domains/portfolio/purchase-new-domain.md), and [VPS creation](/api-reference/endpoints/vps/virtual-machine/purchase-new-virtual-machine.md). |
| `400 Bad Request`           | The request couldn't be processed as sent.                                          | Check the `message`; fix the request.                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `401 Unauthorized`          | The token is missing, malformed, expired, or revoked.                               | Check the `Authorization` header and the token's status in [hPanel → API](https://hpanel.hostinger.com/profile/api).                                                                                                                                                                                                                                                                                                                                               |
| `404 Not Found`             | The route doesn't exist, or the resource doesn't exist on your account.             | Verify the path and any IDs in it.                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `409 Conflict`              | The resource is in a state that doesn't allow this operation, or it already exists. | Read the `message` — it names the conflict.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `422 Unprocessable Content` | Validation failed.                                                                  | Read the `errors` object.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `429 Too Many Requests`     | You exceeded the [rate limit](/api-reference/overview.md#rate-limits).              | Back off and retry after the window resets.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `500 Internal Server Error` | Something failed on Hostinger's side.                                               | Retry; if it persists, [report it](/api-reference/support.md) with the correlation ID.                                                                                                                                                                                                                                                                                                                                                                             |
| `502 Bad Gateway`           | An upstream service was unreachable.                                                | Retry with backoff.                                                                                                                                                                                                                                                                                                                                                                                                                                                |

> The API doesn't use `403`. A permission problem surfaces as `401` if the token itself is rejected, or as `404` if the token is valid but the resource isn't visible to the user who owns it. Tokens inherit the permissions of the user who created them — see [Security & Access](/account/security.md).

## Sources officielles

- Vue d'ensemble : <https://docs.hostinger.com/api-reference/overview>
- Pagination : <https://docs.hostinger.com/api-reference/pagination>
- Erreurs : <https://docs.hostinger.com/api-reference/errors>
- Support : <https://docs.hostinger.com/api-reference/support>
