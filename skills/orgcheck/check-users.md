# User Assessment

## 1. Active Internal Users
```bash
sf data query --query "SELECT Id, Name, Username, Profile.Name, UserRole.Name, LastLoginDate, IsActive, UserPreferencesLightningExperiencePreferred FROM User WHERE IsActive = true AND UserType = 'Standard' ORDER BY LastLoginDate NULLS FIRST" --target-org {{org}}
```

### Flag: Never logged in (CRITICAL)
Users where `LastLoginDate` is null.

### Flag: Not on Lightning (WARNING)
Users where `UserPreferencesLightningExperiencePreferred` is false.

## 2. Admin Privilege Check
```bash
sf data query --query "SELECT Assignee.Name, Assignee.Username, PermissionSet.Name, PermissionsModifyAllData, PermissionsViewAllData, PermissionsManageUsers, PermissionsCustomizeApplication FROM PermissionSetAssignment WHERE Assignee.IsActive = true AND (PermissionsModifyAllData = true OR PermissionsViewAllData = true OR PermissionsManageUsers = true OR PermissionsCustomizeApplication = true)" --target-org {{org}}
```

### Flag: Modify All Data (CRITICAL)
Any non-admin user with ModifyAllData.

### Flag: View All Data (WARNING)
Any non-admin user with ViewAllData.

## 3. MFA Check
```bash
sf data query --query "SELECT Assignee.Name, Assignee.Username, PermissionSet.Name, PermissionsByppassMfaForUiLogins FROM PermissionSetAssignment WHERE PermissionsByppassMfaForUiLogins = true AND Assignee.IsActive = true" --target-org {{org}}
```

### Flag: MFA Bypass (CRITICAL)
Any user with MFA bypass enabled.

## 4. Failed Logins
```bash
sf data query --query "SELECT UserId, COUNT(Id) loginCount FROM LoginHistory WHERE Status != 'Success' AND LoginTime = LAST_N_DAYS:30 GROUP BY UserId HAVING COUNT(Id) > 5" --target-org {{org}}
```

### Flag: Excessive Failed Logins (WARNING)
More than 5 failed logins in 30 days.

## 5. Debug Mode
```bash
sf data query --query "SELECT Id, Name, Username FROM User WHERE IsActive = true AND UserPreferencesUserDebugModePref = true" --target-org {{org}}
```

### Flag: Debug Mode Enabled (INFO)
Active users with debug mode on (performance impact).

## Output Format
```
Users Assessment — {{org}}
Active users: X | Never logged in: X | Not on Lightning: X

| User | Issue | Severity |
|------|-------|----------|
| ...  | ...   | ...      |
```
