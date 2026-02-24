# Security Assessment

## 1. Password Policies
```bash
sf data query --query "SELECT Id, Name FROM Profile WHERE UserType = 'Standard'" --target-org {{org}}
```
Then open the password policy page for review:
```bash
sf org open --target-org {{org}} --path /lightning/setup/SecurityPolicies/home
```

### Flag Rules (check via Profile metadata retrieval)
```bash
sf project retrieve start --metadata Profile:Admin --target-org {{org}}
```
Review the retrieved profile XML for password policy settings:

| Rule | Threshold | Severity |
|------|-----------|----------|
| Password allows security question hint | `passwordQuestion = true` | CRITICAL |
| No password expiration | `passwordExpiration = 0` | CRITICAL |
| Password expiration > 90 days | `passwordExpiration > 90` | WARNING |
| Password history < 3 | `passwordHistory < 3` | WARNING |
| Minimum password length < 8 | `minimumPasswordLength < 8` | CRITICAL |
| Password complexity < 3 | `passwordComplexity < 3` | WARNING |
| No max login attempts | `maxLoginAttempts` undefined | WARNING |
| No lockout period | `lockoutInterval` undefined | WARNING |

## 2. Profile IP Range Restrictions
```bash
sf data query --query "SELECT Id, Name FROM Profile WHERE UserType = 'Standard'" --target-org {{org}}
```
For each profile, check login IP ranges in the retrieved metadata.

### Flag: Excessive IP Range (CRITICAL)
IP range spanning > 16,777,216 addresses (entire class A).

### Flag: Wide IP Range (WARNING)
IP range spanning > 100,000 addresses.

## 3. Profile Login Hour Restrictions
Check login hours in retrieved profile metadata.

### Flag: Excessive Login Hours (WARNING)
Login hours spanning > 20 hours (1200 minutes).

## 4. Org-Wide Sharing Defaults
```bash
sf org open --target-org {{org}} --path /lightning/setup/SecuritySharing/home
```

### Flag: Public Read/Write on sensitive objects (WARNING)
Objects like Account, Contact, Opportunity, Case with Public Read/Write OWD.

## 5. Setup Audit Trail (Recent Changes)
```bash
sf data query --query "SELECT Action, Section, CreatedBy.Name, CreatedDate, Display FROM SetupAuditTrail ORDER BY CreatedDate DESC LIMIT 20" --target-org {{org}}
```

### Flag: Recent Security Changes (INFO)
Any changes to sharing, profiles, or permission sets in last 7 days.

## Output Format
```
Security Assessment — {{org}}

| Check | Status | Issue | Severity |
|-------|--------|-------|----------|
| ...   | ...    | ...   | ...      |
```
