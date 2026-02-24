# List All Registered Orgs

## Steps
1. Read `data/org-registry.json`
2. If registry is empty, say: "No orgs registered. Use `/orgs add <alias>` to register one."
3. Group orgs by relationship and display as a tree:

## Output Format
```
Production Orgs:
  trailhead-playground (brandon.cox.bba5f441ff43@agentforce.com)
    └── uat-sandbox (sandbox)
    └── dev-sandbox (sandbox)

  other-prod (user2@example.com)
    (no linked sandboxes)

DevHub Orgs:
  my-devhub (devhub@example.com)
    └── scratch-1 (scratch)

Unlinked Orgs:
  mystery-sandbox (sandbox, no parent set)
```

4. At the end, show a count: "**{{total}}** orgs registered ({{prod}} production, {{sandbox}} sandbox, {{scratch}} scratch)"
