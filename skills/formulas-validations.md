# Formulas, Roll-Up Summaries & Validation Rules

## List Formula Fields on an Object
```bash
sf data query --query "SELECT QualifiedApiName, Label, DataType FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND DataType = 'Formula' ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## View Formula Field Definition
```bash
sf data query --query "SELECT DeveloperName, Metadata FROM CustomField WHERE TableEnumOrId = '{{objectApiName}}' AND DeveloperName = '{{fieldName}}'" --target-org {{org}} --use-tooling-api
```

## Retrieve Formula Field Metadata
```bash
sf project retrieve start --metadata CustomField:{{objectApiName}}.{{fieldApiName}} --target-org {{org}}
```

## List Roll-Up Summary Fields on an Object
```bash
sf data query --query "SELECT QualifiedApiName, Label, DataType FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND DataType LIKE '%Summary%' ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## List Validation Rules on an Object
```bash
sf data query --query "SELECT Id, ValidationName, Active, Description, ErrorMessage FROM ValidationRule WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}'" --target-org {{org}} --use-tooling-api
```

## View Validation Rule Details
```bash
sf data query --query "SELECT Id, ValidationName, Active, Description, ErrorConditionFormula, ErrorMessage FROM ValidationRule WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND ValidationName = '{{ruleName}}'" --target-org {{org}} --use-tooling-api
```

## Activate a Validation Rule
Retrieve, set `<active>true</active>`, deploy:
```bash
sf project retrieve start --metadata ValidationRule:{{objectApiName}}.{{ruleName}} --target-org {{org}}
# Edit the XML to set <active>true</active>
sf project deploy start --metadata ValidationRule:{{objectApiName}}.{{ruleName}} --target-org {{org}}
```

## Deactivate a Validation Rule
Same flow, set `<active>false</active>`:
```bash
sf project retrieve start --metadata ValidationRule:{{objectApiName}}.{{ruleName}} --target-org {{org}}
# Edit the XML to set <active>false</active>
sf project deploy start --metadata ValidationRule:{{objectApiName}}.{{ruleName}} --target-org {{org}}
```

## Deploy Formula/Validation Changes
```bash
sf project deploy start --source-dir force-app/main/default/objects/{{objectApiName}} --target-org {{org}}
```

## Common Formula Functions
- TEXT() - convert picklist to text
- ISPICKVAL() - check picklist value
- VLOOKUP() - cross-object lookup
- IF(condition, true_value, false_value)
- ISBLANK() / ISNULL()
- TODAY(), NOW(), YEAR(), MONTH(), DAY()
- CONTAINS(), LEFT(), RIGHT(), MID(), LEN()
- ROUND(), CEILING(), FLOOR()
- HYPERLINK(), IMAGE()
- Cross-object: RelationshipName.FieldName (up to 10 levels)

## Common Validation Rule Patterns
- Required field: `ISBLANK(FieldName__c)`
- Date in future: `DateField__c < TODAY()`
- Email format: `NOT(REGEX(Email__c, "^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$"))`
- Prevent edit after stage: `ISCHANGED(StageName) && PRIORVALUE(StageName) = "Closed Won"`

## Notes
- Roll-up summaries only work on Master-Detail relationships (child → parent)
- Roll-up types: COUNT, SUM, MIN, MAX
- Formula fields are read-only and calculated on the fly
- Cross-object formulas can span up to 10 relationship levels
