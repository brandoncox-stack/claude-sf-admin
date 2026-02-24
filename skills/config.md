# Validation Rules, Workflow Rules & Assignment Rules

## List Validation Rules on an Object
```bash
sf data query --query "SELECT Id, ValidationName, Active, Description, ErrorMessage FROM ValidationRule WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}'" --target-org {{org}} --use-tooling-api
```

## Activate/Deactivate Validation Rule
Retrieve, edit the metadata XML, then deploy:
```bash
sf project retrieve start --metadata ValidationRule:{{objectApiName}}.{{ruleName}} --target-org {{org}}
```
Edit the `<active>` field in the retrieved XML, then:
```bash
sf project deploy start --metadata ValidationRule:{{objectApiName}}.{{ruleName}} --target-org {{org}}
```

## List Workflow Rules
```bash
sf data query --query "SELECT Id, Name, TableEnumOrId FROM WorkflowRule ORDER BY Name" --target-org {{org}} --use-tooling-api
```

## Retrieve Workflow Rules
```bash
sf project retrieve start --metadata Workflow:{{objectApiName}} --target-org {{org}}
```

## List Assignment Rules
```bash
sf data query --query "SELECT Id, Name, SobjectType FROM AssignmentRule ORDER BY Name" --target-org {{org}} --use-tooling-api
```
