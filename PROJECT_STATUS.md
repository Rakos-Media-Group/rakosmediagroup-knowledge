# Project Status

## Latest Update

The sync logic now uses the live WordPress model and the corrected schema architecture.

## Confirmed Live Model

- CPT: `knowledge-page`
- Taxonomies:
  - `knowledge-type`
  - `knowledge-topic`
  - `knowledge-industry`
  - `knowledge-framework`

## Completed

- GitHub Markdown remains the source of truth.
- Public Knowledge pages publish at `/knowledge/[slug]/`.
- Repo-only sections are removed from visible public content.
- `post_content` is now limited to classic article HTML plus visible related content.
- Generated schema is stored in post meta instead of article content.
- A front-end hook outputs validated schema in `wp_head` for single `knowledge-page` posts only.
- Repeat sync no longer creates duplicate rendered schema blocks.
- `concepts/structured-data.md` has been synced and verified against rendered public HTML.

## Current Link Behavior

- Safe related linking is enabled.
- The sync currently links only to confirmed live targets.
- Contextual paragraph auto-linking is disabled for now.
- Visible internal linking is limited to the Related Knowledge block until more targets are synced live.
- Concepts should be synced after entities, services, and frameworks so their related sections can resolve to real live pages.

## Current Publishing Order

1. entities
2. services
3. frameworks
4. concepts
5. industries, playbooks, templates

## Current Next Step

Validate and sync the `entities`, `services`, and `frameworks` directories before broad concept publishing.
