# Data Modeling (Objects, Relationships, Schema Builder)

## List All Custom Objects
```bash
sf data query --query "SELECT QualifiedApiName, Label, PluralLabel, KeyPrefix FROM EntityDefinition WHERE QualifiedApiName LIKE '%__c' ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## Describe Object (all fields, relationships, record types)
```bash
sf sobject describe --sobject {{objectApiName}} --target-org {{org}}
```

## List Fields on an Object
```bash
sf data query --query "SELECT QualifiedApiName, Label, DataType, IsRequired, ReferenceTo, RelationshipName FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## List Lookup/Master-Detail Relationships on an Object
```bash
sf data query --query "SELECT QualifiedApiName, Label, DataType, ReferenceTo, RelationshipName FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND DataType LIKE '%Lookup%' OR (EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND DataType LIKE '%MasterDetail%')" --target-org {{org}} --use-tooling-api
```

## List Record Types on an Object
```bash
sf data query --query "SELECT Id, Name, DeveloperName, IsActive, Description FROM RecordType WHERE SobjectType = '{{objectApiName}}' ORDER BY Name" --target-org {{org}}
```

## Retrieve Custom Object Metadata (full definition including fields)
```bash
sf project retrieve start --metadata CustomObject:{{objectApiName}} --target-org {{org}}
```

## Retrieve a Specific Custom Field
```bash
sf project retrieve start --metadata CustomField:{{objectApiName}}.{{fieldApiName}} --target-org {{org}}
```

## Deploy Custom Object Changes
```bash
sf project deploy start --source-dir force-app/main/default/objects/{{objectApiName}} --target-org {{org}}
```

## List All Standard Objects
```bash
sf data query --query "SELECT QualifiedApiName, Label, KeyPrefix FROM EntityDefinition WHERE NOT QualifiedApiName LIKE '%__c' AND IsCustomizable = true ORDER BY QualifiedApiName" --target-org {{org}} --use-tooling-api
```

## View Schema (relationships between objects)
Use Schema Builder in the UI:
```bash
sf org open --target-org {{org}} --path /lightning/setup/SchemaBuilder/home
```

## Create a Custom Object (via metadata)
1. Create the object XML in `force-app/main/default/objects/{{ObjectName}}__c/{{ObjectName}}__c.object-meta.xml`
2. Deploy:
```bash
sf project deploy start --source-dir force-app/main/default/objects/{{ObjectName}}__c --target-org {{org}}
```

## Notes
- Standard relationships: Account->Contact, Account->Opportunity, Opportunity->OpportunityLineItem
- Lookup = optional relationship, Master-Detail = required with cascade delete
- Junction objects use two Master-Detail relationships for many-to-many
