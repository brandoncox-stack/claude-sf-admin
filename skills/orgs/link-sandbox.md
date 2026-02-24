# Link Sandbox to Production Org

## Steps
1. Read `data/org-registry.json`
2. Get the sandbox alias from arguments. If not provided, list all sandbox/scratch orgs and ask which one.
3. If alias not in registry: "**{{alias}}** isn't registered. Run `/orgs add {{alias}}` first."
4. If the org's `type` is `production` or `devhub`: "**{{alias}}** is a production org, not a sandbox. Nothing to link."
5. Show all orgs where `type` is `production` or `devhub`:
   > Link **{{sandboxAlias}}** to which production org?
   > 1. trailhead-playground (brandon.cox.bba5f441ff43@agentforce.com)
   > 2. other-prod (user2@example.com)
6. If no production orgs exist: "No production orgs registered. Run `/orgs add <prod-alias>` first."
7. User picks one. Set `parentAlias` to the chosen alias.
8. Write updated JSON to `data/org-registry.json`.
9. Confirm: "Linked **{{sandboxAlias}}** → **{{prodAlias}}**"
