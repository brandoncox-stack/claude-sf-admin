# Data Model Assessment

## 1. Object Complexity
```bash
sf data query --query "SELECT QualifiedApiName, Label, (SELECT COUNT() FROM Fields) nbFields FROM EntityDefinition WHERE QualifiedApiName LIKE '%__c' AND IsCustomizable = true ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

If the above subquery fails, run separately:
```bash
sf data query --query "SELECT QualifiedApiName, Label FROM EntityDefinition WHERE QualifiedApiName LIKE '%__c' AND IsCustomizable = true ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

Then for each object:
```bash
sf data query --query "SELECT COUNT() FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}'" --target-org {{org}} --use-tooling-api
```

### Flag: Excessive Custom Fields (CRITICAL)
Objects with > 350 custom fields (approaching the 500 hard limit).

### Flag: High Custom Fields (WARNING)
Objects with > 200 custom fields.

## 2. Validation Rules Count per Object
```bash
sf data query --query "SELECT EntityDefinition.QualifiedApiName, COUNT(Id) ruleCount FROM ValidationRule WHERE ManageableState = 'unmanaged' GROUP BY EntityDefinition.QualifiedApiName HAVING COUNT(Id) > 10 ORDER BY COUNT(Id) DESC" --target-org {{org}} --use-tooling-api
```

### Flag: Excessive Validation Rules (WARNING)
Objects with > 20 validation rules.

### Flag: High Validation Rules (INFO)
Objects with > 10 validation rules.

## 3. Page Layouts Count per Object
```bash
sf data query --query "SELECT EntityDefinition.QualifiedApiName, COUNT(Id) layoutCount FROM Layout WHERE ManageableState = 'unmanaged' GROUP BY EntityDefinition.QualifiedApiName HAVING COUNT(Id) > 5 ORDER BY COUNT(Id) DESC" --target-org {{org}} --use-tooling-api
```

### Flag: Excessive Page Layouts (WARNING)
Objects with > 15 page layouts.

## 4. Record Types per Object
```bash
sf data query --query "SELECT SobjectType, COUNT(Id) rtCount FROM RecordType WHERE IsActive = true GROUP BY SobjectType HAVING COUNT(Id) > 5 ORDER BY COUNT(Id) DESC" --target-org {{org}}
```

### Flag: Many Record Types (INFO)
Objects with > 10 active record types.

## 5. Unassigned Record Types
```bash
sf data query --query "SELECT Id, Name, SobjectType, IsActive, DeveloperName FROM RecordType WHERE IsActive = false" --target-org {{org}}
```

### Flag: Inactive Record Types (INFO)
Record types that are inactive (potential cleanup candidates).

## Output Format
```
Data Model Assessment — {{org}}
Custom objects: X | Objects with 200+ fields: X | Objects with 20+ validation rules: X

| Object | Fields | Layouts | Val Rules | Record Types | Issues |
|--------|--------|---------|-----------|--------------|--------|
| ...    | ...    | ...     | ...       | ...          | ...    |
```
