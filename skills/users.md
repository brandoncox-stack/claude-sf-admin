# User Management

## List Users
```bash
sf data query --query "SELECT Id, Name, Username, Email, IsActive, Profile.Name FROM User WHERE IsActive = true ORDER BY Name" --target-org {{org}}
```

## Get User Details
```bash
sf data query --query "SELECT Id, Name, Username, Email, IsActive, Profile.Name, UserRole.Name, LastLoginDate FROM User WHERE Username = '{{username}}'" --target-org {{org}}
```

## Deactivate User
Confirm with user first. This is a destructive action.
```bash
sf data update record --sobject User --where "Username='{{username}}'" --values "IsActive=false" --target-org {{org}}
```

## Freeze User
```bash
sf data query --query "SELECT Id FROM UserLogin WHERE UserId IN (SELECT Id FROM User WHERE Username = '{{username}}')" --target-org {{org}}
# Then update the IsFrozen field
sf data update record --sobject UserLogin --record-id {{userLoginId}} --values "IsFrozen=true" --target-org {{org}}
```

## Assign Permission Set
```bash
sf org assign permset --name {{permSetName}} --target-org {{org}} --on-behalf-of {{username}}
```

## List Permission Set Assignments for User
```bash
sf data query --query "SELECT PermissionSet.Name, PermissionSet.Label FROM PermissionSetAssignment WHERE Assignee.Username = '{{username}}'" --target-org {{org}}
```

## Notes
- Always use `--target-org` to specify which org. Ask user if not clear.
- Default org alias: check `sf org list` if user doesn't specify.
