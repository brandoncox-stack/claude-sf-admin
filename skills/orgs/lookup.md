# Lookup Org Relationships

## Steps
1. Read `data/org-registry.json`
2. Find the org by alias from arguments. If not found, list registered orgs and ask user to clarify.

### If the org is a sandbox or scratch:
Report its parent:
> **{{alias}}** (sandbox) → linked to **{{parentAlias}}**
> Production username: {{parentUsername}}
> Production orgId: {{parentOrgId}}

If `parentAlias` is null:
> **{{alias}}** has no linked production org. Use `/orgs link {{alias}}` to set one.

### If the org is production or devhub:
List all children (orgs where `parentAlias` matches this alias):
> **{{alias}}** has these linked orgs:
>   - {{childAlias1}} ({{childType}})
>   - {{childAlias2}} ({{childType}})

If no children:
> **{{alias}}** has no linked sandboxes.
