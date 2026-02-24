# Picklist Administration

## List Picklist Fields on an Object
```bash
sf data query --query "SELECT QualifiedApiName, Label, DataType FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND (DataType = 'Picklist' OR DataType = 'MultiselectPicklist') ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## Get Picklist Values
```bash
sf data query --query "SELECT Value, Label, IsActive, IsDefaultValue FROM PicklistValueInfo WHERE EntityParticle.EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND EntityParticle.QualifiedApiName = '{{fieldApiName}}' ORDER BY Value" --target-org {{org}} --use-tooling-api
```

## Get Active Picklist Values Only
```bash
sf data query --query "SELECT Value, Label FROM PicklistValueInfo WHERE EntityParticle.EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND EntityParticle.QualifiedApiName = '{{fieldApiName}}' AND IsActive = true ORDER BY Value" --target-org {{org}} --use-tooling-api
```

## List Global Value Sets
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel FROM GlobalValueSet ORDER BY MasterLabel" --target-org {{org}} --use-tooling-api
```

## Get Global Value Set Values
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, Metadata FROM GlobalValueSet WHERE DeveloperName = '{{globalValueSetName}}'" --target-org {{org}} --use-tooling-api
```

## Retrieve Picklist Field Metadata
```bash
sf project retrieve start --metadata CustomField:{{objectApiName}}.{{fieldApiName}} --target-org {{org}}
```

## Retrieve Global Value Set
```bash
sf project retrieve start --metadata GlobalValueSet:{{globalValueSetName}} --target-org {{org}}
```

## Deploy Picklist Changes
```bash
sf project deploy start --source-dir force-app/main/default/objects/{{objectApiName}}/fields/{{fieldApiName}}.field-meta.xml --target-org {{org}}
```

## Deploy Global Value Set Changes
```bash
sf project deploy start --source-dir force-app/main/default/globalValueSets/{{globalValueSetName}}.globalValueSet-meta.xml --target-org {{org}}
```

## Dependent Picklist Setup
Dependent picklists are configured in field metadata. Retrieve and edit the XML:
```bash
sf project retrieve start --metadata CustomField:{{objectApiName}}.{{dependentFieldApiName}} --target-org {{org}}
```
The XML will contain `<valueSettings>` with controlling field references.

## Picklist by Record Type
```bash
sf data query --query "SELECT Id, Name, DeveloperName FROM RecordType WHERE SobjectType = '{{objectApiName}}' AND IsActive = true" --target-org {{org}}
```
Then view picklist values per record type in Setup:
```bash
sf org open --target-org {{org}} --path /lightning/setup/ObjectManager/{{objectApiName}}/RecordTypes/view
```

## Notes
- Standard picklists: cannot delete values, only deactivate
- Custom picklists: can add, edit, deactivate, reorder values
- Global Value Sets: shared picklist values across multiple fields
- Dependent picklists: controlling field determines available values in dependent field
- Record Type picklist filtering: restricts which values appear per record type
