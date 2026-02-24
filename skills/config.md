# Workflow Rules & Assignment Rules

> For validation rules and formula fields, see `formulas-validations.md`

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

## Retrieve Assignment Rules
```bash
sf project retrieve start --metadata AssignmentRule:{{objectApiName}} --target-org {{org}}
```

## List Escalation Rules
```bash
sf data query --query "SELECT Id, Name, SobjectType FROM EscalationRule ORDER BY Name" --target-org {{org}} --use-tooling-api
```

## List Auto-Response Rules
```bash
sf data query --query "SELECT Id, Name, SobjectType FROM AutoResponseRule ORDER BY Name" --target-org {{org}} --use-tooling-api
```
