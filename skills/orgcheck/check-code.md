# Code Quality Assessment

## 1. Apex Classes Overview
```bash
sf data query --query "SELECT Id, Name, Status, IsValid, ApiVersion, NamespacePrefix, LengthWithoutComments, CreatedDate FROM ApexClass WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY Name" --target-org {{org}} --use-tooling-api
```

### Flag: Outdated API Version (WARNING)
Classes with ApiVersion more than 3 years old (currently < v58.0).

### Flag: Needs Recompilation (CRITICAL)
Classes where `IsValid = false`.

### Flag: No Sharing Declared (WARNING)
Classes that don't declare `with sharing` or `without sharing`. Check by retrieving class source:
```bash
sf project retrieve start --metadata ApexClass:{{className}} --target-org {{org}}
```

## 2. Apex Triggers
```bash
sf data query --query "SELECT Id, Name, TableEnumOrId, ApiVersion, Status, LengthWithoutComments FROM ApexTrigger WHERE ManageableState IN ('installedEditable', 'unmanaged') ORDER BY Name" --target-org {{org}} --use-tooling-api
```

### Flag: Trigger with SOQL/DML (CRITICAL)
Triggers with logic directly in the trigger body (> 5000 chars suggests logic not delegated to handler).

### Flag: Outdated API Version (WARNING)
Triggers with ApiVersion more than 3 years old.

## 3. Test Coverage
```bash
sf apex run test --target-org {{org}} --code-coverage --wait 10 --result-format json
```

If test run is too slow, check existing coverage:
```bash
sf data query --query "SELECT ApexClassOrTrigger.Name, NumLinesCovered, NumLinesUncovered FROM ApexCodeCoverageAggregate WHERE NumLinesCovered > 0 OR NumLinesUncovered > 0 ORDER BY ApexClassOrTrigger.Name" --target-org {{org}} --use-tooling-api
```

### Flag: No Coverage (CRITICAL)
Classes/triggers with 0% coverage.

### Flag: Insufficient Coverage (WARNING)
Classes/triggers with < 75% coverage.

## 4. Test Classes with No Assertions
```bash
sf data query --query "SELECT Id, Name FROM ApexClass WHERE Name LIKE '%Test%' AND ManageableState = 'unmanaged'" --target-org {{org}} --use-tooling-api
```
For flagged test classes, check source for `System.assert` usage.

### Flag: Test Without Assertions (WARNING)
Test classes with zero `System.assert*` calls.

## Output Format
```
Code Quality Assessment — {{org}}
Apex Classes: X | Triggers: X | Invalid: X | Low Coverage: X

| Component | Type | API Ver | Coverage | Issues | Severity |
|-----------|------|---------|----------|--------|----------|
| ...       | ...  | ...     | ...      | ...    | ...      |
```
