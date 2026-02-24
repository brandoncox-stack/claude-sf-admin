# Cleanup & Technical Debt Assessment

## 1. Empty Public Groups
```bash
sf data query --query "SELECT Id, Name, DeveloperName, Type, (SELECT Id FROM GroupMembers LIMIT 1) FROM Group WHERE Type = 'Regular' ORDER BY Name" --target-org {{org}}
```

### Flag: Empty Public Group (WARNING)
Groups with zero members.

## 2. Empty Roles
```bash
sf data query --query "SELECT Id, Name, DeveloperName, (SELECT Id FROM Users WHERE IsActive = true LIMIT 1) FROM UserRole ORDER BY Name" --target-org {{org}}
```

### Flag: Role with No Active Users (WARNING)
Roles with zero active members.

### Flag: Deep Role Hierarchy (WARNING)
Roles nested >= 7 levels deep. Check by querying ParentRoleId chain.

## 3. Empty Queues
```bash
sf data query --query "SELECT Id, Name, DeveloperName, Type, (SELECT Id FROM GroupMembers LIMIT 1) FROM Group WHERE Type = 'Queue' ORDER BY Name" --target-org {{org}}
```

### Flag: Empty Queue (WARNING)
Queues with zero members.

## 4. Inactive Validation Rules
```bash
sf data query --query "SELECT Id, ValidationName, EntityDefinition.QualifiedApiName, Active FROM ValidationRule WHERE Active = false AND ManageableState = 'unmanaged'" --target-org {{org}} --use-tooling-api
```

### Flag: Inactive Validation Rule (INFO)
Validation rules that are inactive (delete or reactivate).

## 5. Inactive Flows with No Active Version
```bash
sf data query --query "SELECT Id, DeveloperName, ActiveVersionId, LatestVersionId FROM FlowDefinition WHERE ActiveVersionId = null" --target-org {{org}} --use-tooling-api
```

### Flag: Flow with No Active Version (INFO)
Flow definitions with no active version (cleanup candidates).

## 6. Unused Email Templates
```bash
sf data query --query "SELECT Id, Name, DeveloperName, FolderName, LastUsedDate, CreatedDate FROM EmailTemplate WHERE LastUsedDate = null AND ManageableState = 'unmanaged' ORDER BY Name" --target-org {{org}}
```

### Flag: Never Used Email Template (INFO)
Templates never used.

## 7. Unused Custom Labels
```bash
sf data query --query "SELECT Id, Name, Value, Category FROM CustomLabel WHERE ManageableState = 'unmanaged' ORDER BY Name" --target-org {{org}} --use-tooling-api
```

### Flag: Review Custom Labels (INFO)
List unmanaged custom labels for review (cannot determine usage via SOQL alone).

## 8. Installed Packages
```bash
sf package installed list --target-org {{org}}
```

### Flag: Review Installed Packages (INFO)
List all installed packages — verify they are still needed.

## Output Format
```
Cleanup Assessment — {{org}}
Empty groups: X | Empty roles: X | Empty queues: X | Inactive rules: X

| Item | Type | Issue | Severity |
|------|------|-------|----------|
| ...  | ...  | ...   | ...      |

Recommended cleanup actions:
1. ...
2. ...
```
