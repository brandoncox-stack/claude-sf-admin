# Data Security (OWD, Profiles, Roles, Sharing, Field-Level Security)

## View Org-Wide Defaults (OWD)
```bash
sf org open --target-org {{org}} --path /lightning/setup/SecuritySharing/home
```
Or query:
```bash
sf data query --query "SELECT SobjectType, DefaultInternalAccess, DefaultExternalAccess FROM Organization" --target-org {{org}}
```

## List Profiles
```bash
sf data query --query "SELECT Id, Name, UserType, Description FROM Profile ORDER BY Name" --target-org {{org}}
```

## View Profile Object Permissions
```bash
sf data query --query "SELECT SobjectType, PermissionsRead, PermissionsCreate, PermissionsEdit, PermissionsDelete, PermissionsViewAllRecords, PermissionsModifyAllRecords FROM ObjectPermissions WHERE ParentId IN (SELECT Id FROM PermissionSet WHERE ProfileId IN (SELECT Id FROM Profile WHERE Name = '{{profileName}}'))" --target-org {{org}}
```

## View Field-Level Security for a Profile
```bash
sf data query --query "SELECT SobjectType, Field, PermissionsRead, PermissionsEdit FROM FieldPermissions WHERE ParentId IN (SELECT Id FROM PermissionSet WHERE ProfileId IN (SELECT Id FROM Profile WHERE Name = '{{profileName}}'))" --target-org {{org}}
```

## List Roles (Role Hierarchy)
```bash
sf data query --query "SELECT Id, Name, DeveloperName, ParentRoleId, ParentRole.Name FROM UserRole ORDER BY Name" --target-org {{org}}
```

## View Users in a Role
```bash
sf data query --query "SELECT Id, Name, Username, IsActive FROM User WHERE UserRoleId IN (SELECT Id FROM UserRole WHERE DeveloperName = '{{roleDeveloperName}}')" --target-org {{org}}
```

## Assign Role to User
```bash
sf data update record --sobject User --where "Username='{{username}}'" --values "UserRoleId='{{roleId}}'" --target-org {{org}}
```

## List Sharing Rules
```bash
sf org open --target-org {{org}} --path /lightning/setup/SecuritySharing/home
```

## Retrieve Sharing Rules Metadata
```bash
sf project retrieve start --metadata SharingRules:{{objectApiName}} --target-org {{org}}
```

## List Permission Sets
```bash
sf data query --query "SELECT Id, Name, Label, IsCustom, Description FROM PermissionSet WHERE IsOwnedByProfile = false ORDER BY Label" --target-org {{org}}
```

## List Permission Set Groups
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, Description, Status FROM PermissionSetGroup ORDER BY MasterLabel" --target-org {{org}}
```

## Assign Permission Set to User
```bash
sf org assign permset --name {{permSetApiName}} --target-org {{org}} --on-behalf-of {{username}}
```

## View Login History
```bash
sf data query --query "SELECT UserId, LoginTime, LoginType, SourceIp, Status, Application FROM LoginHistory ORDER BY LoginTime DESC LIMIT 20" --target-org {{org}}
```

## View Setup Audit Trail
```bash
sf data query --query "SELECT Action, Section, CreatedBy.Name, CreatedDate, Display FROM SetupAuditTrail ORDER BY CreatedDate DESC LIMIT 20" --target-org {{org}}
```

## Retrieve Profile Metadata
```bash
sf project retrieve start --metadata Profile:{{profileName}} --target-org {{org}}
```

## Deploy Profile Changes
```bash
sf project deploy start --metadata Profile:{{profileName}} --target-org {{org}}
```

## Notes
- Security layers (evaluated in order): Org-wide defaults → Role hierarchy → Sharing rules → Manual sharing
- OWD sets the baseline (most restrictive), then access is opened up
- Profiles control object/field CRUD; Permission Sets add on top
- Field-Level Security (FLS) controls field visibility per profile/perm set
