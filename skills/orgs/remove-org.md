# Remove Org from Registry

## Steps
1. Read `data/org-registry.json`
2. Find the org by alias from arguments. If not found, list all registered orgs and ask user to clarify.
3. **Check for children:** Find any orgs where `parentAlias` matches this alias. If any exist, warn:
   > These sandboxes are linked to this org: {{list}}. Remove anyway?
   If user confirms, set those children's `parentAlias` to `null`.
4. Confirm with user: "Remove **{{alias}}** from the registry?"
5. Remove the entry from `orgs` array.
6. Write updated JSON to `data/org-registry.json`.
7. Confirm: "Removed **{{alias}}** from registry."

## Note
This only removes from the registry. It does NOT delete the org from Salesforce. Tell the user:
> The org itself is unchanged in Salesforce. To delete, use `/admin delete scratch org` or manage sandboxes in production Setup.
