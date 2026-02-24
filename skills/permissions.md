# Profiles, Permission Sets & Permission Set Groups

## List Profiles
```bash
sf data query --query "SELECT Id, Name FROM Profile ORDER BY Name" --target-org {{org}}
```

## List Permission Sets
```bash
sf data query --query "SELECT Id, Name, Label, IsCustom FROM PermissionSet WHERE IsOwnedByProfile = false ORDER BY Label" --target-org {{org}}
```

## List Permission Set Groups
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, Status FROM PermissionSetGroup ORDER BY MasterLabel" --target-org {{org}}
```

## View Permission Set Details
```bash
sf data query --query "SELECT Id, Name, Label, Description FROM PermissionSet WHERE Name = '{{permSetName}}'" --target-org {{org}}
```

## View Object Permissions in a Permission Set
```bash
sf data query --query "SELECT SobjectType, PermissionsRead, PermissionsCreate, PermissionsEdit, PermissionsDelete FROM ObjectPermissions WHERE ParentId IN (SELECT Id FROM PermissionSet WHERE Name = '{{permSetName}}')" --target-org {{org}}
```

## View Field Permissions in a Permission Set
```bash
sf data query --query "SELECT SobjectType, Field, PermissionsRead, PermissionsEdit FROM FieldPermissions WHERE ParentId IN (SELECT Id FROM PermissionSet WHERE Name = '{{permSetName}}')" --target-org {{org}}
```

## Deploy Permission Set (from local metadata)
```bash
sf project deploy start --source-dir force-app/main/default/permissionsets/{{permSetName}}.permissionset-meta.xml --target-org {{org}}
```
