# UI Components Assessment

## 1. Visualforce Pages
```bash
sf data query --query "SELECT Id, Name, ApiVersion, NamespacePrefix, Description, CreatedDate FROM ApexPage WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY Name" --target-org {{org}} --use-tooling-api
```

### Flag: Outdated API Version (WARNING)
Pages with ApiVersion more than 3 years old (< v58.0).

### Flag: Missing Description (INFO)
Pages with no Description.

## 2. Visualforce Components
```bash
sf data query --query "SELECT Id, Name, ApiVersion, NamespacePrefix, Description FROM ApexComponent WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY Name" --target-org {{org}} --use-tooling-api
```

### Flag: Outdated API Version (WARNING)
Components with ApiVersion more than 3 years old.

## 3. Aura Components
```bash
sf data query --query "SELECT Id, DeveloperName, ApiVersion, NamespacePrefix, Description FROM AuraDefinitionBundle WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY DeveloperName" --target-org {{org}} --use-tooling-api
```

### Flag: Outdated API Version (WARNING)
Aura components with ApiVersion more than 3 years old.

### Flag: Missing Description (INFO)
Aura components with no Description.

## 4. Lightning Web Components
```bash
sf data query --query "SELECT Id, DeveloperName, ApiVersion, NamespacePrefix, Description FROM LightningComponentBundle WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY DeveloperName" --target-org {{org}} --use-tooling-api
```

### Flag: Outdated API Version (WARNING)
LWCs with ApiVersion more than 3 years old.

### Flag: Missing Description (INFO)
LWCs with no Description.

## 5. Lightning Pages (FlexiPages)
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, Type, EntityDefinitionId, Description FROM FlexiPage WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY MasterLabel" --target-org {{org}} --use-tooling-api
```

### Flag: Missing Description (INFO)
FlexiPages with no Description.

## 6. Custom Tabs
```bash
sf data query --query "SELECT Id, Name, Label, Type, SobjectName FROM CustomTab WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY Label" --target-org {{org}} --use-tooling-api
```

## Output Format
```
UI Components Assessment — {{org}}
VF Pages: X | VF Components: X | Aura: X | LWC: X | FlexiPages: X | Tabs: X

| Component | Type | API Ver | Issues | Severity |
|-----------|------|---------|--------|----------|
| ...       | ...  | ...     | ...    | ...      |
```
