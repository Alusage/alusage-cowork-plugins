# GitLab API — Opérations courantes

Base URL : `https://gitlab.sudokeys.com/api/v4`
Auth header : `-H "PRIVATE-TOKEN: $GITLAB_TOKEN"`

## Projets

```bash
# Lister les projets du namespace sudokeys
curl -s "https://gitlab.sudokeys.com/api/v4/groups/sudokeys/projects?per_page=50" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" | python3 -m json.tool

# Trouver l'ID d'un projet par nom
curl -s "https://gitlab.sudokeys.com/api/v4/projects?search={client}-addons" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  | python3 -c "import json,sys; [print(p['id'], p['path_with_namespace']) for p in json.load(sys.stdin)]"
```

## Branches

```bash
# Lister les branches d'un projet
curl -s "https://gitlab.sudokeys.com/api/v4/projects/{id}/repository/branches" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN"

# Créer une branche
curl -s -X POST "https://gitlab.sudokeys.com/api/v4/projects/{id}/repository/branches" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"branch": "TI{REF}", "ref": "master"}'
```

## Merge Requests

```bash
# Créer une MR
curl -s -X POST "https://gitlab.sudokeys.com/api/v4/projects/{id}/merge_requests" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "source_branch": "TI{REF}",
    "target_branch": "master",
    "title": "TI{REF}: {titre}",
    "description": "{description}",
    "remove_source_branch": true,
    "squash": false
  }'

# Lister les MR ouvertes
curl -s "https://gitlab.sudokeys.com/api/v4/projects/{id}/merge_requests?state=opened" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN"
```

## Issues / Tickets GitLab

```bash
# Créer une issue liée à un ticket Odoo
curl -s -X POST "https://gitlab.sudokeys.com/api/v4/projects/{id}/issues" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "TI{REF}: {titre du ticket Odoo}",
    "description": "Ticket Odoo #REF\n\n{description}",
    "labels": "support"
  }'
```

## Notes (commentaires)

```bash
# Commenter une MR
curl -s -X POST "https://gitlab.sudokeys.com/api/v4/projects/{id}/merge_requests/{mr_iid}/notes" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"body": "{commentaire}"}'
```
