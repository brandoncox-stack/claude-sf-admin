# Analytics Assessment

## 1. Reports
```bash
sf data query --query "SELECT Id, Name, DeveloperName, FolderName, LastRunDate, CreatedDate FROM Report WHERE FolderName != null ORDER BY LastRunDate NULLS FIRST LIMIT 200" --target-org {{org}}
```

### Flag: Never Run Report (WARNING)
Reports where `LastRunDate` is null.

### Flag: Stale Report (INFO)
Reports not run in the last 180 days.

## 2. Dashboards
```bash
sf data query --query "SELECT Id, Title, DeveloperName, FolderName, LastReferencedDate, CreatedDate FROM Dashboard ORDER BY LastReferencedDate NULLS FIRST LIMIT 200" --target-org {{org}}
```

### Flag: Stale Dashboard (INFO)
Dashboards not referenced in the last 180 days.

## Output Format
```
Analytics Assessment — {{org}}
Reports: X total | Never run: X | Stale (180d): X
Dashboards: X total | Stale (180d): X

| Item | Type | Folder | Last Used | Issue | Severity |
|------|------|--------|-----------|-------|----------|
| ...  | ...  | ...    | ...       | ...   | ...      |
```
