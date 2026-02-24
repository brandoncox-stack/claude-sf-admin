# Org Management

## List Connected Orgs
```bash
sf org list
```

## Org Details
```bash
sf org display --target-org {{org}}
```

## Open Org in Browser
```bash
sf org open --target-org {{org}}
```

## Open Specific Page in Org
```bash
sf org open --target-org {{org}} --path {{path}}
```
Examples: `--path /lightning/setup/SetupOneHome/home`, `--path /lightning/o/Account/list`

## Check Org Limits
```bash
sf limits api display --target-org {{org}}
```

## Set Default Org
```bash
sf config set target-org={{orgAlias}}
```

## Create Scratch Org
```bash
sf org create scratch --definition-file config/project-scratch-def.json --alias {{alias}} --duration-days {{days}} --target-dev-hub {{devHub}}
```

## Create Sandbox
```bash
sf org create sandbox --name {{sandboxName}} --license-type {{Developer|DeveloperPro|Partial|Full}} --target-org {{prodOrg}}
```

## Delete Scratch Org
Confirm with user first.
```bash
sf org delete scratch --target-org {{org}} --no-prompt
```

## List Auth URLs
```bash
sf org list auth
```
