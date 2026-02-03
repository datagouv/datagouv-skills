# data.gouv.fr Main API Reference

Complete API reference for the main data.gouv.fr API (uData). Cross-checked with the [Swagger](https://www.data.gouv.fr/api/1/swagger.json) and the [official guides](https://guides.data.gouv.fr/guide-data.gouv.fr/readme-1/reference/).

## Base URL

- Production: `https://www.data.gouv.fr/api/1/`
- Demo: `https://demo.data.gouv.fr/api/1/`

## Authentication

Read operations are public. For write operations (POST, PUT, PATCH, DELETE), include header `X-API-KEY`:

```
X-API-KEY: your-api-key
```

API key: https://www.data.gouv.fr/fr/admin/me/ — same permissions as web (e.g. org member to edit org datasets). Use `private: true` to create or keep a dataset as draft; otherwise API publishes it.

**URL identifiers:** use technical id or slug in paths; prefer technical id for scripts (slug can change).

**Optional header `X-Fields`:** field mask to limit returned fields (e.g. `{id,title,slug}`). Applies to many endpoints.

---

## Endpoints

### Datasets

#### List datasets
```
GET /datasets/
```
**Query:** `q`, `page`, `page_size` (default 20), `sort` (e.g. `-created`, `title`, `last_update`, `reuses`, `followers`, `views`), `organization`, `owner`, `tag`, `license`, `featured`, `reuse`, `dataset`, `format`, `geozone`, `granularity`, `temporal_coverage`, `schema`, `topic`, `archived`, `deleted`, `private` (auth required for last three), etc.

#### Create dataset
```
POST /datasets/
```
**Responses:** 201, 400. **Body (JSON):** `title` (required), `description` (required), `frequency` (required, default `unknown`), `last_update` (required), `organization` or `owner`, `license`, `tags`, `private`, `resources`, `extras`, etc.

#### Get dataset
```
GET /datasets/{dataset}/
```

#### Update dataset
```
PUT /datasets/{dataset}/
```

#### Delete dataset
```
DELETE /datasets/{dataset}/
```
**Query:** `send_legal_notice` (boolean, admin only). **Responses:** 204, 404, 410.

#### Dataset resources

Resources are embedded in the dataset object (`GET /datasets/{dataset}/`). To create or reorder:

```
POST /datasets/{dataset}/resources/
```
Create a new resource. **Body:** `title`, `url`, `filetype` (file|remote), `format`, `type` (main|documentation|api|etc).

```
PUT /datasets/{dataset}/resources/
```
Reorder resources. **Body:** array of resource objects in desired order.

#### Get resource
```
GET /datasets/{dataset}/resources/{rid}/
```

#### Update resource
```
PUT /datasets/{dataset}/resources/{rid}/
```

#### Delete resource
```
DELETE /datasets/{dataset}/resources/{rid}/
```

#### Upload resource file
```
POST /datasets/{dataset}/resources/{rid}/upload/
POST /datasets/{dataset}/upload/
```
`multipart/form-data` with file.

#### Dataset badges
```
POST /datasets/{dataset}/badges/
DELETE /datasets/{dataset}/badges/{badge_kind}/
```

#### Dataset featured
```
POST /datasets/{dataset}/featured/
DELETE /datasets/{dataset}/featured/
```

#### Dataset followers
```
GET /datasets/{id}/followers/
POST /datasets/{id}/followers/
DELETE /datasets/{id}/followers/
```

#### Dataset RDF
```
GET /datasets/{dataset}/rdf
GET /datasets/{dataset}/rdf.{_format}
```

#### Community resources

Resources submitted by the community (not the dataset owner). **Query param `dataset`** (optional) on GET/PUT/DELETE/upload when the community resource ID is ambiguous.

```
GET /datasets/community_resources/
POST /datasets/community_resources/
GET /datasets/community_resources/{community}/
PUT /datasets/community_resources/{community}/
DELETE /datasets/community_resources/{community}/
POST /datasets/community_resources/{community}/upload/
POST /datasets/{dataset}/upload/community/
```
Upload endpoints use `multipart/form-data`.

#### Reference data
```
GET /datasets/badges/
GET /datasets/frequencies/
GET /datasets/licenses/
GET /datasets/resource_types/
GET /datasets/extensions/
GET /datasets/schemas/
```

#### Suggestions
```
GET /datasets/suggest/
```
**Query:** `q` (required), `size` (optional).

```
GET /datasets/suggest/formats/
GET /datasets/suggest/mime/
```
**Query:** `q` (required), `size` (optional).

#### Redirect to latest resource version
```
GET /datasets/r/{id}
```
Redirects to the latest version of a resource given its ID. **Query:** `dataset` (optional).

#### Atom feed
```
GET /datasets/recent.atom
```

---

### Organizations

#### List organizations
```
GET /organizations/
```
**Query:** `q`, `page`, `page_size`, `sort`, `badge`, etc.

#### Create organization
```
POST /organizations/
```

#### Get organization
```
GET /organizations/{org}/
```

#### Update organization
```
PUT /organizations/{org}/
```

#### Delete organization
```
DELETE /organizations/{org}/
```

#### Members
```
POST /organizations/{org}/member/{user}/
PUT /organizations/{org}/member/{user}/
DELETE /organizations/{org}/member/{user}/
```
**Member body:** `role` — `admin` or `editor`. Add, update role, or remove member.

#### Membership requests
```
GET /organizations/{org}/membership/
POST /organizations/{org}/membership/
POST /organizations/{org}/membership/{id}/accept/
POST /organizations/{org}/membership/{id}/refuse/
```

#### Organization datasets, reuses, discussions
```
GET /organizations/{org}/datasets/
GET /organizations/{org}/reuses/
GET /organizations/{org}/discussions/
GET /organizations/{org}/contacts/
GET /organizations/{org}/contacts/suggest/
```

#### Exports CSV
```
GET /organizations/{org}/datasets.csv
GET /organizations/{org}/dataservices.csv
GET /organizations/{org}/datasets-resources.csv
GET /organizations/{org}/discussions.csv
```

#### Catalog
```
GET /organizations/{org}/catalog
GET /organizations/{org}/catalog.{_format}
```

#### Badges, logo
```
GET /organizations/badges/
POST /organizations/{org}/badges/
DELETE /organizations/{org}/badges/{badge_kind}/
POST /organizations/{org}/logo/
PUT /organizations/{org}/logo/
```

#### Reference & suggestions
```
GET /organizations/roles/
GET /organizations/suggest/
```

#### Followers
```
GET /organizations/{id}/followers/
POST /organizations/{id}/followers/
DELETE /organizations/{id}/followers/
```

---

### Users

#### List users
```
GET /users/
```
**Query:** `q`, `page`, `page_size`, `sort`, etc.

#### Create user
```
POST /users/
```

#### Get user
```
GET /users/{user}/
```

#### Update user
```
PUT /users/{user}/
```

#### Delete user
```
DELETE /users/{user}/
```

#### Avatar
```
POST /users/{user}/avatar/
```

#### User contacts
```
GET /users/{user}/contacts/
```

#### Reference & suggestions
```
GET /users/roles/
GET /users/suggest/
```

#### Followers
```
GET /users/{id}/followers/
POST /users/{id}/followers/
DELETE /users/{id}/followers/
```

---

### Me (authenticated user)

```
GET /me/
PUT /me/
DELETE /me/
```
Returns/updates current user. `GET` includes `apikey` if present.

```
DELETE /me/apikey/
POST /me/apikey/
```
Manage API key.

```
POST /me/avatar/
```
Upload avatar.

```
GET /me/datasets/
GET /me/reuses/
GET /me/metrics/
GET /me/org_datasets/
GET /me/org_reuses/
GET /me/org_community_resources/
GET /me/org_discussions/
```
User's own content and metrics.

---

### Reuses

#### List reuses
```
GET /reuses/
```
**Query:** `q`, `page`, `page_size`, `sort`, `organization`, `owner`, `tag`, `topic`, `type`, `dataset`, `featured`, etc.

#### Create reuse
```
POST /reuses/
```
**Body:** `title`, `description`, `url`, `topic`, `type` (required), `organization` or `owner`, `datasets`, `dataservices`, `tags`, etc.

#### Get reuse
```
GET /reuses/{reuse}/
```

#### Update reuse
```
PUT /reuses/{reuse}/
```

#### Delete reuse
```
DELETE /reuses/{reuse}/
```

#### Link datasets/dataservices
```
POST /reuses/{reuse}/datasets/
POST /reuses/{reuse}/dataservices/
```

#### Badges, featured, image
```
GET /reuses/badges/
POST /reuses/{reuse}/badges/
DELETE /reuses/{reuse}/badges/{badge_kind}/
POST /reuses/{reuse}/featured/
DELETE /reuses/{reuse}/featured/
POST /reuses/{reuse}/image/
```

#### Reference & suggestions
```
GET /reuses/topics/
GET /reuses/types/
GET /reuses/suggest/
```

#### Followers, Atom
```
GET /reuses/{id}/followers/
POST /reuses/{id}/followers/
DELETE /reuses/{id}/followers/
GET /reuses/recent.atom
```

---

### Dataservices

#### List dataservices
```
GET /dataservices/
```
**Query:** `q`, `page`, `page_size`, `sort`, `owner`, `organization`, `topic`, `reuse`, `dataset`, `tag`, `access_type`, `featured`, `contact_point`, etc.

#### Create dataservice
```
POST /dataservices/
```
**Body:** `title` (required), `description`, `base_api_url`, `organization` or `owner`, `format` (REST, WMS, WSL), `access_type`, `tags`, etc.

#### Get dataservice
```
GET /dataservices/{dataservice}/
```

#### Update dataservice
```
PATCH /dataservices/{dataservice}/
```

#### Delete dataservice
```
DELETE /dataservices/{dataservice}/
```
**Query:** `send_legal_notice` (boolean, admin only).

#### Datasets
```
POST /dataservices/{dataservice}/datasets/
```
**Body:** array of `{id: dataset_id}`.

```
DELETE /dataservices/{dataservice}/datasets/{dataset}/
```

#### Featured
```
POST /dataservices/{dataservice}/featured/
DELETE /dataservices/{dataservice}/featured/
```

#### RDF
```
GET /dataservices/{dataservice}/rdf
GET /dataservices/{dataservice}/rdf.{_format}
```

#### Followers, Atom
```
GET /dataservices/{id}/followers/
POST /dataservices/{id}/followers/
DELETE /dataservices/{id}/followers/
GET /dataservices/recent.atom
```

---

### Contacts (contact points)

#### Create contact point
```
POST /contacts/
```
**Body (JSON):** `name` (required), `email` (required), `role` (required), `contact_form` (optional), `organization` (optional), `owner` (optional).

#### List contact roles
```
GET /contacts/roles/
```

#### Get contact point
```
GET /contacts/{contact_point}/
```

#### Update contact point
```
PUT /contacts/{contact_point}/
```

#### Delete contact point
```
DELETE /contacts/{contact_point}/
```

---

### Harvest (moissonnage)

#### List harvest sources
```
GET /harvest/sources/
```
**Query:** `page`, `page_size`, `owner`, `deleted`, `q`, etc.

#### Create harvest source
```
POST /harvest/sources/
```
**Body:** `name` (required), `url` (required), `backend` (required: `ckan`, `csw-dcat`, `csw-iso-19139`, `dcat`, `dkan`, `maaf`), `active` (default false), `autoarchive` (default true), `description`, `config`, `organization`, `owner`.

#### Get harvest source
```
GET /harvest/source/{source}/
```

#### Update harvest source
```
PUT /harvest/source/{source}/
```

#### Delete harvest source
```
DELETE /harvest/source/{source}/
```

#### Jobs
```
GET /harvest/source/{source}/jobs/
GET /harvest/job/{ident}/
POST /harvest/source/{source}/run/
```

#### Schedule
```
POST /harvest/source/{source}/schedule/
DELETE /harvest/source/{source}/schedule/
```

#### Preview & validate
```
GET /harvest/source/{source}/preview/
POST /harvest/source/preview/
POST /harvest/source/{source}/validate/
```

#### Backends
```
GET /harvest/backends/
```

---

### Discussions

#### List discussions
```
GET /discussions/
```

#### Create discussion
```
POST /discussions/
```
**Body:** `title` (required), `comment` (required), `subject` (required: `{class, id}`), `organization` (optional).

#### Get discussion
```
GET /discussions/{id}/
```

#### Update discussion
```
PUT /discussions/{id}/
```

#### Delete discussion
```
DELETE /discussions/{id}/
```

#### Reply to discussion
```
POST /discussions/{id}/
```
**Body:** `comment` (required), `close` (optional, boolean — only subject owner can close).

#### Edit/delete comment
```
PUT /discussions/{id}/comments/{cidx}/
DELETE /discussions/{id}/comments/{cidx}/
```

#### Spam
```
DELETE /discussions/{id}/comments/{cidx}/spam/
DELETE /discussions/{id}/spam/
```

---

### Notifications

```
GET /notifications/
POST /notifications/{notification}/read/
```

---

### Posts (actualités / pages)

#### List posts
```
GET /posts/
```

#### Create post
```
POST /posts/
```
**Body:** `name` (required), `content`, `body_type` (markdown, html, blocs), `kind` (news, page), `datasets` (IDs), `reuses` (IDs), `tags`, `image_url`, etc.

#### Get post
```
GET /posts/{post}/
```

#### Update post
```
PUT /posts/{post}/
```

#### Delete post
```
DELETE /posts/{post}/
```

#### Image, publish
```
POST /posts/{post}/image/
PUT /posts/{post}/image/
POST /posts/{post}/publish/
DELETE /posts/{post}/publish/
```

#### Atom
```
GET /posts/recent.atom
```

---

### Pages

```
GET /pages/
POST /pages/
GET /pages/{page}/
PUT /pages/{page}/
```

---

### Reports (signalements)

```
GET /reports/
POST /reports/
GET /reports/reasons/
GET /reports/{report}/
PATCH /reports/{report}/
```
**Body create:** `reason` (required: explicit_content, illegal_content, others, personal_data, security, spam), `subject` (optional: `{class, id}`), `message` (optional). **Body patch:** `dismissed_at`, `dismissed_by`, `message`, etc. Admin only for list/patch.

---

### Transfer (transferts de propriété)

```
GET /transfer/
POST /transfer/
GET /transfer/{id}/
POST /transfer/{id}/
```
**Body create:** `subject` (required: `{class, id}`), `recipient` (required: `{class, id}`), `comment` (required). **Body respond:** `response` (accept|refuse), `comment` (optional).

---

### Activity

```
GET /activity/
```
**Query:** `page`, `page_size`, `user`, `organization`, `related_to`. Returns site activity feed.

---

### Site

```
GET /site/
PATCH /site/
GET /site/catalog
GET /site/catalog.{_format}
GET /site/context.jsonld
GET /site/data.{_format}
```
**CSV exports:**
```
GET /site/datasets.csv
GET /site/dataservices.csv
GET /site/reuses.csv
GET /site/organizations.csv
GET /site/resources.csv
GET /site/harvests.csv
GET /site/tags.csv
```

---

### Access type

```
GET /access_type/reason_categories/
```
Returns limitation reason categories for restricted access.

---

### Avatars

```
GET /avatars/{identifier}/{size}/
```
Returns deterministic avatar for identifier at given size.

---

### Spatial (zones géographiques)

```
GET /spatial/granularities/
GET /spatial/levels/
GET /spatial/coverage/{level}/
GET /spatial/zone/{id}/
GET /spatial/zone/{id}/datasets/
GET /spatial/zones/{ids}/
GET /spatial/zones/suggest/
```

---

### Tags

```
GET /tags/suggest/
```

---

### Spam

```
GET /spam/
```

---

### ProConnect (auth)

```
GET /proconnect/auth
GET /proconnect/login/
GET /proconnect/logout
GET /proconnect/logout_oauth
```

---

### Workers (jobs planifiés — admin)

```
GET /workers/jobs/
POST /workers/jobs/
GET /workers/jobs/schedulables/
GET /workers/jobs/{id}/
PUT /workers/jobs/{id}/
DELETE /workers/jobs/{id}/
GET /workers/tasks/{id}/
```

---

## Response format

JSON in/out (`application/json`). File upload endpoints accept `multipart/form-data`, return JSON.

**Paginated lists:** `data`, `page`, `page_size`, `total`, `next_page`, `previous_page` (null when none).

---

## Error handling

- 200: success
- 201: created
- 204: no content (deleted)
- 400: invalid request / validation error
- 401: authentication required
- 403: insufficient permissions
- 404: not found
- 410: gone (e.g. deleted dataservice)
- 423: suspicious activity / spam — creation of new content disabled (body may include `"message": "Due to unusual activities..."`)
- 500: server error
- 502: server unreachable

Error body when present: `{"message": "...", "errors": {...}}`. Include `X-Sentry-ID` response header in support requests when present.

---

## Rate limiting

The API has rate limits. Check response headers:
- `X-RateLimit-Limit` — Request limit per hour
- `X-RateLimit-Remaining` — Remaining requests
- `X-RateLimit-Reset` — Reset time (Unix timestamp)

---

## Additional resources

- **Swagger (source of truth):** https://www.data.gouv.fr/api/1/swagger.json
- **Official docs (guides):** https://guides.data.gouv.fr/guide-data.gouv.fr/readme-1/reference/ — pages by resource (site, datasets, dataservices, reuses, discussions, organizations, spatial, users, me, workers, tags, topics, posts, transfer, notifications, avatars, harvest, contacts)
- **Python Client:** https://github.com/etalab/datagouv-client-python
