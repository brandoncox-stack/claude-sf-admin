# Add Org to Registry

## Steps
1. Read `data/org-registry.json`
2. Get the org alias from arguments. If not provided, ask the user.
3. **Duplicate check:** If alias already exists in registry, tell the user and stop.
4. Verify the org exists in SF CLI:
```bash
sf org display --target-org {{alias}} --json
```
5. Extract `username`, `id` (orgId), and `instanceUrl` from the result.
6. Ask user: What type of org is this?
   - production
   - sandbox
   - scratch
   - devhub
7. **If sandbox or scratch:** Filter registry for orgs where `type` is `production` or `devhub`. Show them:
   > Link this sandbox to which production org?
   > 1. {{prodAlias1}} ({{username1}})
   > 2. {{prodAlias2}} ({{username2}})
   > 3. Skip (link later)

   Set `parentAlias` based on choice, or `null` if skipped.
   If no production orgs exist in registry, set `parentAlias` to `null` and tell user:
   > No production orgs registered yet. Run `/orgs add <prod-alias>` first, then `/orgs link` to connect.
8. Add entry to `orgs` array:
```json
{
  "alias": "{{alias}}",
  "username": "{{username}}",
  "orgId": "{{orgId}}",
  "type": "{{type}}",
  "parentAlias": {{parentAlias or null}},
  "addedAt": "{{YYYY-MM-DD}}"
}
```
9. Write updated JSON to `data/org-registry.json`.
10. Confirm: "Registered **{{alias}}** ({{type}})" and if linked: "→ linked to **{{parentAlias}}**"
