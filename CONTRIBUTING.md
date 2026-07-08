# Contributing to LOLCreds Data

Thanks for helping improve LOLCreds Data. This repository is for credential-location intelligence templates and supporting documentation only.

## Before you submit

1. Confirm the entry is product-, platform-, SaaS-, or appliance-specific.
2. Gather authoritative sources first: vendor docs, official hardening guides, CVEs, vendor advisories, or well-regarded security advisories.
3. Do not include private secrets, exploit code, scanners, credential collection scripts, or live-system data.
4. Keep one product/platform per YAML file.
5. Name files with lowercase kebab-case: `vendor-product.yaml` or the commonly recognized product slug.
6. Ensure the top-level `id` equals the filename without `.yaml`.

## Pull request types

Accepted changes include:

- new YAML entries under `entries/`;
- corrections to existing YAML entries;
- source/reference improvements;
- documentation improvements;
- template/schema wording updates.

Not accepted here:

- scripts or automation;
- scanners, exploit code, or credential dumping utilities;
- bulk unsourced AI-generated entries;
- real credentials or leaked datasets.

## Authoring rules

### Required top-level fields

Each entry must include:

- `id`
- `name`
- `category`
- `vendor`
- `author`
- `credentials`
- `references`

### Required credential fields

Each credential block must include:

- `id`
- `name`
- `description`
- `type`
- `nature`
- `sensitivity`
- `location`

Use `looks_like`, `default`, and `notes` only when they add sourced or detection-relevant value.

### Defaults

Only add a `default` block when a vendor document, CVE, or advisory states the literal default value. Do not treat documentation examples as product defaults.

### User-defined credentials

User/admin password flows belong in LOLCreds when they are product-relevant. Even if the value is user-defined, the entry should document where the credential lives, how it is used, and what artifacts may contain it.

### Backdoors and hardcoded accounts

Include public backdoor or hardcoded-account cases only when backed by CVE/advisory sources. If a source confirms a hardcoded password but does not publish the literal value, document the account/context without a `default` block.

### `location` and `looks_like`

- Reserve `looks_like` for credential values or context-specific patterns.
- If a regex has no example, include `detail` explaining the context.

Good:

```yaml
location:
  - type: environment
    path: EXAMPLE_API_TOKEN, EXAMPLE_CLIENT_SECRET
  - type: http_header
    path: Authorization
    detail: Bearer token used by the API
looks_like:
  - example: 'ex_XXXXXXXXXXXXXXXXXXXXXXXX'
    pattern: 'ex_[A-Za-z0-9]{20,}'
```

Avoid:

```yaml
looks_like:
  - example: 'EXAMPLE_API_TOKEN=XXXXXXXXXXXXXXXX'
  - example: 'Authorization: Bearer XXXXXXXXXXXXXXXX'
```

## Manual review checklist

Before opening a PR, check:

- [ ] YAML parses.
- [ ] Filename stem matches top-level `id`.
- [ ] Each credential has required fields.
- [ ] `location.type` values are in the allowed taxonomy.
- [ ] Regex patterns are valid and safely quoted.
- [ ] No real secrets, live tokens, exploit code, or collection scripts are included.
- [ ] At least one authoritative reference is present.
- [ ] Defaults are explicitly documented by a source.
- [ ] Product-family defaults are not generalized from a specific model or example.

## Style

- Prefer concrete, scannable locations: file paths, env vars, headers, database tables, endpoints.
- Keep descriptions factual and source-backed.
- Use `notes` for caveats, blast-radius notes, and distinctions between defaults, examples, and generated values.
- Use single-quoted YAML scalars for regexes with backslashes.
