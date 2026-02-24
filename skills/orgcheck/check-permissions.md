# Permissions Assessment

## 1. Custom Profiles with No Users
```bash
sf data query --query "SELECT Id, Name, (SELECT Id FROM Users WHERE IsActive = true LIMIT 1) FROM Profile WHERE UserType = 'Standard'" --target-org {{org}}
```

### Flag: Empty Custom Profile (WARNING)
Custom profiles with zero active users assigned. Exclude standard profiles (System Administrator, Standard User, etc.).

### Flag: Low Adoption Profile (INFO)
Custom profiles with 10 or fewer active users.

## 2. Permission Sets with No Assignments
```bash
sf data query --query "SELECT Id, Name, Label, IsCustom, (SELECT Id FROM Assignments WHERE Assignee.IsActive = true LIMIT 1) FROM PermissionSet WHERE IsOwnedByProfile = false AND IsCustom = true" --target-org {{org}}
```

### Flag: Empty Permission Set (WARNING)
Custom permission sets with zero active assignments.

## 3. Permission Set Groups
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, Status, (SELECT Id FROM Assignments WHERE Assignee.IsActive = true LIMIT 1) FROM PermissionSetGroup" --target-org {{org}}
```

### Flag: Empty Permission Set Group (WARNING)
Groups with zero active assignments.

## 4. Permission Set License Usage
```bash
sf data query --query "SELECT Id, MasterLabel, TotalLicenses, UsedLicenses FROM PermissionSetLicense WHERE TotalLicenses > 0 ORDER BY MasterLabel" --target-org {{org}}
```

### Flag: License Near Limit (CRITICAL)
UsedLicenses / TotalLicenses >= 80%.

### Flag: Unused Licenses (INFO)
Licenses where UsedLicenses is significantly below TotalLicenses.

## Output Format
```
Permissions Assessment — {{org}}
Profiles: X total | Empty: X | Permission Sets: X total | Empty: X

| Item | Type | Issue | Severity |
|------|------|-------|----------|
| ...  | ...  | ...   | ...      |
```
