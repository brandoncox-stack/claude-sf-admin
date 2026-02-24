# Org Limits Assessment

## 1. API Limits
```bash
sf limits api display --target-org {{org}}
```

### Flag: Limit at 80%+ (CRITICAL)
Any limit where used/max >= 80%.

### Flag: Limit at 60%+ (WARNING)
Any limit where used/max >= 60%.

## 2. Storage Usage
```bash
sf data query --query "SELECT Name, StorageUsedMB, StorageLimitMB FROM Organization" --target-org {{org}}
```

Or check via limits:
```bash
sf org display --target-org {{org}} --json
```

### Flag: Storage Near Limit (CRITICAL)
Data or file storage > 80% used.

## 3. Custom Object Count
```bash
sf data query --query "SELECT COUNT() FROM EntityDefinition WHERE QualifiedApiName LIKE '%__c' AND IsCustomizable = true" --target-org {{org}} --use-tooling-api
```

### Flag: High Object Count (INFO)
More than 400 custom objects (Enterprise limit is typically 2000, but complexity increases).

## 4. License Usage
```bash
sf data query --query "SELECT Name, TotalLicenses, UsedLicenses FROM UserLicense WHERE TotalLicenses > 0 ORDER BY Name" --target-org {{org}}
```

### Flag: License Near Limit (CRITICAL)
UsedLicenses / TotalLicenses >= 80%.

### Flag: Unused Licenses (INFO)
Significant gap between Used and Total (potential cost savings).

## Output Format
```
Org Limits Assessment — {{org}}

| Limit | Used | Max | % Used | Status |
|-------|------|-----|--------|--------|
| ...   | ...  | ... | ...    | ...    |

License Usage:
| License | Used | Total | % Used | Status |
|---------|------|-------|--------|--------|
| ...     | ...  | ...   | ...    | ...    |
```
