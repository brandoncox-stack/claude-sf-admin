# Automation Assessment

## 1. Flows
```bash
sf data query --query "SELECT Id, DeveloperName, ActiveVersionId, LatestVersionId, ProcessType FROM FlowDefinition ORDER BY DeveloperName" --target-org {{org}} --use-tooling-api
```

Get version details for active flows:
```bash
sf data query --query "SELECT Id, Definition.DeveloperName, VersionNumber, Status, ApiVersion, ProcessType, Description, TriggerType FROM Flow WHERE Status = 'Active' ORDER BY Definition.DeveloperName" --target-org {{org}} --use-tooling-api
```

### Flag: Process Builder Still Active (CRITICAL)
Flows where `ProcessType = 'Workflow'` (Process Builders should be migrated to Flows).

### Flag: No Active Version (WARNING)
FlowDefinitions where `ActiveVersionId` is null but `LatestVersionId` exists.

### Flag: Too Many Versions (WARNING)
Flows with > 7 versions (query version count per definition).

### Flag: Missing Description (INFO)
Active flows with no Description.

### Flag: Outdated API Version (WARNING)
Active flows with ApiVersion more than 3 years old (< v58.0).

### Flag: Runs Without Sharing (CRITICAL)
Flows where the running mode is System Mode Without Sharing.

## 2. Workflow Rules
```bash
sf data query --query "SELECT Id, Name, TableEnumOrId FROM WorkflowRule WHERE ManageableState = 'unmanaged' ORDER BY Name" --target-org {{org}} --use-tooling-api
```

### Flag: Active Workflow Rules (WARNING)
Workflow rules should be migrated to Flows.

## 3. Process Builders (via Flow API)
```bash
sf data query --query "SELECT Id, Definition.DeveloperName, VersionNumber, Status, ProcessType FROM Flow WHERE ProcessType = 'Workflow' AND Status = 'Active'" --target-org {{org}} --use-tooling-api
```

### Flag: Active Process Builders (CRITICAL)
All active Process Builders should be migrated to Flows.

## Output Format
```
Automation Assessment — {{org}}
Active Flows: X | Process Builders: X | Workflow Rules: X

| Automation | Type | Status | Issues | Severity |
|------------|------|--------|--------|----------|
| ...        | ...  | ...    | ...    | ...      |
```

## Recommendations
- Migrate all Process Builders to Flows
- Migrate Workflow Rules to Flows
- Consolidate flows with > 7 versions (delete old inactive versions)
- Add descriptions to all active flows
