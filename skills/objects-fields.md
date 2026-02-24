# Custom Objects & Fields

## List Custom Objects
```bash
sf data query --query "SELECT DurableId, QualifiedApiName, Label FROM EntityDefinition WHERE QualifiedApiName LIKE '%__c' ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## Describe an Object (fields, record types)
```bash
sf sobject describe --sobject {{objectApiName}} --target-org {{org}}
```

## List Fields on an Object
```bash
sf data query --query "SELECT QualifiedApiName, Label, DataType, IsRequired FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## Get Picklist Values
```bash
sf data query --query "SELECT Value, IsActive, Label FROM PicklistValueInfo WHERE EntityParticle.EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND EntityParticle.QualifiedApiName = '{{fieldApiName}}' AND IsActive = true" --target-org {{org}} --use-tooling-api
```

## Retrieve Object/Field Metadata
```bash
sf project retrieve start --metadata CustomObject:{{objectApiName}} --target-org {{org}}
```

## Deploy Object/Field Changes
```bash
sf project deploy start --source-dir force-app/main/default/objects/{{objectApiName}} --target-org {{org}}
```
