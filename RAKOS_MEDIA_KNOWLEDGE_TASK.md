# Rakos Media WordPress Sync Task

## Live WordPress State

- WordPress is being accessed over SSH and WP-CLI.
- Live site URL: `https://rakosmediagroup.com/`
- Confirmed Knowledge CPT key: `knowledge-page`
- Confirmed Knowledge taxonomies:
  - `knowledge-type`
  - `knowledge-topic`
  - `knowledge-industry`
  - `knowledge-framework`

## Current Sync Requirements

- Rebuild from the GitHub knowledge repo as the source of truth.
- Do not trust older Knowledge posts, taxonomy assignments, related fields, or older schema handling.
- Store only human-readable article HTML in `post_content`.
- Generate schema during sync, store it in meta, and output it in `wp_head` on single Knowledge posts only.

## Confirmed Schema Architecture

- Do not append JSON-LD into `post_content`.
- Save generated schema JSON to:
  - `rakos_generated_schema_json`
- Treat `schema_json_override` as an optional manual override when it contains valid JSON.
- Output schema only on:
  - `is_singular('knowledge-page')`
- Output:
  - `WebPage`
  - `BreadcrumbList`
  - `Organization`
  - primary schema from page type
  - `FAQPage` only when valid FAQ content exists

## Related Link Architecture

- Related Knowledge blocks must not invent dead `/knowledge/.../` URLs.
- Only link related items when a safe live target exists.
- Matching order:
  1. live `knowledge-page` by slug
  2. live `knowledge-page` by exact title
  3. live `knowledge-page` by `repo_slug`
  4. selected relationship posts when available
  5. approved non-Knowledge live pages for service targets
- If no safe target exists yet, render plain text or skip the link.

## Confirmed Live ACF Fields

- `repo_file_path`
- `repo_slug`
- `repo_last_updated`
- `github_source_url`
- `knowledge_title`
- `primary_entity`
- `schema_type`
- `word_count_target`
- `related_services`
- `related_concepts`
- `source_urls`
- `suggested_schema_type`
- `supporting_schema_types`
- `schema_json_override`
- `internal_link_suggestions`
- `related_knowledge_pages`
- `soft_cta_text`
- `soft_cta_button_text`
- `soft_cta_button_url`

## Important ACF Clarifications

- `related_frameworks` is not a live ACF field.
- `related_industries` is not a live ACF field.
- Framework and industry data still belong on the post through live taxonomies:
  - `knowledge-framework`
  - `knowledge-industry`

## Execution Order

Current preferred publishing order:

1. entities
2. services
3. frameworks
4. concepts
5. industries, playbooks, templates

Reason:

- concepts depend on entities, services, and frameworks for clean live internal linking
- syncing concepts first leaves Related Knowledge mostly unlinked even when the repo data is correct

## Current First Verified Page

- File: `concepts/structured-data.md`
- Live URL: `https://rakosmediagroup.com/knowledge/structured-data/`
- Verified:
  - no YAML visible
  - no repo-only sections visible
  - rendered public source contains the expected schema markers
  - schema is not stored in `post_content`
  - rendered source contains one schema block after repeat sync

## Next Task

- validate the `entities`, `services`, and `frameworks` batches
- sync validated files in that order
- then rerun concept pages once their live dependencies exist
