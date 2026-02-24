# Flow Management

## List Active Flows
```bash
sf data query --query "SELECT Id, DeveloperName, ActiveVersionId, LatestVersionId, ProcessType FROM FlowDefinition WHERE ActiveVersionId != null ORDER BY DeveloperName" --target-org {{org}} --use-tooling-api
```

## List All Flows (including inactive)
```bash
sf data query --query "SELECT Id, DeveloperName, ActiveVersionId, LatestVersionId, ProcessType FROM FlowDefinition ORDER BY DeveloperName" --target-org {{org}} --use-tooling-api
```

## Get Flow Version Details
```bash
sf data query --query "SELECT Id, FlowDefinitionViewId, VersionNumber, Status, ProcessType FROM Flow WHERE DefinitionId = '{{flowDefinitionId}}'" --target-org {{org}} --use-tooling-api
```

## Activate a Flow Version
```bash
sf data update record --sobject FlowDefinition --record-id {{flowDefinitionId}} --values "ActiveVersionId='{{versionId}}'" --target-org {{org}} --use-tooling-api
```

## Deactivate a Flow
Confirm with user first.
```bash
sf data update record --sobject FlowDefinition --record-id {{flowDefinitionId}} --values "ActiveVersionId=''" --target-org {{org}} --use-tooling-api
```

## Retrieve Flow Metadata
```bash
sf project retrieve start --metadata Flow:{{flowApiName}} --target-org {{org}}
```

## Deploy Flow
```bash
sf project deploy start --metadata Flow:{{flowApiName}} --target-org {{org}}
```
